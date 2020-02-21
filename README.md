# CodeSquid

![87029311_3358068287542541_5377126684669509632_o](https://user-images.githubusercontent.com/48466830/74895228-776c9b80-53d4-11ea-8268-fbc1fad8970e.jpg)

<br>

### 클래스, 객체, 인스턴스
* 클래스 - 인스턴스를 만들기 위한 틀, 속성과 행동을 갖는다.
* 인스턴스 - 클래스를 토대로 만들어진 객체, 속성을 갖는다. 행동은 클래스에 있다.
* 객체 - 실체, 관련된 것들의 모음.

<br>

## 성능을 비교할 수 있는 3가지 기준

Memory Allocation: <font color="green">Stack</font> or <font color="red">Heap</font>

Reference Counting: <font color="green">Yes</font> or <font color="red">No</font>

Method Dispatch: <font color="green">Static</font> or <font color="red">Dynamic</font>

<br>

### struct는 정말 reference counting을 사용하지 않는가?

일정 크기 이상으로 증가하면 스토리지는 값을 힙에 할당하고 그 힙을 가르킨다.

해당 스토리지는 스택에 존재한다.

struct 내부에 class가 존재할 경우 rc를 사용한다.

<br><br>

# Reference

[스위프트 성능 이해하기](https://youtu.be/z1Gf6EosaUQ)

