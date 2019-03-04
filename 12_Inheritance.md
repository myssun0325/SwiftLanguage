## Inheritance
클래스는 메서드와 프로퍼티와 다른 클래스로부터 속성을 상속받을 수 있다.
상속받은 클래스를 `subclass`라고 한다. 상속해준 클래스를 `superclass`.
상속이 스위프트에서 다른 타입과 다른 클래스를 구분하는 기본적인 특징이다.
재정의 가능.
스위프트에서는 재정의는 확실하게 확인해주어야 한다.
상속받은 프로퍼티에 프로퍼티 감시자도 추가할 수 있다.(모든 프로퍼티에 가능)

#### Defining a Base Class
`base class`: 어떤 클래스도 상속받지 않은 클래스, 기반 클래스

#### Subclassing
Subclassing은 이미 존재하는 클래스에서 새로운 클래스의 수행을 말한다.
subclass에 재정의나 새로운 특성을 추가할 수 있다.

```swift
class Vehicle {
    var currentSpeed = 0.0
    var description: String {
        return "traveling at \(currentSpeed) miles per hour"
    }
    func makeNoise() {
        // do nothing - an arbitrary vehicle doesn't necessarily make a noise
    }
}

let someVehicle = Vehicle()
print("Vehicle: \(someVehicle.description)")

class Bicycle: Vehicle {
    var hasBasket = false
}

let bicycle = Bicycle()
bicycle.hasBasket = true

bicycle.currentSpeed = 15.0
print("Bycle: \(bicycle.description)")

class Tandem: Bicycle {
    var currentNumberOfPassengers = 0
}

let tandem = Tandem()
tandem.hasBasket = true
tandem.currentNumberOfPassengers = 2
tandem.currentSpeed = 22.0
print("Tandem: \(tandem.description)")

```

#### Overriding
subclass는 인스턴스메서드, 타입메서드, 인스턴스 프로퍼티, 타입 프로퍼티, 서브스크립트를 재정의 할 수 있다.
`override`라는 키워드를 사용, 스위프트 컴파일러는 override 키워드를 통해 superclass에서 일치하는 선언을 했는지 체크한다.

* Accessing Superclass Methods, Properties, and Subscripts
    - 메서드, 프로퍼티, 서브스크립트를 재정의할 때, 슈퍼클래스에 정의되어 있는 부분을 재정의로 사용하는 것은 유용하게 쓰일 수 있다. 예를 들어 이미 구현되어 있는 것을 수정하거나, 상속받은 변수에 수정된 값을 저장할 수 있다.
    - superclass에 접근하고 싶을 때, `super` 키워드를 사용할 것.
* Overriding Methods
상속받은 타입메서드나 인스턴스 메서드를 재정의할 수 있다.
* Overriding Properties
상속받은 인스턴스 프로퍼티나 타입 프로퍼티를 재정의할 수 있다(to provide own custom getter and setter for that property, or to add property observer). -> 프로퍼티 감시자와 연산프로퍼티에 관한 것
* Overriding Property Getters and Setters
상속된 프로퍼티가 저장 프로퍼티인지 연산프로퍼티인지 상관없이 상속받은 프로퍼티를 재정의하기 위해 custom getter, setter를 제공할 수 있다. 서브클래스는 상속받은 프로퍼티가 저장 프로퍼티인지 연산프로퍼티인지 알지 못한다.(타입과 이름만 안다.)
    - 재정의를 통해 상속받은 읽기전용 프로퍼티를 읽기쓰기 프로퍼티로 바꿀 수 있다.
    - 재정의를 통해 읽기쓰기 프로퍼티를 읽기전용 프로퍼티로 바꾸진 못한다.
    - Note: 만약에 프로퍼티 재정의의 일부로 setter를 제공했다면, 반드시 getter를 제공해야한다. 만약에 상속받은 프로퍼티의 값을 재정의한 getter에서 변경하고 싶지 않다면, getter에서 `super.someProperty`를 반환 받음으로써 반환된 값을 넘겨 줄 수있다(상속된 값).
* Overriding Property Observers
상속받은 프로퍼티에 프로퍼티 감시자를 사용할 수 있다. 처음 구현(어떻게 구현되었건)과 상관없이 상속된 값의 변경이 있을 때마다 변화를 알아차릴 수 있다.
    - Note: `상속받은 상수 저장 프로퍼티`나 `상속받은 읽기전용 연산프로퍼티`에는 프로퍼티감시자를 추가할 수 없다. 이런 프로퍼티의 값들은 설정할 수 없고, willSet과 didSet을 제공하는 것이 적절치 못하다. 또한 위에 말한 프로퍼티에는 재정의 setter와 재정의 프로퍼티 감시자 또한 제공하지 못한다. 만약에 프로퍼티의 값의 변화를 모니터링하고 싶다면, custom setter를 통해 할 수 있다.  

#### Preventing Overrides
재정의 방지 : `final` keyword(such as `final var`, `final func`, `final class`, `final subscript`)
재정의 방지된 것에 재정의를 시도하면 컴파일 에러 발생.
클래스 확장(extension)에서 추가한 메서드, 프로퍼티, 서브스크립트에도 extension's definition(확장 정의에서) final을 사용할 수 있다.
