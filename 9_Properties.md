#### Stored Porperties
* Stored Properties of Constant Structure Instances
    - 값 타입이 아닌 참조타입인 클래스에서는 상수로 선언된 프로퍼티도 변경가능
* Lazy Stored Properties
    - 처음 사용할 때까지 초기 값이 계산되지 않음
    - lazy property는 variable만됨(`var`), 상수프로퍼티는 초기화가 이루어지기 전에 값을 가져야 하기 때문에
* Stored Properties and Instance Variables

#### Computed Properties
* getter와 옵셔널setter

* Shorthand Setter Declaration : setter의 new value를 정의안할때 default - `newValue`

* Read-Only Computed Properties : getter만 있고, setter는 X
* 연산 프로피터도 `var`로만 선언

#### Property Observers (willSet, didSet)
* 정의된 모든 stored properties 에 추가할 수 있다.lazy stored property는 예외
* 상속받은 프로퍼티에도(저장, 연산 프로퍼티) 가능(오버라이딩으로)
* 오버라이딩 안한 연산 프로퍼티에는 옵저버가 필요 없다 -> setter에서 값이 변경되면 반응할 수 있어서
* superclass프로퍼티의 willset이나 didset은 superclass의 initializer가 호출된 후에 subclass initializer에서 프로퍼티를 set할 때 호출된다.
```swift
class StepCounter {
    var totalSteps: Int = 0 {
        willSet(newTotalSteps) {
            print("About to set totalSteps to \(newTotalSteps)")
        }
        didSet {
            if totalSteps > oldValue {
                print("Added \(totalSteps - oldValue) steps")
            }
        }
    }
}

let stepCounter = StepCounter()
stepCounter.totalSteps = 200
stepCounter.totalSteps = 360
stepCounter.totalSteps = 896

```
* 상속관련: superclass의 프로퍼티감시자는 superclass의 이니셜라이저가 호출된 후에, 프로퍼티가 set 될 때 subclass의 이니셜라이저에서 호출된다.

* If you pass a property that has observers to a function as an in-out parameter, the willSet and didSet observers are always called.

#### Global and Local Variables
* 전역 변수와 전역 상수는 lazy저장 프로퍼티와 유사한 방식으로 나중에 계산된다. lazy 저장 프로피터와 다른점은 전역 상수나 변수는 lazy를 표시할 필요가 없다. 지역 변수, 상수는 lazy 해당 없음

#### Type Properties  
* static
* 타입자체에 속하게 되는 프로퍼티 -> 타입자체에 속한다? = A class 의 타입프로퍼티 a가 있다면
A.a로 사용할 수 있다. (공용사용에 유용)
