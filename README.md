메모리관리
=========

보통 4KB 단위로 묶어서 페이지 라는 하나의 행정구역을 만든다.

총 32 비트의 주소중 하위 12비트는 페이지 내에서의 주소를 나타내게 되는 것.

<br/>

<br/>

1.주소 바인딩
----


##### 논리적 주소(가상 주소)

프로그램이 실행을 위해 메모리에 적재되면 그 프로세스를 위한 독자적인 주소 공간이 생성.

<br/>

#### 물리적 주소

---



물리적 메모리에 실제로 올라가는 위치.

> CPU가 기계어 명령을 수행하기 위해 논리적 주소를 통해 메모리 참조를 하게 되면 해당 논리적 주소가 물맂거 메모리의 어느 위치에 매핑되는지 확인해야 한다. 이렇게 프로세스의 논리적 주소를 물리적 메모리 주소로 연결시켜주는 작업을 주소 바인딩( address binding) 이라고 한다.




###### 주소 바인딩 방식

- 컴파일 타임 바인딩
- 로드 타임 바인딩
- 실행시간 바인딩

<br/>

#### 실행시간 바인딩

---



CPU가 특정 프로세스의 논리적 주소를 참조하려고 할 때 MMU 기법은 그 주소값에 기준 레지스터의 값을 더해 물리적 주소값을 얻어낸다.

> 기준 레지스터는 재배치 레지스터라고도 부르며 그 프로세스의 물리적 메모리 시작 주소를 가지고 있다.

MMU 기법에서는 프로그램의 주소 공간이 물리적 메모리의 한 장소에 **연속적으로 적재**되는 것으로 가정한다.

따라서 그 프로그램이 적재되는 물리적 메모리상의 시작 주소만 알면 주소 변환을 쉽게 할 수 있다.

MMU 기법에서는 문맥교환으로 CPU에서 수행 중인 프로세스가 바뀔 때마다 재배치 레지스터의 값을 그 프로세스에 해당되는 값으로 재설정함으로써 각 프로세스에 맞는, 서로 다른 100번지 위치에 접근하는 것을 지원.

<br/>

*고려할 사항?*

> 다중 프로그래밍 환경에서 물리적 메모리 안에는 여러 개의 프로세스가 동시에 올라가 있는 경우가 대부분이다.
>
> 따라서  위의 MMU 방식을 사용하여 주소 변환을 했을 경우 CPU가 요청한 논리적 주속밧과 재배치 레지스터 안에 있는 값을 더한 결과가 해당 프로세스의 주소 공간을 벗어나는 경우가 발생할 수 있다.
>
> 이것을 방지하기 위해 **한계 레지스터(limit register)**라는 또 하나의 레지스터를 사용.

<br/>

#### 한계 레지스터

---



프로세스가 자신의 주소 공간을 넘어서는 메모리 참조를 하려고 하는지 체크하는 용도로 사용.

현재 CPU에서 수행 중인 프로세스의 논리적 주소의 최댓값, 즉 그 프로세스의 크기를 담고 있다.

<br/>

<br/>

2.메모리 관리와 관련된 용어
---


+ [동적로딩](#동적로딩)
+ [동적연결](#동적연결)
+ [중첩](#중첩)
+ [스와핑](#스와핑)



#### 동적로딩

---



프로세스의 주소 공간 전체를 메모리에 다 올려놓는 것이 아니라 메모리는 좀 더 효율적으로 사용하기 위해 해당 부분이 불릴 때 그 부분만을 메모리에 적재하는 방식.

<br/>

#### 동적연결

---



컴파일을 통해 생성된 목적 파일과 라이브러리 파일 사이의 연결을 프로그램의 실행 시점까지 지연시키는 기법.

> **정적연결?** 
>
> 프로그래머가 작성한 코드와 라이브러리 코드가 모두 합쳐져서 실행파일이 생성.



> ※ 목적파일(object file)?
>
> 프로그래머가 작성한 소스 코드를 컴파일하여 생성된 파일.
>
> ※ 라이브러리 파일(library file)?
>
> 이미 컴파일된 파일.

<br/>

동적연결을 가능하게 하기 위해 실행파일의 라이브러리 호출 부분에 해당 라이브러리의 위치를 찾기 위한 스텁(stub)이라는 작은 코드를 둔다.

스텁을 통해 해당 라이브러리가 메모리에 이미 존재 하는지 살펴보고 그럴 경우 그 주소의 메모리 위치에서 직접 참조하며, 그렇지 않을 경우 디스크에서 동적 라이브러리 파일을 찾아 메모리로 적재한 후수행한다.

<br/>

#### 중첩

---



프로세스의 주소 공간을 분할해 실제 필요한 부분만을 메모리에 적재하는 기법.

<br/>

_동적로딩과 차이점?_

동적로딩은 메모리에 더 많은 프로세스를 동시에 올려놓고 실행하기 위한 용도인 반면, 중첩은 단일 프로세스만을 메모리에 올려놓는 환경에서 메모리 용량보다 큰 프로세스를 실행하기 위한 어쩔 수 없는 선택.

<br/>

중첩은 운영체제의 지원 없이 프로그래머에 의해 구현되어야 했으며, 작은 공간의 메모리를 사용하던 초창기 시스템에서 프로그래머가 손수 구현 했다 해서 수작업 중첩이라고도 부른다.

<br/>

#### 스와핑

---



메모리에 올라온 프로세스의 주소 공간 전체를 디스크의 스왑 영역에 일시적으로 내려놓는 것을 말한다.

스와핑의 가장 중요한 역할은 메모리에 존재하는 프로세스의 수를 조절하는 것.

<br/>

_Swap in & Swap out_

> - swap in: 디스크에서 메모리로 올리는 작업.
> - swap out: 메모리에서 디스크로 내리는 작업.



<br/>



#### 스와핑이 일어나는 과정

---

1. 스와퍼라고 불리는 중기 스케줄러에 의해 스왑 아웃시킬 프로세스를 선정.
2. 스왓 아웁 대상으로 선정된 프로세스에 대해서는 현재 메모리에 올라가 있는 주소 공간의 내용을 통째로 디스크 스왑 영역에 스왑 아웃시킨다.



<br/>



#### 컴파일 타임 바인딩, 로드 타임 바인딩, 실행시간 바인딩에서 스와핑

---

+ 컴파일 타임 바인딩과 로드 타임 바인딩에서는 스왑 아웃된 프로세스가 다시 스왑 인될 때에는 **원재 존재하던 메모리 위치**로 다시 올라가야 한다.
+ 반면 실행시간 바인딩 기법에서는 추후 빈 메모리 영역 아무 곳에나 프로세스를 올릴 수 있다.

<br/>

<br/>

3. 물리적 메모리의 할당 방식
---

물리적 메모리는 운영체제 상주 영역과 사용자 프로세스 영역으로 나뉘어 사용된다. 

+ 운영체제 상주 영역: 인터럽트 벡터와 함께 물리적 메모리의 낮은 주소 영역을 사용하며, 운영체제 커널이 이곳에 위치하게 된다.
+ 사용자 프로세스 영역: 물리적 메모리의 높은 주소 영역을 사용하며, 여러 사용자 프로세스들이 이곳에 적재되어 실행된다.

<br/>

#### 사용자 프로세스 영역의 관리 방법

---

+ [1.연속할당](#1.연속할당)
+ [2.불연속할당](#2.불연속할당)

<br/>

#### 1.연속할당

---

각각의 프로세스를 물리적 메모리의 **연속적인 공간**에 올리는 방식.

연속할당 방식에서는 물리적 메모리를 다수의 분할로 나누어 하나의 분할에 하나의 프로세스가 적재 되도록 한다.

분할을 관리하는 방식에 따라 [고정분할(fixed partition allocation)](#고정분할)방식과 [가변분할(variable partition allocation)](#가변분할) 방식으로 다시 나누어 볼 수 있다.

<br/>



##### _고정분할_

---

물리적 메모리를 고정된 크기의 분할로 미리 나누어두는 방식.

<br/>

##### _가변분할_

---

분할을 미리 나누어놓지 않은 채 프로그램이 실행되고 종료되는 순서에 따라 분할을 관리하는 방식.





