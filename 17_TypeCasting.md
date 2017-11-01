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
* 전체코드
    ```swift
    class MediaItem {
        var name: String
        init(name: String) {
            self.name = name
        }
    }

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


    let library = [
        Movie(name: "Casablanca", director: "Michael Curtiz"),
        Song(name: "Blue Suede Shoes", artist: "Elvis Presley"),
        Movie(name: "Citizen Kane", director: "Orson Welles"),
        Song(name: "The One And Only", artist: "Chesney Hawkes"),
        Song(name: "Never Gonna Give You Up", artist: "Rick Astley")
    ]
    print(type(of: library))
    // the type of "library" is inferred to be [MediaItem]

    var movieCount = 0
    var songCount = 0

    for item in library {
        if item is Movie {
            movieCount += 1
        } else if item is Song {
            songCount += 1
        }
    }

    print("Media library contains \(movieCount) movies and \(songCount) songs")
    // Prints "Media library contains 2 movies and 3 songs"

    ```

#### Downcasting
다운캐스팅 : `as?` `as!`
`as?` : 옵셔널값 리턴, 다운캐스팅의 결과가 불확실할 때
`as!` : 다운캐스팅을 시도하고, 강제언래핑, 결과를 확신할 때 (자식클래스 타입으로의 다운캐스팅)

```swift
class MediaItem {
    var name: String
    init(name: String) {
        self.name = name
    }
}

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


let library = [
    Movie(name: "Casablanca", director: "Michael Curtiz"),
    Song(name: "Blue Suede Shoes", artist: "Elvis Presley"),
    Movie(name: "Citizen Kane", director: "Orson Welles"),
    Song(name: "The One And Only", artist: "Chesney Hawkes"),
    Song(name: "Never Gonna Give You Up", artist: "Rick Astley")
]
print(type(of: library))
// the type of "library" is inferred to be [MediaItem]

var movieCount = 0
var songCount = 0

for item in library {
    if item is Movie {
        movieCount += 1
    } else if item is Song {
        songCount += 1
    }
}

print("Media library contains \(movieCount) movies and \(songCount) songs")
// Prints "Media library contains 2 movies and 3 songs"

for item in library {
    if let movie = item as? Movie {
        print("Movie: \(movie.name), dir. \(movie.director)")
    } else if let song = item as? Song {
        print("Song: \(song.name), by \(song.artist)")
    }
}
// Movie: Casablanca, dir. Michael Curtiz
// Song: Blue Suede Shoes, by Elvis Presley
// Movie: Citizen Kane, dir. Orson Welles
// Song: The One And Only, by Chesney Hawkes
// Song: Never Gonna Give You Up, by Rick Astley

```

* 중요포인트
    * `if let moive = item as? Movie`
    * 해석 : item을 Movie로써 접근하고 만약에 성공하면 새로운 임시상수 `movie`에 옵셔널 Movie로 반환된 값을 저장해라
* Note: 캐스팅은 실제로 인스턴스나 인스턴스의 값을 변경하지 않는다. 내부적으로 인스턴스는 여전히 똑같이 남아있다. 단지 캐스팅된 타입의 인스턴스로 취급되고 접근될 뿐이다.

#### Type Casting for Any and AnyObject
* `Any` : 어떤 타입의 인스턴스든 표현할 수 있다.(함수 타입 포함)
* `AnyObject` : 어떤 클래스타입의 인스턴스든 표현할 수 있다.
이 두개는 정말 명시적으로 필요할 때만 사용하는 것이 좋고 그 외에는 항상 타입 명확히 하는 것이 좋다.

```swift
class MediaItem {
    var name: String
    init(name: String) {
        self.name = name
    }
}

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


let library = [
    Movie(name: "Casablanca", director: "Michael Curtiz"),
    Song(name: "Blue Suede Shoes", artist: "Elvis Presley"),
    Movie(name: "Citizen Kane", director: "Orson Welles"),
    Song(name: "The One And Only", artist: "Chesney Hawkes"),
    Song(name: "Never Gonna Give You Up", artist: "Rick Astley")
]
print(type(of: library))
// the type of "library" is inferred to be [MediaItem]

var movieCount = 0
var songCount = 0

for item in library {
    if item is Movie {
        movieCount += 1
    } else if item is Song {
        songCount += 1
    }
}

print("Media library contains \(movieCount) movies and \(songCount) songs")
// Prints "Media library contains 2 movies and 3 songs"

print("example")
for item in library {
    if let movie = item as? Movie {
        print("Movie: \(movie.name), dir. \(movie.director)")
    } else if let song = item as? Song {
        print("Song: \(song.name), by \(song.artist)")
    }
}


// 여기서부터 Any.

var things = [Any]()

things.append(0)
things.append(0.0)
things.append(42)
things.append(3.14159)
things.append("hello")
things.append((3.0, 5.0))
things.append(Movie(name: "Ghostbusters", director: "Ivan Reitman"))
things.append({ (name: String) -> String in "Hello, \(name)" })

print("--------")
for thing in things {
    switch thing {
    case 0 as Int:
        print("zero as an Int")
    case 0 as Double:
        print("zero as a Double")
    case let someInt as Int:
        print("an integer value of \(someInt)")
    case let someDouble as Double where someDouble > 0:
        print("a positive double value of \(someDouble)")
    case is Double:
        print("some other double value that I don't want to print")
    case let someString as String:
        print("a string value of \"\(someString)\"")
    case let (x, y) as (Double, Double):
        print("an (x, y) point at \(x), \(y)")
    case let movie as Movie:
        print("a movie called \(movie.name), dir. \(movie.director)")
    case let stringConverter as (String) -> String:
        print(stringConverter("Michael"))
    default:
        print("something else")
    }
}

// zero as an Int
// zero as a Double
// an integer value of 42
// a positive double value of 3.14159
// a string value of "hello"
// an (x, y) point at 3.0, 5.0
// a movie called Ghostbusters, dir. Ivan Reitman
// Hello, Michael
```

Any는 옵셔널도 가능하다.
