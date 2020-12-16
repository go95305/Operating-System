CPU 스케줄링
===

+ CPU 버스트

+ I/O 버스트

+ CPU 바운드 프로세스

+ I/O 바운드 프로세스(바람직)

<br/>

---



+ 선입선출 스케줄링
+ 최단작업 우선 스케줄링(SJF)
+ 우선순위 스케줄링
+ 라운드 로빈 스케줄링
+ 멀티레벨 큐
+ 멀티레벨 피드백 큐
+ 다중처리기 스케줄링
+ 실시간 스케줄링

<br/>

### 선입선출 스케줄링

프로세스가 준비 큐에 도착한 시간 순서대로 CPU를 할당하는 방식.

<br/>

#### 최단작업 우선 스케줄링(SJF)

**<u>CPU버스트</u>**가 가장 짧은 프로세스에게 제일 먼저 CPU를 할당.

+ 선점형
+ 비선점형

<br/>

### 우선순위 스케줄링

프로세스들 중 우선순위가 가장 높은 프로세스에게 제일 먼저 CPU를 할당

+ 선점형
+ 비선점형

<br/>

### 라운드 로빈 스케줄링

CPU사용시간을 제한.

시간이 경과하면 CPU를 회수후 다른 프로세스에게 CPU할당.

<br/>

### 멀티레벨 큐

준비 큐를 여러 개로 분할해 관리.

+ 전위 큐: 대화형 작업(라운드 로빈 스케줄링)
+ 후위 큐: 계산 위주의 작업(FCFS)

<br/>

### 멀티레벨 피드백 큐

하나의 큐에서 다른 큐로 이동이 가능.

<br/>

### 다중처리기 스케줄링

여러 CPU가 알아서 준비큐에서 프로세스를 꺼내간다.

+ 대칭형
+ 비대칭형

<br/>

### 실시간 스케줄링

정해진 데드라인안에 반드시 작업을 처리.

+ 경성 실시간 시스템
+ 연성 실시간 시스템
<br/>

----

<br/>

프로세스 관리
===

### 프로세스 문맥의 분류

+ 하드웨어 문맥 - 프로그램 카운터, 각종 레지스터 값.
+ 프로세스 주소 공간 - 코드, 데이터, 스택으로 구성되는 자기 자신만의 독자적인 주소 공간.
+ 커널상의 문맥

<br/>

### 스케줄러

어떤 프로세스에게 자원을 할당할지를 결정하는 운영체제 커널의 코드를 지칭

+ 장기 스케줄러
+ 단기 스케줄러
+ 중기 스케줄러

<br/>

### 프로세스의 생성

#### fork()

자식 프로세스를 생성할 때 부모 프로세스의 내용을 그대로 복제 생성.

프로세스 ID를 제외한 모든 정보를 그대로 복사.

동일한 주소 공간.

+ fork()의 결과값이 0 이면 자식
+ fork()의 결과값이 0보다 크면 부모

<br/>

#### exec()

생성된 자식 프로세스에게 새로운 주소 공간을 제공.

fork()된 자식을 사용하려면 exec()를 사용해야한다.

<br/>

### IPC(Inter-Process Communication)

서로 다른 프로세스 간에 발생하는 통신.

+ 메시지 전달
  - 간접통신
  - 직접통신
+ 공유메모리

<br/>

----

:balance_scale: 메모리관리
=========

보통 4KB 단위로 묶어서 페이지 라는 하나의 행정구역을 만든다.

총 32 비트의 주소중 하위 12비트는 페이지 내에서의 주소를 나타내게 되는 것.

<br/>

<br/>

주소 바인딩
----


##### 논리적 주소(가상 주소)

프로그램이 실행을 위해 메모리에 적재되면 그 프로세스를 위한 독자적인 주소 공간이 생성.

<br/>

### 물리적 주소

---



물리적 메모리에 실제로 올라가는 위치.

> CPU가 기계어 명령을 수행하기 위해 논리적 주소를 통해 메모리 참조를 하게 되면 해당 논리적 주소가 물맂거 메모리의 어느 위치에 매핑되는지 확인해야 한다. 이렇게 프로세스의 논리적 주소를 물리적 메모리 주소로 연결시켜주는 작업을 주소 바인딩( address binding) 이라고 한다.




###### 주소 바인딩 방식

- 컴파일 타임 바인딩
- 로드 타임 바인딩
- 실행시간 바인딩

<br/>

### 실행시간 바인딩

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

### 한계 레지스터

---



프로세스가 자신의 주소 공간을 넘어서는 메모리 참조를 하려고 하는지 체크하는 용도로 사용.

현재 CPU에서 수행 중인 프로세스의 논리적 주소의 최댓값, 즉 그 프로세스의 크기를 담고 있다.

<br/>

<br/>

메모리 관리와 관련된 용어
---


+ [동적로딩](#동적로딩)
+ [동적연결](#동적연결)
+ [중첩](#중첩)
+ [스와핑](#스와핑)



### 동적로딩

---



프로세스의 주소 공간 전체를 메모리에 다 올려놓는 것이 아니라 메모리는 좀 더 효율적으로 사용하기 위해 해당 부분이 불릴 때 그 부분만을 메모리에 적재하는 방식.

<br/>

### 동적연결

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

### 중첩

---



프로세스의 주소 공간을 분할해 실제 필요한 부분만을 메모리에 적재하는 기법.

<br/>

_동적로딩과 차이점?_

동적로딩은 메모리에 더 많은 프로세스를 동시에 올려놓고 실행하기 위한 용도인 반면, 중첩은 단일 프로세스만을 메모리에 올려놓는 환경에서 메모리 용량보다 큰 프로세스를 실행하기 위한 어쩔 수 없는 선택.

<br/>

중첩은 운영체제의 지원 없이 프로그래머에 의해 구현되어야 했으며, 작은 공간의 메모리를 사용하던 초창기 시스템에서 프로그래머가 손수 구현 했다 해서 수작업 중첩이라고도 부른다.

<br/>

### 스와핑

---



메모리에 올라온 프로세스의 주소 공간 전체를 디스크의 스왑 영역에 일시적으로 내려놓는 것을 말한다.

스와핑의 가장 중요한 역할은 메모리에 존재하는 프로세스의 수를 조절하는 것.

<br/>

_Swap in & Swap out_

> - swap in: 디스크에서 메모리로 올리는 작업.
> - swap out: 메모리에서 디스크로 내리는 작업.



<br/>



### :1234: 스와핑이 일어나는 과정

---

1. 스와퍼라고 불리는 중기 스케줄러에 의해 스왑 아웃시킬 프로세스를 선정.
2. 스왓 아웁 대상으로 선정된 프로세스에 대해서는 현재 메모리에 올라가 있는 주소 공간의 내용을 통째로 디스크 스왑 영역에 스왑 아웃시킨다.



<br/>



### 컴파일 타임 바인딩, 로드 타임 바인딩, 실행시간 바인딩에서 스와핑

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

### 사용자 프로세스 영역의 관리 방법

---

+ [연속할당](#연속할당)
+ [불연속할당](#불연속할당)

<br/>

### 연속할당

---

각각의 프로세스를 물리적 메모리의 **연속적인 공간**에 올리는 방식.

연속할당 방식에서는 물리적 메모리를 다수의 분할로 나누어 하나의 분할에 하나의 프로세스가 적재 되도록 한다.

분할을 관리하는 방식에 따라 [고정분할(fixed partition allocation)](#고정분할)방식과 [가변분할(variable partition allocation)](#가변분할) 방식으로 다시 나누어 볼 수 있다.

<br/>



### 고정분할

---

물리적 메모리를 주어진 개수만큼의 영구적인 분할로 미리 나누어두고 **각 분할에 하나의 프로세스**를 적재해 실행시킬 수 있게 한다.

분할의 크기는 모두 동일하게 할 수도 있고 서로 다르게 할 수도 있다.

<u>외부조각</u>과 <u>내부조각</u>이 발생할 수 있다.  



#### 외부조각

프로그램의 크기보다 분할의 크기가 작은 경우 해당 분할이 비어 있는데도 불구하고 프로그램을 적재하지 못하기 때문에 발생하는 메모리 공간.

#### 내부조각

프로그램의 크기보다 분할의 크기가 큰 경우 해당 분할에 프로그램을 적재하고 남는 메모리 공간을 뜻한다. 내부 조각은 특정 프로그램에 이미 배당된 공간으로 볼 수 있으므로 내부조각에 수용할 수 있는 충분히 작은 크기의 프로그램이 있다 해도 공간을 활용할 수 없어 메모리가 낭비 된다.

<br/>

### 가변분할

---

메모리에 적재되는 프로그램의 크기에 따라 분할의 크기, 개수가 동적으로 변하는 방식.

<br/>

#### 동적 메모리 할당 문제

주소 공간의 크기가 n인 프로세스를 메모리에 올릴 때 물리적 메모리 내 가용 공간 중 어떤 위치에 올릴 것인지 결정하는 문제.

+ [최초적합(first-fit)](#최초적합)
+ [최적적합(best-fit)](#최적적합)
+ [최악적합(worst-fit)](#최악적합)

<br/>

#### :one: 최초적합

+ 가용 공간 중 가장 먼저 찾아지는 곳에 프로세스를 할당.
+ 가용 공간이 프로그램 크기보다 작으면 건너뛰고, 그렇지 않으면 가용 공간이 최초로 발견되면 그 공간에 프로그램을 올린다.
+ 완전 탐색을 하지 않으므로 시간적인 측면에서 효율적.

<br/>

#### :+1: 최적적합

+ 가장 작은 가용 공간을 찾아 그곳에 새로운 프로그램을 할당.
+ 완전 탐색을 해야하므로 시간적 오버헤드가 발생하고 다수의 매우 작은 가용 공간들이 생성될 수 있다.
+ 공간적인 측면에서 효율적.

<br/>

#### :disappointed: 최악적합

+ 가용 공간 중 가장 크기가 큰 곳에 새로운 프로그램을 할당.
+ 완전탐색을 해야하므로 시간적 오버헤드가 발생.
+ 상대적으로 더 큰 프로그램을 담을 수 있는 가용 공간을 빨리 소진한다는 문제점이 있다.

<br/>

#### 컴팩션

한편 가변분할 방식에서 발생하는 외부조각 문제를 해결하기 위한 방법으로 컴팩션(Compaction)이라는 것이 있다. 컴팩션은 물리적 메모리 중에서 프로세스에 의해 사용 중인 메모리 영역을 한쪽으로 몰고 가용 공간들을 다른 한쪽으로 모아서 하나의 큰 가용 공간을 만드는 방법.

효율적인 메모리 공간의 사용을 위해 수행 중인 프로세스의 물리적 메모리 위치를 옮겨야 하므로 프로그램의 실행 도중에 프로세스의 주소가 동적으로 바뀔 수 있는 실행시간 바인딩 방식이 지원되는 환경에서만 수행될 수 있다.

<br/>

<br/>



### 불연속할당

---

하나의 프로세스가 물리적 메모리의 여러 위치에 분산되어 올라갈 수 있는 메모리 할당 기법을 말한다.

+ [페이징 기법](#페이징 기법)
+ [세그먼테이션](#세그먼테이션)
+ [페이지드 세그먼테이션](#페이지드 세그먼테이션)

<br/>

### :page_facing_up: 페이징 기법

---

프로세스의 주소 공간을 **동일한 크기의 페이지 단위**로 나누어 물리적 메모리의 서로 다른 위치에 페이지들을 저장하는 방식.

+ 물리적 메모리를  페이지와 동일한 크기의 프레임으로 미리 나누어둔다.
+ 메모리에 올리는 단위가 동일한 크기의 페이지 단위이므로, 메모리를 같은 크기로 미리 분할해두더라도 빈 프레임이 있으면 어떤 위치이든 사용될 수 있기 때문이다.
+ 연속할당에서 발생했던 동적 메모리 할당 문제가 발생하지 않는다.
+ 외부조각 문제가 발생하지 않는다.
+ 내부조각이 발생할 수는 있다.

<br/>

#### 페이징 기법 순서

1. [주소 변환 기법](#1주소-변환-기법)

2. [페이지 테이블의 구현](#2페이지-테이블의-구현)

3. [계층적 페이징](#3계층적-페이징)

4. [역페이지 테이블](#4역페이지-테이블)

5. [공유 페이지](#5공유-페이지)

6. [메모리 보호](#6메모리-보호)

<br/>

#### 1.주소 변환 기법

페이지 번호와 페이지 오프셋으로 나누어 주소 변환에 사용한다.

페이지 번호는 각 페이지별 주소 변환 정보를 담고 있는 페이지 테이블 접근 시 인덱스로 사용.

페이지 오프셋은 하나의 페이지 내에서의 변위를 알려준다. 기준 주소값에 변위를 더함으로써 요청된 논리적 주소에 대응하는 물리적 주소를 얻을 수 있다.

<br/>

---

#### 2.페이지 테이블의 구현

물리적 메모리에 위치. 현재 CPU에서 실행 중인 프로세스의 페이지 테이블에 접근하기 위해 **기준 레지스터**와 **페이지 테이블 길이 레지스터**가 사용.

> 페이지 테이블 기준 레지스터: 메모리 내에서의 페이지 테이블의 시작 위치. 페이지 테이블 길이 레지스터는 페이지 테이블의 크기를 보관한다.

<br/>

페이징 기법에서의 메모리 접근 연산은 주소 변환을 위해 페이지 테이블에 접근하는 것과, 변환된 주소에서 실제 데이터에 접근하는 것, 이렇게 **두 번의 메모리 접근을 필요로 한다.**  



> TLB: 페이지 테이블 접근 오버헤드를 줄이고 메모리의 접근 속도를 향상시키기 위한 고속의 주소 변환용 하드웨어 캐시.
>
> 비용이 매우 비싸므로 빈번히 참조되는 페이지에 대한 주소 변환 정보만을 담게 된다.



#### TLB와 페이지 테이블의 구성 차이

+ 페이지 테이블에는 하나의 프로세스를 구성하는 모든 페이지에 대한 주소 변환 정보가 페이지 번호에 따라 **<u>순차적</u>**으로 들어 있기 때문에, 페이지 번호가 주어지면 테이블의 시작 위치에서 페이지 번호만큼 떨어진 항목에 곧바로 접근해 해당 페이지에 대응되는 프레임을 얻을 수 있다.
+ 반면, TLB를 사용한 주소 변환의 경우에는 **프로세스의 모든 페이지에대한 주소 변환 정보를 TLB가 가지고 있지 않기 때문에** 페이지 번호와 이에 대응하는 프레임 번호가 쌍으로 저장되어야 한다.
+ 또한 TLB를 통한 주소 변환을 위해서는 해당 페이지에 대한 주소 변환 정보가 TLB에 있는지 확인 하기 위해 TLB의 모든 항목(entry)을  찾아봐야 하는 오버헤드가 발생. 이러한 오버헤드를 줄이기 위해 병렬탐색이 가능한 **연관 레지스터**를 사용.

<br/>

#### 연관 레지스터를 사용할 때 평균적인 메모리 접근시간

<br/>

> EAT = (1+e)a + (2+e)(1-a)  
>
> =2+e-a



<br/>

>  (1+e)항은 요청된 페이지의 주소 변환 정보가 TLB에 존재하는 경우.

>  e는 TLB로부터 직접 물리적 메모리 주소를 얻는 데 소요되는 시간.

>  1+e는 실제 원하는 데이터에 접근하기 위해 한 번의 메모리 접근이 필요하므로 1+e의 시간이 소요.

> 여기서 TLB에서 찾아지는 비율인 a를 곱하면 첫 번째 항을 얻을 수있다.

> (2+e)(1-a)는 요청된 페이지의 주소 변환 정보가 TLB에 존재하지 않는 경우

> e는 요청된 페이지의 주소 변환 정보가 TLB에  있는지 확인하는 시간.

> 2+e는 페이지 테이블에 접근하는 시간과 실제 원하는 데이터에 접근하는 시간.

> 1-a는 TLB에서 찾지 못하는 비율.

> 평균 메모리 접근시간 **2+e-a**

---



<br/>

#### 3.계층적 페이징

현대 대부분의 프로그램은 4GB의 주소 공간 중 지극히 일부분만을 사용하므로 페이지 테이블을 위한 메모리의 사용은 심각한 공간 낭비.

따라서 페이지 테이블에 사용되는 메모리 공간의 낭비를 줄이기 위해 **<u>2단계 페이징 기법을 사용</u>**.

<br/>

#### 2단계 페이징 기법

주소 변환을 위해 외부 페이지 테이블과 내부 페이지 테이블의 두 단계에 걸친 페이지 테이블을 사용.

사용되지 않는 주소 공간에 대해서는 외부 페이지 테이블의 항목을 NULL로 설정하며, 여기에 대응하는 내부 페이지 테이블을 생성하지 않는 것.

메모리 공간을 줄여 공간적인 이득을 볼 수 있지만, 주소 변환을 위해 접근해야 하는 페이지 테이블의 수가 증가하므로 시간적인 손해가 뒤따르게 된다.

<br/>

프로세스의 논리적 주소를 두 종류의 페이지 번호(p1,p2)와 페이지 오프셋(d)으로 구분한다. p1은 외부 페이지 테이블의 인덱스, p2는 내부 페이지 인덱스.

1. 외부 페이지 테이블로부터 p1만큼 떨어진 위치에서 내부 페이지 테이블 주소를 얻는다.
2. 내부 페이지 테이블로부터 p2만큼 떨어진 위치에서 요청된 페이지가 존재하는 프레임의 위치를 얻게 된다.
3. 해당 프레임으로부터 d만큼 떨어진 곳에서 원하는 정보에 접근할 수 있게 된다.

<br/>

---

#### 4.역페이지 테이블

페이지 테이블로 인한 메모리 공간의 낭비가 심한 이유는 모든 프로세스의 모든 페이지에 대해 페이지 테이블 항목을 다 구성해야 하기 때문이다. 이 문제를 해결하기 위한 대안으로 역페이지 테이블 기법이 사용된다.

역페이지 테이블 기법은 물리적 메모리의 페이지 프레임 하나당 페이지 테이블에 하나씩의 항목을 두는 방식.

즉, 각 프로세스마다 페이지 테이블을 두지 않고, 시스템 전체에 페이지 테이블을 하나만 두는 방법.

즉 페이지 테이블의 각 항목은 프로세스 번호(pid)와 그 프로세스 내의 논리적 페이지 번호(p)를 담고 있게 된다.

<br/>

---

#### 5.공유 페이지

공유 코드를 담고 있는 페이지.

> :grey_question:**공유 코드?**
>
> 메모리 공간의 효율적인 사용을 위해 여러 프로세스에 의해 공통으로 사용될 수 있도록 작성된 코드.

공유 페이지는 여러 프로세스에 의해 공유되는 페이지이므로 물리적 메모리에 하나만 적재되어 메모리를 좀 더 효율적으로 사용할 수 있게 한다.

<br/>

---

#### 6.메모리 보호

페이지 테이블의 각 항목에는 주소 변환 정보뿐 아니라 메모리 보호를 위한 보호비트와 유효-무효 비트를 두고 있다.

> :grey_question:**보호비트**
>
> 각 페이지에 대한 접근 권한의 내용을 담고 있다. 보호비트는 각 페이지에 대해 읽기-쓰기/읽기전용 등의 접근 권한을 설정하는 데에 사용된다.
>
> 
>
> :grey_question:**유효-무효 비트**
>
> 해당 페이지의 내용이 유효한지에 대한 내용을 담고 있다.

<br/>

### :page_facing_up: 세그먼테이션

---

프로세스의 주소 공간을 의미 단위의 세그먼트로 나누어 물리적 메모리에 올리는 기법.

세그먼트는 특정 크기 단위로 나눈 것이 아니라 의미를 가질 수 있는 논리적인 다뉘로 나눈 것이기에 그 크기가 균일하지 않다.

논리적 주소가 **<세그먼트 번호, 오프셋>** 으로 나뉘어 사용된다.

> :grey_question:**세그먼트 번호**
>
> 해당 논리적 주소가 프로세스 주소 공간 내에서 몇 번째 세그먼트에 속하는지를 나타낸다.
>
> 
>
> :grey_question:**오프셋**
>
> 세그먼트 내에서 얼마만큼 떨어져 있는지에 대한 정보.

<br/>

세그먼테이션 기법에서는 주소 변환을 위해 세그먼트 테이블을 사용한다. 이때 세그먼트 테이블의 각 항목은 기준점과 한계점을 가지고 있다.

기준점은 물리적 메모리에서 그 세그먼트의 시작 위치를 나타내고, 한계점은 그 세그먼트의 길이.

> :grey_question:**세그먼트 테이블 기준 레지스터**
>
> 현재 CPU에서 실행 중인 프로세스의 세그먼트 테이블이 메모리의 어느 위치에 있는지 그 시작 주소를 담고 있다.
>
> 
>
> :grey_question:**세그먼트 테이블 길이 레지스터**
>
> 그 프로세스의 주소 공간이 총 몇 개의 세그먼트로 구성되는지, 즉 세그먼트의 개수를 나타낸다.

<br/>

#### :family_man_boy_boy: 공유 세그먼트

이 세그먼트를 공유하는 모든 프로세스의 주소 공간에서 동일한 주소에 위치해야 한다.

<br/>

#### 외부조각 발생?

세그먼테이션 기법에서는 프로그램을 의미 단위로 나누기 때문에 세그먼트의 길이가 균일하지 않다. 따라서 물리적 메모리 관리에서 외부조각이 발생하게 되며, 세그먼트를 어느 가용 공간에 할당할 것인지 결정하는 문제가 발생.

<br/>

### :pager:페이지드 세그먼테이션

---

프로그램을 의미 단위의 세그먼트로 나눈다.

+ 세그먼트는 동일한 크기 페이지들의 집합으로 구성.(외부조각 해결)
+ 세그먼트 단위로 프로세스 간의 공유나 프로세스 내의 접근 권한 보호가 이루어지도록 함으로써 페이징 기법의 약점을 해소.

<br/>

페이지는 세그먼테이션 기법에서는 주소 변환을 위해 외부의 세그먼트 테이블과 내부의 페이지 테이블, 이렇게 두 단계의 테이블을 이용한다.

---



<br/>

<br/>

:slot_machine: 가상 메모리
===

가상메모리는 프로세스마다 각각 0번지부터의 주소 공간을 가지게 되며, 이들 공간 중 일부는 물리적 메모리에 적재되고 일부는 디스크의 스왑 영역에 존재하게 된다.

<br/>

### 가상메모리 기법

---

프로세스의 주소 공간을 메모리에 적재하는 단위에 따라 가상 메모리 기법은 **요구 페이징**  방식과 **요구 세그먼테이션** 방식으로 구현될 수 있다.

+ [요구 페이징](#요구 페이징)
+ [요구 세그먼테이션](#요구  세그먼테이션)

<br/>

### 요구 페이징

---

프로그램 실행 시 프로세스를 구성하는 모든 페이지를 한꺼번에 메모리에 올리는 것이 아니라 당장 사용될 페이지만을 올리는 방식.

#### 예시

냉장고에 몇일 치 음식을 한번에 보관하는게 아니라 매일 매끼먹을 것만 따로 구매해서 보관.

<br/>

#### 요구페이징의 장점

+ 메모리 사용량이 감소.
+ 프로세스 전체를 메모리에 올리는데 소요되는 입출력 오버헤드도 줄어든다.
+ 프로그램을 구성하는 페이지 중 일부만을 페이지에 적재하게 되므로 물리적 메모리의 용량보다 큰 프로그램도 실행할 수 있게 되기 때문이다.

<br/>

#### :frowning: 유효-무효 비트?

프로세스가 실행되기 전에는 모든 페이지의 우효-무효 비트가 무효값으로 초기화 되어 있지만, 특정 페이지가 참조되어 메모리에 적재되는 경우 해당 페이지의 유효-무효 비트는 유효값으로 바뀌게 된다.

그리고 메모리에 적재되어 있던 페이지가 디스크의 스왑 영역으로 쫓겨날 때에는 유효-무효 비트가 다시 무효값을 가지게 된다.

<br/>

#### 페이지 부재

CPU가 참조하려는 페이지가 현재 메모리에 올라와 있지 않아 유효-무효 비트가 무효로 세팅되어 있는 경우를 우리는 **'페이지 부재(page fault)'**가 일어났다고 말한다.

<br/>

#### :1234: 요구 페이징의 페이지 부재 처리

---

1. CPU가 무효 페이지에 접근하면 주소 변환을 담당하는 하드웨어인 MMU가 페이지 부재 트랩(page fault trap)을 발생시킨다.
2. CPU의 제어권이 커널모드로 전환되고, 운영체제의 페이지 부재 처리 루틴이 호출되어 페이지 부재를 처리.
3. 사용되지 않는 주소 영역에 속한 페이지에 접근하려 했거나 해당 페이지에 대한 접근 권한 위반을 했을 경우에는 해당 프로세스를 종료
4. 접근이 적법한 것으로 판명된 경우, 물리적 메모리에서 비어 있는 프레임을 할당받아 그 공간에 해당 페이지를 읽어온다.

<br/>

#### 요구 페이징의 성능

---

요구 페이징의 성능은 다음과 같이 요청한 페이지를 참조하는 데 걸리는 유효 접근시간으로 측정.

> **유효 접근시간(effective access time)**
>
> = (1-P) X 메모리 접근시간 + P X (페이지 부재 발생 처리 오버헤드 + 메모리에 빈 프레임이 없는 경우 스왑 아웃 오버헤드 + 요청된 페이지의 스왑 인 오버헤드 + 프로세스의 재시작 오버헤드)

> **페이지 부재 발생비율(page fault rate)  0<= P <=1**
>
> P=0: 페이지 부재가 한 번도 일어나지 않은 경우
>
> P=1: 모든 참조 요청에서 페이지 부재가 발생한 경우

(1-P)는 페이지 부재가 일어나지 않는 비율. 이 경우에는 이미 페이지가 메모리에 적재되어 있는 것이므로 메모리에 접근 하는 시간만이 소요.

<br/>

### :bookmark_tabs: 페이지 교체

---

페이지 부재가 발생하면 요청된 페이지를 디스크에서 메모리로 읽어와야 한다.

이때 물리적 메모리에 빈 프레임이 존재하지 않을 수 있다. 이 경우에는 앞서 설명한 바와 같이 메모리에 올라와 있는 페이지 중 하나를 디스크로 쫓아내 메모리에 빈 공간을 확보하는 작업이 필요.

어떠한 프레임에 있는 페이지를 쫓아낼 것인지 결정하는 알고리즘을 **교체 알고리즘** 이라고 하는데 이 알고리즘의 목표는 페이지 부재율을 최소화하는 것이다.

**<u>가까운 미래에 참조될 가능성이 가장 적은 페이지를 선택해서 내쫓는 것이 성능을 향상시킬 수 있는 방안.</u>**

<br/>

+ [최적 페이지 교체](#최적-페이지-교체)
+ [선입선출 알고리즘](#선입선출-알고리즘)
+ [LRU 알고리즘](#LRU-알고리즘)
+ [LFU 알고리즘](#LFU-알고리즘)
+ [클럭 알고리즘](#클럭-알고리즘)

<br/>

---

### 최적 페이지 교체

물리적 메모리에 존재하는 페이지 중 가장 먼 미래에 참조될 페이지를 쫓아낸다.

<br/>

### 선입선출 알고리즘

페이지 교체 시 물리적 메모리에 가장 먼저 올라온 페이지를 우선적으로 내쫓는다.

> **FIFO의 이상 현상(FIFO anomaly)**
>
> 프레임이 3개인 경우 9번의 페이지 부재가 발생 하였지만, 프레임일 1개 더 증가한 4개일때는 오히려 10번의 페이지 부재가 발생.

<br/>

### LRU 알고리즘

페이지 교체 시 가장 오래전에 참조가 이루어진 페이지를 쫓아낸다. 달리 설명하면 마지막 참조 시점이 가장 오래된 페이지를 교체하게 되는 것이다.

<br/>

### LFU 알고리즘

페이지의 참조 횟수로 교체시킬 페이지를 결정한다. 즉 물리적 메모리 내에 존재하는 페이지 중에서 과거에 참조 횟수가 가장 적었던 페이지를 쫓아내고 그 자리에 새로 참조될 페이지를 적재.

최저 참조 횟수가 동일한 경우, 상대적으로 더 오래전에 참조된 페이지를 쫓아내도록 구현하는 것이 효율적.

<br/>

#### 참조 횟수 계산 방법

+ Incache-LFU
+ Perfect-LFU

<br/>

#### Incache-LFU

페이지가 물리적 메모리에 올라온 후부터의 참조 횟수를 카운트하는 방식. 따라서 페이지가 메모리에서 쫓겨났다가 다시 들어온 경우 참조 횟수는 1부터 새롭게 시작.

<br/>

#### Perfect-LFU

메모리에 올라와 있는지의 여부와 상관없이 그 페이지의 과거 총 참조 횟수를 카운트. 메모리에서 쫓겨난 페이지의 참조 기록까지 모두 보관하고 있어야 하므로 그 오버헤드가 상대적으로 더 크다고 할 수 있다.

<br/>

---

<br/>

### 클럭 알고리즘

---

LRU 알고리즘이 가장 오래전에 참조된 페이지를 교체하는 것에 비해 클럭 알고리즘은 오랫동안 참조되지 않은 페이지 중 하나를 교체한다.

> 최근에 참조되지 않은 페이지를 교체 대상으로 선정한다는 측면에서 LRU와 유사하지만 교체되는 페이지의 참조 시점이 가장 오래되었다는 것을 보장하지는 못한다는 점에서 LRU를 근사시킨 알고리즘으로 볼 수 있다.

<br/>

하드웨어적인 자원으로 동작하기 때문에 LRU에 비해 페이지의 관리가 훨씬 바르고 효율적으로 이루어진다. 따라서 대부분의 시스템에서 페이지 교체 알고리즘으로 클럭 알고리즘을 채택한다.

<br/>

### 클럭 알고리즘 수행 순서

1. 프레임들의 참조 비트를 순차적으로 조사.
2. 참조비트는 각 프레임마다 하나씩 존재하며 그 프레임 내의 페이지가 참조될 때 마다 하드웨어에 의해 1로 자동 세팅.
3. 클럭 알고리즘은 참조비트가 1인 곳은 0으로 바꾸고 그냥 지나가고 0인 페이지는 교체한다.
4. 교체된 페이지의 참조비트는 1로 세팅된다.

<br/>

---

<br/>

### 페이지 프레임의 할당

+ 균등할당
+ 비례할당
+ 우선순위 할당

<br/>

---

### 균등할당

모든 프로세스에게 페이지 프레임을 균일하게 할당.

<br/>

### 비례할당

프로세스의 크기를 고려하여 할당.

<br/>

### 우선순위 할당

우선순위에 따라 페이지 프레임을 다르게 할당. 당장 CPU에서 실행될 프로세스와 그렇지 않은 프로세스를 구분하여 전자 쪽에 더 많은 페이지 프레임을 할당하는 방식.

<br/>

---

<br/>

### 전역교체와 지역교체

교체할 페이지를 선정할 때, 교체 대상이 될 프레임의 범위를 어떻게 정할지에 따라 교체 방법을 **전역교체**와 **지역교체**로 구분.

<br/>

### 전역교체

+ 모든 페이지 프레임이 교체 대상이 될 수 있는 방법.

+ 전체 메모리를 각 프로세스가 공유해서 사용하고 교체 알고리즘에 근거해서 할당되는 메모리 양이 가변적으로 변하는 방법.

<br/>

### 지역교체

+ 현재 수행 중인 프로세스에게 할당된 프레임 내에서만 교체 대상을 선정할 수 있는 방법.

+ 지역교체 방법은 프로세스마다 페이지 프레임을 미리 할당하는 것을 전제.
+ 해당 프로세스에게 할당된 프레임 내에서만 페이지를 교체할 수 있다.

<br/>

---

<br/>

### 스레싱

---

집중적으로 참조되는 페이지들의 집합을 메모리에 한꺼번에 적재하지 못하면 페이지 부재율이 크게 상승해 CPU 이용률이 급격히 떨어지는 현상.

<br/>

### 스레싱 방지 방법

+ [워킹셋 알고리즘](#워킹셋-알고리즘)
+ [페이지 부재 빈도 알고리즘](#페이지-부재-빈도-알고리즘)

<br/>

### 워킹셋 알고리즘

---

### 지역성 집합(Locality Set)

집중적으로 참조되는 페이지들의 집합.

<br/>

워킹셋 알고리즘은 이러한 지역성 집합이 메모리에 동시에 올라갈 수 있도록 보장하는 메모리 관리 알고리즘.

워킹셋 알고리즘은 메모리에 올라와 있는 프로세스들의 워킹셋 크기의 합이 프레임의 수보다 클 경우 일부 프로세스를 스왑 아웃시켜서 남은 프로세스의 워킹셋이 메모리에 모두 올라가는 것을 보장.

</br>

#### 페이지 부재 빈도 알고리즘

---

어떤 프로세스의 페이지 부재율이 시스템에서 미리 정해놓은 상한값을 넘게 되면 이 프로세스에 할당된 프레임의 수가 부족하다고 판단하여 이 프로세스에게 프레임을 추가로 더 할당.

반면 프로세스의 페이지 부재율이 하한값 이하로 떨어지면 이 프로세스에게 필요 이상으로 많은 프레임이 할당된 것으로 간주해 할당된 프레임의 수를 줄인다.

---

<br/>

<br/>

:cd: 디스크 관리
===

외부에서는 디스크를 일정한 크기의 저장공간들로 이루어진 1차원 배열처럼 취급하게 된다. 이 일정한 크기의 저장공간을 **논리블록**이라고 한다.

각 논리블록에 저장되는 디스크 내의 물리적 위치를 섹터(sector)라고 부른다. 즉 논리블록 하나가 섹터 하나와 1대1로 매핑되어 저장되는 것이다.

<br/>

### 디스크 스케줄링

---

디스크에 대한 접근시간은 **탐색시간**, **회전지연시간**, **전송시간**으로 구분된다.

<br/>

* 탐색시간: 접근하려는 데이터가 원판의 안쪽에 있는지 바깥쪽에 있는지에 따라 헤드를 움직이는 데 걸리는 시간이 탐색시간.
* 회전지연시간: 디스크가 회전해서 읽고 쓰려는 섹터가 헤드 위치에 도달하기까지 걸리는 시간.
* 전송시간: 해당 섹터가 헤드 위치에 도달한 후 데이터를 실제로 섹터에 읽고 쓰는데 소요되는 시간.

<br/>

운영체제는 탐색시간을 줄이기 위해 헤드의 움직임을 최소화하는 스케줄링 작업을 한다. 디스크 스케줄링이란 효율적인 디스크 입출력을 위해 여러 섹터들에 대한 입출력 요청이 들어왔을 때 이들을 어떠한 순서로 처리할 것인지 결정하는 메커니즘을 말한다.

<br/>

### 디스크 스케줄링 방법

---

* [FCFS 스케줄링](#FCFS-스케줄링)
* [SSTF 스케줄링](#SSTF-스케줄링)
* [SCAN 알고리즘](#SCAN-알고리즘)
* [C-SCAN 알고리즘](#C-SCAN-알고리즘)
* [LOOK 알고리즘](#LOOK-알고리즘)
* [C-LOOK 알고리즘](#C-LOOK-알고리즘)

<br/>

### FCFS 스케줄링

디스크에 먼저 들어온 요청을 먼저 처리하는 방식.

FCFS 스케줄링의 최악의 경우, 입출력 요청이 디스크의 한쪽 끝과 반대쪽 끝에 번갈아 도착한다면 헤드는 디스크를 계속 왕복하며 일을 처리해야 하므로 탐색시간이 매우 비효율적으로 늘어나는 결과를 초래.

<p align="center"><img src="./img/FCFS.jpg" height="300px" width="300px"></p>

<br/>

### SCAN 알고리즘

헤드가 디스크 원판의 안쪽 끝과 바깥쪽 끝을 오가며, 그 경로에 존재하는 모든 요청을 처리.

SCAN 알고리즘은 한쪽 끝에서 다른 쪽 끝으로 한 번만 이동하면 현재 큐에 들어온 모든 요청을 처리할 수 있으므로 이동거리 측면에서 매우 효율적이다.

<p align="center"><img src="./img/SCAN.png" height="300px" width="300px"></p>

제일 안쪽이나 제일 바깥쪽 위치보다는 가운데 위치가 기다리는 평균시간이 더 짧다.

<br/>

### C-SCAN 알고리즘

헤드가 다른 쪽 끝에 도달해 방향을 바꾼 후에는 요청을 처리하지 않고 곧바로 출발점으로 다시 이동.

<p align="center"><img src="./img/C-SCAN.png" height="300px" width="300px"></p>

<br/>

### LOOK 알고리즘

LOOK 알고리즘은 헤드가 한쪽 방향으로 이동하다가 그 방향에 더 이상 대기 중인 요청이 없으면 헤드의 이동 방향을 즉시 반대로 바꾸는 스케줄링 방식.

<p align="center"><img src="./img/LOOK.png" height="300px" width="300px"></p>

<br/>

### C-LOOK 알고리즘

C-LOOK 알고리즘은 전방에 요청이 없을 때 방향을 바꾼다는 측면에서는 LOOK과 유사하며, 한쪽 방향으로 이동할 때에만 요청을 처리.

<p align="center"><img src="./img/C-LOOK.png" height="300px" width="300px"></p>

<br/>

### :card_index_dividers: 다중 디스크 환경에서의 스케줄링

---

동일한 정보를 여러 디스크에 중복 저장함으로써 인기 있는 데이터를 여러 디스크로부터 동시에 서비스 할 수 있고, 일부 디스크에 오류가 발생해도 지속적인 서비스가 가능하며, 정보의 유싱을 방지 할 수 있다. 이처럼 **다중 디스크를 사용하면 시스템의 성능과 신뢰성을 동시에 향상**가능.

<br/>

최근에는 전력 소모를 줄이는 것이 디스크 관리의 또 다른 중요한 목표로 인식.

디스크에 요청을 골고루 분산시키기보다는 일부 디스크에 요청을 집중시키고 나머지 디스크는 회전을 정지시키는 것이 더 효과적이다.

<br/>

### :electric_plug: 디스크의 저전력 관리

---

* 비활성화 기법
* 회전속도 조절 기법
* 디스크의 데이터 배치 기법
* 버퍼캐싱 및 사전인출 기법
* 쓰기전략을 통한 저전력 디스크  기법

<br/>

<br/>

:spider_web: 웹캐싱 기법
===

### 웹캐싱

---



웹캐싱이란 웹 사용자에 의해 빈번히 요청되는 데이터를 사용자와 지리적으로 가까운 웹캐시 서버에 보관해 빠른 서비스를 가능하게 하는 기법을 말한다.

**<u>프락시서버</u>**는 통상적으로 일개 그룹의 웹 사용자에 대한 서비스 지연시간을 줄이기 위해 사용되며, 궁극적으로는 네트워크의 대역폭 절약과 함께 웹서버의 부하를 줄이는 역할도 담당하게 된다.

**<u>역방향 프락시캐시</u>**는 일개 그룹에 속한 웹서버의 객체들을 캐싱하여 서버의 부하를 직접적으로 줄이는 역할.

<p align="center"><img src="./img/proxy.jpg" height="300px" width="300px"></p>

<br/>



### 웹캐시의 교체 알고리즘

---

페이징 기법에서는 동일한 크기의 페이지를 캐싱의 대상으로 하며, 캐시부재시 페이지를 디스크에서 메모리로 읽어오기 때문에 그 비용인 균일하다.

반면, 웹캐싱에서는 객체를 가지고 있는 근원지의 서버의 위치 및 특성에 따라 객체를 캐시로 읽러오는 비용이 다르며, 하나의 URL에 대응하는 파일 단위로 캐싱이 이루어지기 때문에 캐싱 단위의 크기가 균일하지 않다.

<br/>

#### 교체 알고리즘의 성능 척도

웹캐싱에서는 객체들의 크기와 인출 비용이 균일하지 않기 때문에 객체들의 **참조가능성**과 **이질성**을 함께 고려해서 객체들의 가치를 평가하는 것이 바람직하다.

+ 캐시적중률
+ 바이트적중률
+ 지연감소율
+ 비용절감률

<br/>

#### 참조 가능성의 예측

캐시 교체 알고리즘은 미래의 참조를 미리 알지 못하는 상태에서 미래에 참조될 가능성이 높은 객체를 선별해 한정된 캐시 공간에 보관하는 것이므로 그 효율성은 궁극적으로 미래의 참조 가능성을 얼마나 잘 예측하는가에 좌우된다.

<br/>

#### LRFU(Lest Recently Frequently Used)

과거 시점에서의 각각의 참조가 현재 객체의 참조 가능성을 예측하는데 기여하게 되며, 최근의 참조일수록 기여도를 더 높게 계산한다.

> **객체의 참조 가능성 P(i)**
>
> **참조된 시점 t1, t2, t3**
>
> **현재 시간 tc**
>
> i에 대한 모든 과거 참조 시점과 참조 횟수를 다 이용한다. 과거 시점에서의 각각의 참조가 객체의 가치를 높이는 데 기여하는 바를 각 시점의 최근성에 근거해 계산하고 이를 더하는 것이다.
>
> **P(i) = (tc-t1) + (tc-t2) * (tc-t3)**

<br/>

#### 객체의 이질성에 대한 고려

객체를 캐시에 보관하고 있을지 여부를 결정하는 가치평가 기준은, 객체의 참조 가능성에 의한 가치와 캐시에서 적중될 경우 실제로 절약할 수 있는 비용을 동시에 고려해야 한다.

<br/>

#### 알고리즘의 시간 복잡도

캐시 교체 알고리즘이 실제 시스템에 유효하게 사용되기 위해서는 시간복잡도 측면에서 현실성이 있어야 한다.

일반적으로 캐시 내에 있는 객체의 수 n개에 대해 시간 복잡도 측면에서는 캐시 운영에 드는 시간이 O(logn)를 넘지 않는 것이 바람직하다.

<br/>

### 웹캐시의 일관성 유지 기법

---

캐싱된 웹 객체가 근원지 서버에서 변경될 수 있으므로 웹캐시에서는 사용자에게 유효한 정보를 전달하기 위한 일관성 유지 기법이  필요하다.

<br/>

#### 적응형 TTL

약한 일관성 유지 기법을 사용.

> 약한 일관성 유지 기법이란 사용자의 요청이 있을 때마다 캐싱된 객체가 변경되었는지 근원지 서버에 일일이 확인하는 것이 아니라, 변경되었을 가능성이 높은 경우에만 확인하는 기법.

<br/>

강한 일관성 유지 기법을 상용할 경우 웹서버와 네트워크의 부담이 커서 득보다는 실이 많다.

+ [polling-every-time(강한 일관성)](#polling-every-time)
+ [invalidation(강한 일관성)](#invalidation)
+ [adaptive TTL(약한 일관성)](#adaptive-TTL)

<br/>

### polling-every-time

---

캐시 내에 이미 존재하는 객체에 대한 요청이 있을 때마다 근원지 서버에 객체의 변경 여부를 확인하는 방법.

<br/>

### invalidation

---

근원지 서버가 자신의 객체를 캐싱하고 있는 모든 프락시서버를 기록해두었다가 해당 객체가 변경된 경우 해당 프락시서버들에 변경 사실을 알려주는 방법.

<br/>

### adaptive TTL

---

캐시 내에 이미 존재하는 객체에 대한 요청이 있을 때, 해당 객체에 대한 최종 변경 시각과 최종 확인 시각을 고려해서 변경되었을 가능성이 높다고 판단되는 경우에만 근원지 서버에 변경 여부를 확인하는 방법. 변경 가능성은 **LMF(Last Modified Factor)**에 의해 판단하며 <u>LMF가 임계값 이상인 경우메나 변경가능성이 높다고 보아 근원지 서버에 변경 여부를 확인</u>한다.

> LMF = (C-V)/(V-M)
>
> M: 해당 객체의 최종 변경 시각
>
> V: 최종 확인 시각
>
> C: 현재 시각



<br/>

### 웹캐시의 공유 및 협력 기법

---

웹캐시 간의 공유는 일반적으로 인터넷 캐시 프로토콜에 의해 이루어진다(ICP). 사용자가 프록시서버에 웹 객체를 요구했는데 프록시서버가 그 객체를 캐싱하고 있지 않는 경우 ICP에서는 동료 프록시들에게 ICP질의를 멀티캐스트해서 누가 웹 객체를 가지고 있는지 확인한다. 그 후 그 동료 프록시에게 HTTP요청을 보내어 받아와 사용자에게 전달한다. 그리고 캐시 배열 간 경로지정 프로토콜(CARP)이 있다.

<br/>

### 웹캐시의 사전인출 기법

---

웹서비스의 응답 지연시간을 줄이기 위한 방법의 일환으로 사용자에 의해 아직 요청되지 않은 객체를 미리 받아오는 사전인출 기법의 중요성이 증가하고 있다. **예측 사전인출 기법**과 **대화식 사전인출 기법**으로 나누어볼 수 있다. 전자는 하나의 웹페이지가 참조되었을 때 새로운 웹페이지가 참조될 확률을 과거 기록을 통해 예측하는 것이고, 후자는 사용자가 HTML문서에 대한 요청을 했을 때 웹캐시는 캐싱하고 있던 HTML 문서를 미리 파싱해 그 문서에 포함되거나 연결된 웹 객체를 미리 받아와서 후속 요청에 곧바로 전달하는 기법이다.

<br/>

### 동적 웹 객체의 캐싱 기법

---

지금까지의 웹캐싱 기법은 실시간으로 변하지 않는 데이터 HTML,JPG,GIF 등의 정적 웹 컨텐츠에 대한 캐싱이었다. 그러나 실시간성을 요구하는 컨텐츠인 ASP, CGI등 동적인 웹 컨텐츠를 처리하는 부분이 전체 웹서비스 지연시간 중 상당한 부분을 차지하며 웹 서버의 과부하를 일으키는 주요 요인으로 분석된다. 동적 웹컨텐츠에 대한 웹캐싱은 정적 웹컨텐츠의 캐싱에 비해 데이터 관리에 어려움이 따른다. 그 이유는 요청받은 내용에 대해 프로그램을 실행한 후 그 결과물을 보내주어야 하기 때문이다. 정적 컨텐츠와는 다르게 말이다. 현재까지 동적 웹컨텐츠 캐싱은 주로 웹서버 내부에서 빠른 처리를 위해 웹서버 전위에 설치하는 역방향 프록시 캐시 또는 웹서버 가속기 중 일부에서 활용하고 있는 실정이다.

