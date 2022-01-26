# Main Memory

## Background

- 프로세스는 메인 메모리에 로드되어 실행중인 프로그램이다.
- 메모리는 바이트 단위의 배열로 구성되어 있다.
- CPU는 프로그램 카운터를 사용해 메모리에서 명령어를 fetch 및 실행한다.

### Memory Space

![](https://www.cs.uic.edu/~jbell/CourseNotes/OperatingSystems/images/Chapter8/8_01_LogicalAddressSpace.jpg)

각 프로세스가 메모리 주소 공간을 따로따로 관리하는 것이 멀티프로세싱, 멀티프로그래밍의 기본적인 조건 중 하나이다.

이를 위해 프로세스에게 분리된 메모리 공간을 확보해주어야 하며 베이스 레지스터와 리미트 레지스터를 통해 논리 주소 공간을 정의한다.

![](https://www.cs.uic.edu/~jbell/CourseNotes/OperatingSystems/images/Chapter8/8_02_HardwareAddressProtection.jpg)

또한 메모리 공간을 보호하기 위해 사용자 모드에서 생성된 모든 주소를 아래 그림과 같이 베이스, 리미트와 비교한다.

특정 메모리 주소에 접근할 때 베이스와 리미트 사이에 존재하는 ligal한 주소 일때만 접근이 가능하며, 잘못된 접근인 경우 세그멘테이션 오류가 발생해 프로그램이 중단된다.

### Address Binding

프로그램은 실행 전 디스크에 이진 실행 파일로 저장되어 있다. 프로그램을 실행하려면 프로그램을 메모리로 가져와야 하며 가져온 후 실행되어야 그 프로그램은 프로세스가 된다.

다음은 사용자 프로그램의 프로세싱 과정이다.

소스코드 단계에서 symbolic한 코드들이 컴파일러르 통해 재배치 가능한 주소로 바뀌고 링커와 로더를 통해 논리 / 절대 주소에 바인딩된다.

![](https://www.cs.uic.edu/~jbell/CourseNotes/OperatingSystems/images/Chapter8/8_03_MultistepProcessing.jpg)

- 컴파일러는 i, count, a, num 등 사용자가 symbolic하게 작성한 소스코드 내 주소를 재배치 가능한 주소(realocatable address)에 바인딩한다.
- 링커를 통해 논리 주소가 만들어진다.
- 로더를 실행할 때 절대 주소에 바인딩된다.

### Logical vs Physical Address Space

- 논리 주소: CPU에 의해 생성되는 주소다.
- 물리 주소: 실제 하드웨어 상, 메모리 장치에 표시되는 주소다. 쉽게 말해 메모리 주소 레지스터에 로드된 주소다.
- 논리 주소 공간: 사용자 프로그램에 의해 생성된 모든 논리 주소의 집합이다.
- 물리 주소 공간: 모든 물리적 주소의 집합이다.

### MMU(memory management unit)

MMU란 논리 주소에서 물리 주소로 매핑하는 하드웨어 장치다. MMU에는 재배치 레지스터(relocation register)가 베이스 레지스터 역할을 한다.

![](https://www.cs.uic.edu/~jbell/CourseNotes/OperatingSystems/images/Chapter8/8_04_DynamicRelocation.jpg)

재배치 레지스터를 통해 논리 주소를 동적으로 물리 주소로 매핑할 수 있다.

### Dynamic Loading

동적 로딩이란 메모리 주소 공간을 효율적으로 사용하기 위해 모든 루틴을 한꺼번에 로딩하는 것이 아니라 필요할 때만 로딩하는 것이다.

재배치 가능한 링킹 로더가 필요한 루틴을 호출할 때 프로그램의 주소 테이블을 업데이트한다.

### Dynamic Linking and Shared Libraries

- DLLs(dynamically linked libraries): 프로그램 실행 중간에 사용자 프로그램에 링크되어 사용할 수 있는 시스템 라이브러리다. 라이브러리가 로드된 상태에서 필요할 때 링크된다.
- 동적 링킹과 정적 링킹: [link](https://jhnyang.tistory.com/42)
- 공유 라이브러리: 메인 메모리에 하나의 DLL 인스턴스가 존재해 다수 사용자 프로세스가 공유할 수 있기 때문에 DLL을 공유 라이브러리라고 부르기도 한다.

<br>

## Contiguous Memory Allocation

메모리는 보통 운영체제를 위한 부분과 사용자 프로세스를 위한 부분, 두 부분으로 나뉜다. 따라서 우리는 메인 메모리를 효율적인 방법으로 할당해야 한다.

연속 메모리 할당(contiguous memory allocation)은 프로세스를 메모리의 단일 섹션에 할당해 다음 프로세스와 인접하게 즉, 연속적으로 할당하는 방식이다.

### Memory Protection

![](https://www.cs.uic.edu/~jbell/CourseNotes/OperatingSystems/images/Chapter8/8_06_HardwareSupport.jpg)

재배치 레지스터와 리미트 레지스터를 통해 프로세스가 소유하지 않은 메모리에 접근하는 것을 방지한다.

### Memory Allocation

연속 메모리 할당을 위한 방법은 다음과 같다.

- variable-partition: 메모리를 동일한 크기의 파티션으로 나누고 각 프로세스를 파티션에 할당한다.
- hole: 사용 가능한 메모리 블록 부분을 홀이라고 하며 프로세스가 메모리에 로드될 때 이 홀의 크기에 적합하면 홀에 할당한다.

사용 가능한 메모리 공간인 홀에 효율적으로 메모리 할당을 하기위한 전략에는 3가지가 있다.

- first fit: 홀을 탐색해 가장 첫 번째 홀에 할당하는 방법이다.
- best fit: 할당할 수 있는 가장 작은 크기의 홀에 할당한다.
- worst fit: 가장 큰 홀에 할당한다.

first fit과 best fit은 스토리지 활용도 측면에서 거의 동일하지만 first fit이 조금 더 빠르다.

### Fragmentation

메모리 할당에는 항상 단편화 문제가 발생한다. 단편화란 메모리 공간의 크기가 적절하지 않아 할당할 수 없고 낭비되는 상태를 말한다.

- 외부 단편화(external fragmentation): 프로세스의 크기보다 더 많은 공간이 메모리에 존재함에도 불구하고 그 공간이 나누어져 있어 할당할 수 없는 상태다.
- 내부 단편화(internal fragmentation): 프로세스가 본인의 크기보다 큰 메모리 공간에 할당되어 잉여 메모리가 낭비되는 상태다.

<br>

## Paging

페이징이란 프로세스의 물리 주소 공간을 불연속적(non-contiguous)으로 관리하는 것이다.

- 페이징을 통해 외부 단편화와 조각화된 메모리 공간 문제를 해결할 수 있다.
- 페이징의 기본 개념은 물리적 메모리를 같은 크기의 프레임(frame) 블록으로 나누고 논리적 메모리 공간을 같은 크기의 페이지(page) 블록으로 나누어 논리 주소 공간을 물리 주소 공간과 완전히 분리하는 것이다.
- 물리적 메모리의 프레임과 매핑해 페이지를 가리키는 논리 주소에서 프레임을 가리키는 물리 주소로 변환한다.
- 쉽게 말해 페이징이란 프로세스를 일정 크기인 페이지로 분할해 메모리에 적재하는 방식이다.
- 주소를 전달할 때 페이지 번호(p)와 페이지 오프셋(d)를 통해 p페이지의 d번째 메모리에 할당할 수 있다.
- 페이지의 크기는 하드웨어가 결정한다.

![](https://www.cs.uic.edu/~jbell/CourseNotes/OperatingSystems/images/Chapter8/8_10_PagingHardware.jpg)

CPU는 논리 주소의 페이지 번호를 페이지 테이블을 통해 프레임 번호(f)로 변환한다. 이 결과 값을 통해 물리 주소로 매핑된다.

### Page Table

페이지 테이블은 메인 메모리에 존재하며 이전에는 하드웨어가 관리했지만 최근에는 PTBR(page table base register)에 의해 관리된다.

![](https://www.cs.uic.edu/~jbell/CourseNotes/OperatingSystems/images/Chapter8/8_12_PagingExample.jpg)

- 페이지 테이블 위 그림과 같이 인덱스로 페이지 번호를 가리키고 담고 있는 숫자로 매핑할 프레임 번호를 가리킨다.
- 그림 속 페이지 테이블은 페이지 테이블의 역할을 이해하기 쉽게 표현한 것이며 실제로 페이지 테이블은 페이지 번호와 프레임 번호 이외에도 다양한 필드들이 존재한다.

### TLB

[link](https://jhnyang.tistory.com/471)

<br>

## Swapping

프로세스는 메모리에 로드되어 실행 중인 프로그램이다. 그렇다면 실제 물리 주소 공간보다 큰 프로세스는 어떻게 실행이 가능한 것일까?

이것을 가능하게 하는 것이 바로 스와핑이다. 스와핑은 명령어와 데이터를 포함한 프로세스 전체를 저장공간으로 swap out 해놓고 필요한 경우에만 메모리로 swap in 해서 사용한다.

이렇게 프로세스 전체를 스와핑하는 것은 큰 비용이 요구되므로 프로세스를 페이지 단위로 스와핑하는 것 또한 가능하다. 페이지 단위로 스와핑하면 물리적 메모리와 논리적 메모리를 분리할 수 있으며 아주 작은 단위의 페이지도 스왑할 수 있다.

---

### Reference

- [운영체제 공룡책 강의](https://www.inflearn.com/course/%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C-%EA%B3%B5%EB%A3%A1%EC%B1%85-%EC%A0%84%EA%B3%B5%EA%B0%95%EC%9D%98/lecture/65286?volume=1.00)
