
# 발생 이유

MySQL에서는 종류 불문 쿼리를 처리할 때 대상 데이터들을 메모리에 올린다. 이 메모리 공간의 크기는 innodb_buffer_pool 이 결정한다. 만약 대상 데이터의 크기가 버퍼의 사이즈보다 크다면, 버퍼 풀에서는 처리된 데이터를 내보내고 새 데이터를 Disk로부터 가져오는 과정을 반복한다. 이 동작을 하는 과정에서 해당 테이블에 데이터 조작 명령어가 들어온다면 일관성이 깨질 수 있기 때문에, 테이블에다가 lock을 건다.

이 때 DB는 내부적으로 lock_table을 통해 락을 관리하는데, 이 테이블이 관리할 수 있는 락의 개수를 초과한다면 1206 에러가 발생한다.

# 해결 방안

목표 구간을 나누어 쿼리를 처리한다.

```sql
CREATE PROCEDURE batch_delete() 
BEGIN 
	-- 시작 및 종료 ID 설정 
	SET @start_id = 150000000; 
	SET @end_id = 300000000;
	
	-- 한 번의 배치에서 삭제할 레코드 수 
	SET @batch_size = 10000; 
	
	-- 삭제 루프 시작 
	batch_delete_loop: LOOP 
	
	-- 배치 삭제 
	SET @delete_query = CONCAT('DELETE FROM comment WHERE id BETWEEN ', @start_id, ' AND ', @start_id + @batch_size); PREPARE stmt FROM @delete_query; EXECUTE stmt; DEALLOCATE PREPARE stmt; 
	
	-- 시작 ID 업데이트 
	SET @start_id = @start_id + @batch_size + 1; 
	
	-- 종료 조건 확인: 시작 ID가 설정한 종료 ID를 초과하면 루프 종료 
	IF @start_id > @end_id 
		THEN LEAVE batch_delete_loop; 
		END IF; 
	END LOOP batch_delete_loop; 
END; 


CALL batch_delete();
```


### Reference
https://greypencil.tistory.com/143
https://medium.com/@yangcar/exceeds-the-lock-table-size-in-mysql-12afbaa46e52