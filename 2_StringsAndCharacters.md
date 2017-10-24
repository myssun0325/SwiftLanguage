* 스위프트의 String type은 Foundation의 NSString class와 연결되어 있다. -> 만약에 Foundation을 import하면 String에서 NsString 메서드를 사용할 수 있다(casting 없이).

#### String Literals
* Special Characters in String Literals
    - `\0` : null character
    - `\\` : backslash
    - `\t` : horizontal tab
    - `\n` : line feed
    - `\r` : carriage return
    - `\"` : double quote
    - `\'` : single quote

#### Initializing an Empty String
```swift
var emptyString = "" // empty string literal
var anotherEmptyString = String() // initializer syntax

// Find out whether a String value is empty by checking its Boolean `isEmpty` property
if emptyString.isEmpty {
  print("Nothing to see here")
}
```

#### String Mutability
```swift
var variableString = "Horse"
variableString += " and carriage"
```

#### String Are Value Types
* 스위프트에서 String은 참조타입이 아닌 값타입
* 스위프트 컴파일러는 문자열 사용을 최적화하여 실제 복사가 절대적으로 필요한 경우에만 복사한다.(성능이 좋음)

#### Working with Characters
> let exclmationMark: Character = "!"

#### Concatenating Strings and Characters
```swift
let string1 = "hello"
let string2 = " there"
var welcome = string1 + string2
```
* Character value를 String variable에 append 시킬 수 있음
```swift
let exclmationMark: Character = "!"
welcome.append(exclmationMark)
```

#### String Interpolation
```swift
let multiplier = 3
let message = "\(multiplier) times 2.5 is \(Double(multiplier) * 2.5)"
```

#### Unicode
* Unicode is an international standard for encoding, representing, and processing text in different writing systems.
* 스위프트의 String과 Character 타입은 완전히 유니코드와 호환된다.
* Unicode Scalars
    - 스위프트의 native String 타입은 Unicode Scalar값으로 만들어졌다.
    - Unicode Scalar는 21-bit숫자다.
    - Unicode Scalar는 U+0000 ~ U+D7FF 또는 U+E000 ~ U+10FFFF 범위를 모두 포함하는 Unicode code point다.
    - Unicode Scalar는 Unicode surrogate pair code points(U+D800 ~ U+DFFF)를 포함하지 않는다.
    - 21-bit 유니코드 스칼라에 문자만 할당되어 있는것이 아니라 나중에 할당을 위해 예약되어있는 스칼라도 있다.
    - 문자에 할당된 스칼라는 일반적으로 이름을 갖고있다.(예: LATIN SMALL LETTER A("a"))

* Extended Grapheme Clusters
    - grapheme : 언어의 최소 단위 (a, b, c, …), cluster : 집단, 무리, 접속된 여러개의 단말들
    - 스위프트의 Character 타입의 모든 인스턴스는 하나의 extended grapheme clutser를 나타낸다.
    - Extended grapheme cluster는 하나이상의 유니코드 스칼라(사람이 읽을 수 있는 하나의 문자를 생성하는)의 sequence이다.
    - extended grapheme clutser는 유니코드 스칼라의 순서로 사람이 읽을 수 있는 문자를 만든다.
    ```swift
    let precomposed: Character = "\u{D55C}"                  // 한
    let decomposed: Character = "\u{1112}\u{1161}\u{11AB}"   // ᄒ, ㅏ, ㄴ  
    // 둘의 결과는 '한'으로 같음
    ```

#### Counting Characters(예제 중요!)
* 스위프트에서 extended grapheme clusters를 사용하는것을 잊지말 것
```swift
var word = "cafe"
print("the number of characters in \(word) is \(word.count)")
// Prints "the number of characters in cafe is 4"

word += "\u{301}"    // COMBINING ACUTE ACCENT, U+0301

print("the number of characters in \(word) is \(word.count)")
// Prints "the number of characters in café is 4"
```

#### Accessing and Modifying a String
* 문자열의 프로퍼티와 메서드를 통해서 문자열에 접근하고 수정할 수 있고 또한 서브스크립트 문법을 통해서도 가능하다.
* String Indices
    - String 값에는 index type과 연관된 String.Index를 갖고 있다. String.Index는 문자열에서 각 문자의 위치에 해당한다.
    - 서로 다른 문자는 저장되기 위해서 서로 다른 양의 메모리를 요구한다. 그래서 문자열에서 어떤 문자가 어느 위치에 있는지 결정하려면 문자열의 시작이나 끝에서부터 각각 유니코드스칼라를 iterate over 해야한다. 이런 이유로 스위프트의 문자열은 integer 값으로 인덱싱을 할 수 없다.
    - `startIndex` 프로퍼티는 문자열의 첫번째 문자의 위치에 접근하기위해 사용한다.
    - `endIndex` 프로퍼티는 문자열의 *마지막 문자 다음 위치* 에 접근할 때 사용한다. 그래서 문자열의 서브스크립트의 valid argument로 사용할 수 없다. 만약에 문자열이 비어있다면 startIndex와 endIndex는 같다.
    - 주어진 인덱스의 전이나 후의 인덱스에 접근하려면 문자열의 `index(before:)`, `index(after:)`메서드를 사용하면 된다.
    - 주어진 인덱스로부터 멀리 떨어져있는 인덱스에 접근하려면 `index(_:offsetBy:)`메서드를 사용하면 된다.
    - 서브스크립트 문법을 통해 특정 문자열 인덱스에 있는 문자에 접근할 수 있다.
        ```swift
        let greeting = "Guten Tag!"
        greeting[greeting.startIndex] //  G
        greeting[greeting.index(before: greeting.endIndex)] // !
        greeting[greeting.index(after: greeting.startIndex)] // u
        let index = greeting.index(greeting.startIndex, offsetBy: 7)
        greeting[index] // a
        ```
    - 문자열의 범위 밖에 있는 인덱스에 접근하려고하면 runtime error 발생
    - `indices`프로퍼티를 통해 string의 모든 문자에 접근할 수 있다.
        ```swift
        for index in greeting.indices {
          print("\(greeting[index]) " , terminator: "")
        }
        print
        ```
    - NOTE: Collection 프로토콜을 준수하는 모든 타입에 대해서 startIndex, endIndex 프로퍼티와 index(before:), index(after:), `index(_:offsetBy:)`메소들을 사용할 수 있다.

* Inserting and Removing
    ```swift
    var welcome = "hello"
    welcome.insert("!", at: welcome.endIndex)
    // welcome now equals "hello!"

    welcome.insert(contentsOf: " there", at: welcome.index(before: welcome.endIndex))
    // welcome now equals "hello there!"

    welcome.remove(at: welcome.index(before: welcome.endIndex))
    // welcome now equals "hello there"

    let range = welcome.index(welcome.endIndex, offsetBy: -6)..<welcome.endIndex
    welcome.removeSubrange(range)
    // welcome now equals "hello"
    ```
    - `insert(_:at:), insert(contentsOf:at:), remove(at:),removeSubrange(_:)`메서드는 RangeReplaceableCollection 프로토콜을 준수하는 타입에서 사용가능

* Substrings
    ```swift
    let greeting = "Hello world!"
    let index = greeting.index(of: ",") ?? greeting.dendIndex
    let beginning = greeting[..<index]
    // beginning is "Hello"

    // Convert the result to a String for long-term storage.
    let newString = String(beginning)
    ```
    - 문자열에 대한 작업 중 잠깐동안만 substring을 사용
    - 오랫동안 저장할 준비가 될때 위 코드처럼 substring을 String의 인스턴스로 변환해서 사용
    <img src="pics/pic_1.png" />
    - substring은 원래 문자열을 저장하는데 사용되는 메모리의 일부를 다시 사용할 수 있다.(for 성능최적화 -> 문자열이나 substring을 수정할 때까지 추가적으로 메모리를 복사해야되는 cost를 줄일 수 있다.) 이와 같은 이유 때문에 오래 저장하는것에 대해서 오래 유지되어야한다면 String인스턴스로 다시 저장해서 사용
* String, Substring은 모두 `StringProtocol`을 준수. 문자열을 조작하는 함수가 StringProtocol value를 받아들일 때 편리하다. 이런 함수는 String 또는 SubString value로 호출할 수 있다.

#### Comparing Strings
* textual value 비교 3가지 방법
    1. string and character equality
        ```swift
        let quotation = "We're a lot alike, you and I."
        let sameQuotation = "We're a lot alike, you and I."
        if quotation == sameQuotation {
          print("These two strings are considered equal")
        }
        ```
        두 개의 배교 대상의 extended grapheme clusters이 같으면(canonically equivalent) 같은 것으로 간주.  
        \u{E9} == \u{E9}
    2. prefix equality
    3. suffix equality


#### Unicode Representations of Strings(문자열의 유니코드 표현)
유니코드 문자열이 텍스트파일이나 다른 저장소에 기록될 때, 문자열에 있는 유니코드 스칼라가 몇가지 인코딩 형식(encoding forms)중 하나로 인코딩 된다.
각 인코딩 형식은 코드 단위(code unit)으로 알려진 small chunk로 문자열을 인코딩한다. 여기엔 `UTF-8`, `UTF-16`, `UTF-32`인코딩 형식이 있다.
스위프트는 문자열의 유니코드 표현에 접근하기 위한 몇가지 방법을 제공한다. 가장 기본적인 방법은 for in 문으로 Character값에 접근하기.
* 3가지 유니코드호환표현으로 문자열 값에 접근하는 방법
    1. A collection of UTF-8 code units(문자열의 `utf8`프로퍼티로 접근)
    2. A collection of UTF-16 code units(문자열의 `utf16`프로퍼티로 접근)
    3. A collection of 21-bit Unicode scalar values, 문자열의 UTF-32 인코딩형식과 같음(`unicodeScalars` 프로퍼티로 접근)

```swift
let dogString = "Dog‼🐶"

for codeUnit in dogString.utf8 {
    print("\(codeUnit) ", terminator: "")
}

print()

for codeUnit in dogString.utf16 {
    print("\(codeUnit) ", terminator: "")
}

print()

// Unicode Scalar Representation
for scalar in dogString.unicodeScalars {
    print("\(scalar.value) ", terminator: "")
}
print()

for scalar in dogString.unicodeScalars {
    print("\(scalar) ")
}

print("")
```


* playground 출력관련 issue
```swift
let dogString = "Dog‼🐶"
for codeUnit in dogString.utf8 {
    print("\(codeUnit) ", terminator: "")
}

print("") // 이게 들어가야만 위에 print가 출력되는데. terminator와 관련있는거 같다.

for n in 0...5 {
  print(n, terminator: "")
} // 이렇게만 하면 안나온다. 마찬가지로 아래에 print로 무언가를 해주어야 출력이 된다.
```
