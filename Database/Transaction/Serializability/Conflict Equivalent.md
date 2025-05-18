---
상위 개념: "[[Conflict]]"
---
# Conflict Equivalent
		T1           T2
		read(A)        
		write(A)
						read(A)
						write(A)
		read(B)
		write(B)
						read(B)
						write(B)

위 스케줄에서 `T1.read(B)`와 `T2.write(A)`는 [충돌](Conflict)하지 않는다. 따라서 이들의 순서를 바꾸어도 전체 실행 결과에 영향을 주지 않는다.

		T1           T2
		read(A)        
		write(A)
						read(A)
		read(B)
						write(A)
		write(B)
						read(B)
						write(B)

이렇게 어떠한 스케줄 S에서 충돌이 일어나지 않는 명령어들의 순서를 바꾸어 스케줄 S'로 변경된다면, S와 S'는 충돌 동등(Conflict equivalent)하다고 말할 수 있다.