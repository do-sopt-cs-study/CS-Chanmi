## ☀ 뮤텍스와 세마포어

프로세스 간 메시지를 전송하거나, 공유메모리를 통해 공유된 자원에 여러 개의 프로세스가 동시에 접근하면 `임계 영역 (Critical Section) 문제`가 발생할 수 있다.

따라서 공유 자원에 한 번에 하나의 프로세스만 접근할 수 있도록 제한을 두는 **동기화 방식**(상호 배제)을 취해야 한다. 그 대표적인 도구인 `뮤텍스`와 `세마포어`는 **공유된 자원의 데이터를 여러 스레드/프로세스가 접근하는 것을 막는 역할**을 한다.

<br>

## ☀ 임계 영역 (Critical Section)

> 여러 프로세스가 데이터를 공유하며 수행될 떄, **각 프로세스에서 공유 데이터를 접근하는 프로그램 코드 블록**
> 
- 여러 프로세스가 동일 자원을 동시에 참조하여 값(공유하는 변수명, 파일 등)이 오염될 가능성이 있는 영역
- 프로그래밍 시 성능 향상을 위해 임계영역을 최소화하는 설계를 해야한다.

<br>

## ☀ 뮤텍스(mutex)

> 동시 프로그래밍에서 공유 불가능한 자원의 동시 사용을 피하기 위해 사용하는 알고리즘
> 
- 임계구역을 가진 스레드들의 실행 시간이 서로 겹치지 않고 단독으로 실행되도록 하는 기술
- 한 프로세스에 의해 소유될 수 있는 Key를 기반으로 한 상호배제 기법
    - Key에 해당하는 어떤 객체(Object)가 있으며, 이 객체를 소유한 스레드/프로세스만이 공유자원에 접근 가능
- 다중 프로세스들의 공유 리소스에 대한 접근을 조율하기 위해 `동기화` 또는 `락`을 사용.
- 1개의 락만을 갖는 `Locking 메커니즘`으로 오직 하나의 스레드만이 동일한 시점에 뮤텍스를 얻어 임계 영역에 들어올 수 있음
- 오직 이 스레드만이 임계 영역에서 나갈 때 뮤텍스를 해제할 수 있다.
- 예시) 화장실이 하나만 있는 식당(열쇠)

![Untitled](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FdfJ75G%2FbtqZJ43DWsg%2FOUjUDPalDipT8rkuM7aks1%2Fimg.png
)

### 과정

- 1번 프로세스가 자원을 접근하기 위해 Key를 점유한다.
- 1번 프로세스는 키를 점유했기 때문에 공유 자원을 사용한다.
- 2번 프로세스가 공유 자원을 사용하기를 원한다.
- 2번 프로세스는 키를 점유하기 위해 대기한다.
- 3번 프로세스 또한 공유 자원을 사용하기 위해 2번 프로세스 다음 순번으로 대기한다.
- 1번 프로세스가 공유 자원을 다 사용하고 Key를 반환한다.
- 2번 프로세스는 대기하고 있다가 반환된 Key를 점유하고 공유 자원을 사용한다.

<br>

## ☀ 세마포어(Semaphore)

> 멀티 프로그래밍 환경에서 공유된 자원에 대한 접근을 제한하는 방법
> 
- 공유자원의 상태를 나타낼 수 있는 **카운터**
    - 사용하고 있는 스레드/프로세스의 수를 공통으로 관리하는 하나의 값을 이용해 상호배제를 달성한다.
    - 운영체제 또는 커널의 한 지정된 저장장치 내의 값
    - 일반적으로 비교적 긴 시간을 확보하는 리소스에 대한 이용
    - 유닉스 프로그래밍에서 세마포어는 운영체제의 리소스를 경쟁적으로 사용하는 다중 프로세스에서 행동을 조정하거나 또는 동기화하는 기술
- 공유 자원에 접근할 수 있는 프로세스의 **최대 허용치만큼 사용자가 동시 접근 가능**
- 각 프로세스는 세마포어의 값을 확인하고 변경할 수 있다.
- 자원을 사용하지 않는 상태가 될 때, 대기하던 프로세스가 즉시 자원을 사용한다.
- **이미 다른 프로세스에 의해 사용중**이라는 사실을 알게 되면, **재시도 전 일정시간** 대기
- 세마포어를 사용하는 프로세스는 **그 값을 확인**하고, **자원을 사용하는 동안에는 그 값을 변경함**으로써 다른 세마포어 사용자들이 대기하도록 해야 한다.
- 세마포어는 이진수를 사용하거나 추가적인 값을 가질 수 있다.
- 예시) 화장실 안에 여러 개의 칸이 있는 레스토랑, 화장실 입구에는 현재 화장실의 빈 칸 개수를 보여주는 전광판

![Untitled](https://3553248446-files.gitbook.io/~/files/v0/b/gitbook-legacy-files/o/assets%2F-M5HOStxvx-Jr0fqZhyW%2F-MH25VklJHGlyU_CCTs3%2F-MH2RRv7KZTokJ2fkHd5%2FCounting-Semaphore-in-OS-Operating-System.png?alt=media&token=c4c209d9-da67-4410-80d2-f5badf35cd8c
)


### 과정

- 공유 자원에 대한 최대 허용치를 정의한다. 우선, 3으로 해보겠다.
- 1번 프로세스가 공유 자원에 접근한다. 허용치는 2로 감소하였다.
- 2번 프로세스가 공유 자원에 접근한다. 허용치는 1로 감소하였다.
- 3번 프로세스가 공유 자원에 접근한다. 허용치는 0로 감소하였다.
- 4번 프로세스가 공유 자원에 접근한다. 허용치가 0이므로 대기한다.
- 2번 프로세스가 공유 자원을 다 사용하였다. 허용치는 1로 증가하였다.
- 4번 프로세스는 대기 하다 허용치가 1로 증가되어 공유 자원에 접근한다. 허용치는 다시 0으로 감소하였다.

<br>

## ☀ 뮤텍스와 세마포어의 차이

1. **동기화 대상의 개수**
    1. Mutex는 동기화 대상이 only 1개일 때 사용
    2. Semaphore는 동기화 대상이 1개 이상일 때 사용
2. **세마포어는 뮤텍스가 될 수 있지만, 뮤텍스는 세마포어가 될 수 없다.**
    1. Mutex는 0, 1로 이루어진 이진 상태를 가지므로 Binary Semaphore!
3. **Mutex는 자원 소유 가능 + 책임을 가지는 반면, Semaphore는 자원 소유 불가**
    1. 뮤텍스는 상태 0, 1 뿐이므로 Lock 가질 수 있음
4. **Mutex는 소유하고 있는 스레드만이 이 Mutex를 해제할 수 있다.**
    1. 반면, Semaphore는 Semaphore를 소유하지 않는 스레드가 Semaphore를 해제할 수 있다.
5. **Semaphore는 시스템 범위에 걸쳐 있고, 파일 시스템 상의 파일로 존재한다.**
    1. 반면, Mutex는 프로세스의 범위를 가지며 프로세스 종료될 때 자동으로 Clean up 된다.

뮤텍스와 세마포어는 모두 완벽한 기법은 아니므로, 데이터 무결성을 보장할 수 없으며 모든 교착 상태를 해결하지는 못한다.

하지만, 상호배제를 위한 기본적인 기법이며 여기에 좀 더 복잡한 매커니즘을 적용해 개선된 성능을 가질 수 있도록 하는 것이 중요하다.

<br>

## ☀ 참고 자료

[뮤텍스와 세마포어](https://incheol-jung.gitbook.io/docs/q-and-a/computer-science/undefined-1)

[[OS] 뮤텍스(Mutex)와 세마포어(Semaphore)란?](https://chelseashin.tistory.com/40)

[뮤텍스(Mutex)와 세마포어(Semaphore)의 차이](https://worthpreading.tistory.com/90)
