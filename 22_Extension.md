## Extension

* 스위프트 익스텐션 타입에 추가할 수 있는 기능
	- 연산 타입 프로퍼티 / 연산 인스턴스 프로퍼티
	- 타입 메서드 / 인스턴스 메서드
	- 이니셜라이저
	- 서브스크립트
	- 중첩 타입
	- **특정 프로토콜을 준수할 수 있도록 기능 추가**

* 익스텐션은 타입에 새로운 기능을 추가할 수 있지만, 기존에 존재하는 기능을 재정의할 수는 없다.

#### 언제?
* 외부에서 가져온 타입에 원하는 기능을 추가할 때
* 상속이 안되는 열거형과 구조체에 기능을 추가할 때

#### 이니셜라이저
* 익스텐션으로 __클래스 타입__ 에 편의 이니셜라이저는 추가할 수 있지만, 지정 이니셜라이저는 추가할 수 없다. 지정 이니셜라이저는 반드시 구현부에 위치해야 한다.
* 익스텐션으로 값 타입에 이니셜라이저를 추가했을 때, 해당 값 타입이 다음 조건을 모두 성립하면 익스텐션으로 사용자정의 이니셜라이저를 추가한 이후에도 해당 타입의 __기본 이니셜라이저와 멤버와이즈 이니셜라이저를 호출__ 할 수 있다.
	- __모든 저장프로퍼티에 기본값이 있을 경우__
	- __타입에 기본 이니셜라이저와 멤버와이즈 이니셜라이저 외에 추가 사용자정의 이니셜라이저가 없을 경우__

```swift
struct Size {
    var width: Double = 0.0
    var height: Double = 0.0
}

struct Point {
    var x: Double = 0.0
    var y: Double = 0.0
}

struct Rect {
    var origin: Point = Point()
    var size: Size = Size()
}

let defaultRect: Rect = Rect()
let memberwiseRect: Rect = Rect(origin: Point(x: 2.0, y: 2.0), size: Size(width: 5.0, height: 5.0))

extension Rect {
    init(center: Point, size: Size) {
        let originX: Double = center.x - (size.width / 2)
        let originY: Double = center.y - (size.height / 2)
        self.init(origin: Point(x: originX, y: originY), size: size)
    }
}

let centerRect: Rect = Rect(center: Point(x: 4.0, y: 4.0), size: Size(width: 3.0, height: 3.0))

```
