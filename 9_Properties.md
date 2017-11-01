#### Stored Porperties
* Stored Properties of Constant Structure Instances
    - 값 타입이 아닌 참조타입인 클래스에서는 상수로 선언된 프로퍼티도 변경가능
* Lazy Stored Properties
    - 처음 사용되기 전까지 initial value(초기값)가 계산되지 않는 프로퍼티, 변수 앞에 `lazy`를 사용
    - Note: lazy property는 인스턴스 초기화가 지나서도 초기값을 가져올 수 없기 때문에 항상 *변수* 에만 사용해야 한다. 상수 프로퍼티는 초기화가 완료되기전에 항상 값을 가지고 있어야 하기 때문에 lazy를 사용할 수 없다.
    - Lazy property는 프로퍼티의 초기값이 인스턴스의 초기화가 끝날 때까지 값을 알 수 없는 외부요소에 의존할 때 유용하다.
    - Lazy property는 프로퍼티의 초기값이 복잡하거나 computationally expensive setup을 요구할 때 유용하다.
    ```swift
    class DataImporter {
        /*
         DataImporter is a class to import data from an external file.
         The class is assumed to take a nontrivial amount of time to initialize.
         */
        var filename = "data.txt"
        // the DataImporter class would provide data importing functionality here
    }

    class DataManager {
        lazy var importer = DataImporter()
        var data = [String]()
        // the DataManager class would provide data management functionality here
    }

    let manager = DataManager()
    manager.data.append("Some data")
    manager.data.append("Some more data")
    // the DataImporter instance for the importer property has not yet been created

    print(manager.importer.filename)
    // the DataImporter instance for the importer property has now been created
    // Prints "data.txt"
    ```
* Stored Properties and Instance Variables
  - 공식문서 이해안감 -> 책 활용
#### Computed Properties
* getter와 옵셔널setter

* Shorthand Setter Declaration : setter의 new value를 정의안할때 default - `newValue`

* Read-Only Computed Properties : getter만 있고, setter는 X
* 연산 프로피터도 `var`로만 선언

#### Property Observers (willSet, didSet)
* 정의된 모든 stored properties 에 추가할 수 있다.lazy stored property는 예외
* 상속받은 프로퍼티에도(저장, 연산 프로퍼티) 가능(오버라이딩으로)
* 상속되지 않은 연산 프로퍼티에는 옵저버가 필요 없고 사용 할 수도 없다. -> getter와 setter로 구현가능
* superclass의 프로퍼티감시자 willset과 didset은 superclass의 initializer가 호출된 후에 subclass initializer안에서 프로퍼티를 set할 때 호출된다. => 부모 클래스의 프로퍼티 감시자를 부른다는 의미
* 예제코드
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
    // About to set totalSteps to 200
    // Added 200 steps
    stepCounter.totalSteps = 360
    // About to set totalSteps to 360
    // Added 160 steps
    stepCounter.totalSteps = 896
    // About to set totalSteps to 896
    // Added 536 steps
    ```

* Note: If you pass a property that has observers to a function as an in-out parameter, the willSet and didSet observers are always called. This is because of the copy-in copy-out memory model for in-out parameters: The value is always written back to the property at the end of the function.
* 초기화 이후부터 프로퍼티를 감시, 클래스의 init안에서 값을 할당핳 때는 didSet, willSet이 호출되지 않음

#### Global and Local Variables
연산프로퍼티와 프로퍼티 감시자는 전역변수와 지역변수로 사용가능하다. 전역변수는 함수, 메서드, 클로저, type context 외부에 정의되어 있는 변수다. 지역변수는 함수, 메서드, 클로저 문맥 안에 정의되어 있는 변수다. 전역이나 지역변수로 연산변수와(computed variables) 저장변수를 위한 옵저버(observers for stored variables)를 정의할 수 있다.
* Note: 전역상수와 전역변수는 lazy stored property와 비슷한 방법으로 항상 지연(lazily) 연산된다. lazy stored property와 다르게 전역변수와 전역상수는 lazy를 표시하지 않아도된다. 이에 반해 지역변수와 지역상수는 절때 지연(lazily)연산되지 않는다.

#### Type Properties  
인스턴스 프로퍼티는 특정 타입의 인스턴스에 속하는 프로퍼티다. 새로운 인스턴스를 만들 때마다 인스턴스는 프로퍼티값을 가지고 있고 이것은 다른 인스턴스와 분리되어 있다. (인스턴스 당 프로퍼티를 가지고 있음)
타입자체에 속하는 프로퍼티는 인스턴스하나에 속하지 않을 수 있다. 이런 프로퍼티의 복사본은 인스턴스를 몇개를 만들든지 하나만 있다. -> *타입 프로퍼티*
타입프로퍼티는 모든 인스턴스가 사용하는 공통적인 값을 정의하는데 유용하다.(모든 인스턴스가 사용할 수 있는 상수, 변수 프로퍼티라든지)
* Note : 타입프로퍼티는 인스턴스 프로퍼티와 다르게 항상 기본값(default value, 초기값)을 설정해줘야 한다. (왜냐하면 타입자체에 초기화할 때 저장된 타입프로퍼티에 값을 할당할 수 있는 이니셜라이저를 가지고 있지 않기 때문에). 저장 타입프로퍼티는 처음 액세스할 때 항상 지연초기화된다. 이것은 다중 스레드 환경에서 동시에 액세스 할 때도 한번만 초기화 된다는 것을 보장한다.(lazy 키워드가 필요 없음)
* Type Property Syntax
    - 스위프트에서 타입프로퍼티는 타입의 정의 일부로 작성된다.
    - 타입프로퍼티 키워드 : `static`
    - 클래스 타입에 대한 연산 타입 프로퍼티로는 `class` 키워드를 사용할 수 있다. `class`키워드를 사용하는 것은 subclass가 superclass의 구현을 재정의(override)할 수 있도록 허용한다.
    ```swift
    struct SomeStructure {
        static var storedTypeProperty = "Some Value."
        static var computedTypeProperty: Int {
            return 1
        }
    }

    enum SomeEnumeration {
        static var storedTypeProperty = "Some Value."
        static var computedTypeProperty: Int {
            return 6
        }
    }

    class SomeClass {
        static var storedTypeProperty = "Some Value."
        static var computedTypeProperty: Int {
            return 27
        }

        class var overrideableComputedTypeProperty: Int {
            return 107
        }
    }
    // 아래 계속..
    ```
* Querying and Setting Type Properties
타입프로퍼티는 인스턴스 프로퍼티처럼 dot syntax를 통해 조회되고(query) 설정한다. 하지만 타입프로퍼티는 인스턴스가 아니라 타입 자체에서 조회되고 설정한다.
    - 위에 말을 예제를 통해 보면 똑같이 dot syntax를 사용하지만 인스턴스가 아니라 타입자체에 dot syntax를 통해서 설정하고 조회할 수 있다는 의미
      ```swift
      print(SomeStructure.storedTypeProperty)
      // Prints "Some value."
      SomeStructure.storedTypeProperty = "Another value."
      print(SomeStructure.storedTypeProperty)
      // Prints "Another value."
      print(SomeEnumeration.computedTypeProperty)
      // Prints "6"
      print(SomeClass.computedTypeProperty)
      // Prints "27"
      ```
      - audio Channel 예제를 통한 타입프로퍼티 이해
      ```swift
      struct AudioChannel {
          static let thresholdLevel = 10
          static var maxInputLevelForAllChannels = 0
          var currentLevel: Int = 0 {
              didSet {
                  if currentLevel > AudioChannel.thresholdLevel {
                      // cap the new auido level to the threshold level
                      currentLevel = AudioChannel.thresholdLevel
                  }
                  if currentLevel > AudioChannel.maxInputLevelForAllChannels {
                      // store this as the new overall maximum input level
                      AudioChannel.maxInputLevelForAllChannels = currentLevel
                  }
              }
          }
      }

      var leftChannel = AudioChannel()
      var rightChannel = AudioChannel()
      leftChannel.currentLevel = 7
      print(leftChannel.currentLevel)
      print(AudioChannel.maxInputLevelForAllChannels)
      rightChannel.currentLevel = 11
      print(rightChannel.currentLevel)
      print(AudioChannel.maxInputLevelForAllChannels)
      ```
