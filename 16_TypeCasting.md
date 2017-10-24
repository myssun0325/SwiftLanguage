## Type Casting

타입캐스팅은 인스턴스의 타입을 체크하거나 다른 superclass나 subclass로 취급하기 위한 방법이다.
키워드 : `is` `as`
또한 타입캐스팅을 타입이 프로토콜을 준수하는지에 대해서도 사용할 수 있다.

#### Defining a Class Hierarchy for Type Casting
타입캐스팅을 클래스와 서브클래스에 대해서 특정 클래스의 인스터스 타입을 확인하거나 인스턴스가 같은 계층구조안에 다른 클래스로 cast하기위해 사용할 수 있다. 아래 코드는 클래스들의 계층구조와 이 클래스들의 인스턴스를 포함하고 있는 배열을 정의하고 있다.(타입 캐스팅 예제)

* 첫번째 코드
    ```swift
    class MediaItem {
      var name: String
      init(name: String) {
        self.name = name
      }
    }
    ```
* 두번째 코드(MediaItem의 서브클래스)
    ```swift
    class Movie: MediaItem {
      var director: String
      init(name: String, director: String) {
        self.director = director
        super.init(name: name)
      }
    }

    class Song: MediaItem {
      var artist: String
      init(name: String, artist: String) {
        self.artist = artist
        super.init(name: name)
      }
    }
    ```
* 세번째 코드
    ```swift
    let library = [
        Movie(name: "Casablanca", director: "Michael Curtiz"),
        Song(name: "Blue Suede Shoes", artist: "Elvis Presley"),
        Movie(name: "Citizen Kane", director: "Orson Welles"),
        Song(name: "The One And Only", artist: "Chesney Hawkes"),
        Song(name: "Never Gonna Give You Up", artist: "Rick Astley")
    ]
    // the type of "library" is inferred to be [MediaItem]

    ```
* 마지막 세번째 코드를 통해서 array의 요소에 접근하려고 iterate를 하게되면 아이템은 원하는 `Movie`나 `Song`타입이 아니라 `MediaItem`을 받게 된다. Movie나 Song이 MediaItem타입이 아닌 그들의 native type (`Movie`나 `Song`)으로 접근하길 원하면, 그 타입을 체크하고 다른 타임으로 다운캐스팅`downcasting` 을 실행해야 한다. 아래 그 방법을 설명한다.

#### Checking Type
