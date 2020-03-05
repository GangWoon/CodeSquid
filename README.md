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



<br>

![image](https://user-images.githubusercontent.com/48466830/75874959-2342cc00-5e56-11ea-9045-26f550225e9f.png)

글에 들어가기 앞서 mvc 이론적인 부분이 아닌, 실제로 프로젝트에 적용할 때 필요한 정보에 대해서 정리했다.



## MVC 

## Model

---

### Model -> ViewController 

model에서 viewcontroller로 정보를 보내는 건 model이 viewcontroller를 알고 있는 것처럼 느껴지지만, 사실 아래에 있는 두 가지 선택 중 어느 걸 선택하더라도 viewController가 model을 감시하는 구조이다. __model은 viewController을 몰라야 한다.__

- KVO
  observer에게 감시받는 속성을 갱신한다.

  ``` swift 
  extension Model: ViewDataSoure, Datable {
      func someMethod(newData: Data) {
          self.information = newData
      }
    	...
  }
  ```

- Notification
  notification center로 정보를 전송한다.

  ``` swift
  NotificationCenter.default.post(name: NSNotification.Name("model"),
                                  object: nil,
                                  userInfo: ["information": game])
  ```

> 모델 객체는 뷰 객체나 뷰 프레임워크에 의존하지 않고 독립적으로 만들어야 한다.			- JK

<br>

<br>

## View

---

### View -> ViewController

view에서 event가 발생한 걸 viewController로 보내기 위해 delegate 패턴을 사용한다.

``` swift
weak var datasource: ViewDataSource?
var data: Datable?

@objc func didTouchedButton() {
  	datasource.someMethod(newData: someData)
}
```

<br>

view객체에서 신경 써야 할 부분은 model객체를 모르는 것이다.  __datasource라는 속성을 구현해주는 대상을 몰라야 한다.__ 

하지만 정보를 받아올 때 "model을 사용할 수밖에 없는 게 아닌가." 의문점이 드는데 이 부분을 프로토콜의 다형성을 사용한다.

model이 특정 프로토콜을 채택하게 만들어서 view에서는 그 특정 프로토콜을 받도록 만든다.

> 다른 객체를 참조하는 경우에는 구체 타입에 의존하지 않고, 프로토콜에 의존하도록 만든다.			-JK

<br>

<br>

## ViewController 

---

mvc 패턴에서 가장 중요한 역할을 하는 viewController,  __view와 model의 존재를 알고 있는 존재__로서, event가 발생하면 model과 view를 묶어주는 역할을 담당한다.

### ViewController -> Model

view에서 받은 자료를 model로 전달한다.

``` swift
override func viewDidLoad() {
  ...
		view.datasource = model  
}

```

<br>

### ViewController -> View

model에서 받은 정보를 view에게 넘겨준다.

- KVO

  ``` swift
  observer = dataManager.observe(\.model) { _, model in
      view.data = model.newValue
  }
  ```

- Notification 

  ``` swift
  NotificationCenter.default.addObserver(self,
                                          selector: #selector(pushInformationToView),
                                          name: NSNotification.Name( "model"),
                                          object: nil)
  
  @objc func pushInformationToView(_ notificaiton: Notification) {
      .... notifiaction.userInfo 사용
   }
  ```

<br>

<br>

mvc 패턴의 작동을 시간 순으로 나태는 그림이다.

![Paper 다이어리 90](https://user-images.githubusercontent.com/48466830/76004112-56678700-5f4c-11ea-8bd1-9e47875df435.png)

<br>

<br>

# Reference

[스위프트 성능 이해하기](https://youtu.be/z1Gf6EosaUQ)

