---
상위 개념: "[[CFS/Completely Fair Scheduling]]"
---
# Time Accounting
CFS를 구현하기 위해 모든 스케줄러는 프로세스가 실행된 시간을 기록해야 한다. 이 작업을 시간 회계(Time Accounting)이라고 한다.

CFS는 `<linux/sched.h>`에 정의된 `sched_entity`라는 스케줄러 엔티티 구조체를 사용해 프로세스 회계 정보를 관리한다.

```c
struct sched_entity {
        struct load_weight          load;
        struct rb_node              run_node;
        struct list_head            group_node;
        unsigned int                on_rq;
        u64                         exec_start;
        u64                         sum_exec_runtime;
        u64                         vruntime;
        u64                         prev_sum_exec_runtime;
        u64                         last_wakeup;
        u64                         avg_overlap;
        u64                         nr_migrations;
        u64                         start_runtime;
        u64                         avg_wakeup;
        /* CONFIG_SCHEDSTATS가 설정된 경우에만 활성화되는 통계 필드는 생략 */
};
```

스케줄러 엔티티 구조체는 프로세스 디스크립터의 멤버변수 `se`로 포함되어 있다.

`vruntime` 변수에는 프로세스의 가상 런타임이 저장된다. 이는 실제 실행 시간을 가중치(nice)로 정규화한 값이다. 이는 지금까지 얼마나 수행되었는지를 나타내는 값이다.

`kernerl/sched_fair.c`에 정의된 update_curr() 함수는 회계 작업을 수행한다.

```c
static void update_curr(struct cfs_rq *cfs_rq)
{
        struct sched_entity *curr = cfs_rq->curr;
        u64 now = rq_of(cfs_rq)->clock;
        unsigned long delta_exec;

        if (unlikely(!curr))
                return;

        /*
         * 마지막으로 부하를 갱신한 이후 현재 태스크가 실행한 시간(32비트에서도 오버플로 나지 않는다)을 구한다.
         */
        delta_exec = (unsigned long)(now - curr->exec_start);
        if (!delta_exec)
                return;

        __update_curr(cfs_rq, curr, delta_exec);
        curr->exec_start = now;

        if (entity_is_task(curr)) {
                struct task_struct *curtask = task_of(curr);

                trace_sched_stat_runtime(curtask, delta_exec, curr->vruntime);
                cpuacct_charge(curtask, delta_exec);
                account_group_exec_runtime(curtask, delta_exec);
        }
}
```

`update_curr()`은 현재 프로세스의 실행 시간을 계산해 `delta_exec`에 저장한 뒤, 그 값을 실행 가능한 프로세스 수에 따라 가중치를 적용하는 `__update_curr()`에 전달한다. 이후 현재 프로세스의 `vruntime`을 가중치가 적용된 값 만큼 증가시킨다.

```c
static inline void __update_curr(struct cfs_rq *cfs_rq, struct sched_entity *curr,
                                 unsigned long delta_exec)
{
        unsigned long delta_exec_weighted;

        schedstat_set(curr->exec_max, max((u64)delta_exec, curr->exec_max));

        curr->sum_exec_runtime += delta_exec;
        schedstat_add(cfs_rq, exec_clock, delta_exec);
        delta_exec_weighted = calc_delta_fair(delta_exec, curr);

        curr->vruntime += delta_exec_weighted;
        update_min_vruntime(cfs_rq);
}
```
`update_curr()`은 시스템 타이머가 주기적으로 호출할 뿐만 아니라, 프로세스가 실행 가능 상태가 되거나 블록되어 실행 불가능해질 때마다 호출된다. 이런 방식으로 `vruntime`은 특정 프로세스가 얼마나 실행되었는지를 나타내며, 스케줄러는 이를 참고하여 다음에 어떤 프로세스를 실행할지 결정한다.

> [!info] vruntime의 단위
> vruntime은 나노초 단위를 가지며, 이는 타이머 틱과는 별도의 단위로 관리된다.

