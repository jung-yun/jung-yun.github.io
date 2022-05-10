---
layout: single
title: "API 디자인 가이드라인(API Design Guidelines) Swift"
date: "2022-04-07"
last_modified_at: "2022-04-08"
---

> Swift 공식 API 디자인 가이드라인의 번역본입니다.
> 포스트 후반부에는 제가 추천하는 API 디자인 가이드라인을 추가하였습니다.

# 기본

- **사용 시점에서의 명확성**이 가장 중요한 목표다. 메서드와 프로퍼티와 같은 한 번 선언되지만 여러번 사용된다. API는 사용시에 정확하고 명확하도록 설계되야한다. 설계를 평가할 땐, 선언을 읽는 것만으로는 충분하지 않을 때가 있다. 그렇기 때문에 항상 유즈 케이스를 검사하여 문맥상 명확한지 확인애햐 한다.
- **간결함보다 명확함이 우선돼야 한다.** 스위프트 코드가 짧아질 순 있지만, 가능한 최소한의 문자로 작성하는 것은 목표가 아니다. 간결함은 강한 타입 시스템의 부수적인 효과이고, 또 자연스레 보일러 플레이트 코드를 줄이는 기능들의 효과이다.
- 모든 선언을 **문서화 하라.** 문서화를 통해 얻는 통찰은 디자인에 지대한 영향을 준다.

> 만약 API의 기능을 간단한 말로 옮겨적기 힘들다면, 아마도 잘못된 API를 디자인한 것이다.

### Detail

- **스위프트의 [dialect of Markdown](https://developer.apple.com/library/archive/documentation/Xcode/Reference/xcode_markup_formatting_ref/)을 사용해라.**
- 엔터티를 설명하는 **요약으로 시작하라.** 보통, API는 선언과 요약을 통해서 완전히 이해 가능하다.
    ```swift
    /// `self`의 `view`를 똑같은 요소를 가지지만 반전된 순서로 리턴한다.
    func reversed() -> ReverseCollection
    ```
           
    - **요약에 집중하라.** 가장 중요한 부분이다. 좋은 문서는 요약 이외에는 필요하지 않다.
    - 가능하다면 **주어나 동사가 생략된 문장을 써라.** 마침표로 문장을 끝내야하며 완전한 문장을 써서는 안된다.
    - **메서드의 기능과 리턴하는 값을 설명하지만** null effect나 void return을 생략하라.
        ```swift
        /// `self`의 앞에 `newHead`를 삽입한다.
        mutating func prepend(_ newHead: Int)

        /// `self`의 요소에 따라오는 `head`를 포함하는 `List`를 반환한다. 
        func prepending(_ head: Element) -> List

        /// `self`가 비어있지 않다면 가장 첫 값을 제거해서 반환한다;
        /// 아니라면 nil을 반환한다.
        mutating func popFirst() -> Element?
        ```
               
        > Note: 위의 *popFirst*같은 경우, 흔하지 않게 요약이 세미 콜론으로 나눠져있다.
           
    - [subscript](https://swjman.tistory.com/15)가 접근하는 값을 명시해라.
           ```swift
           /// `index`번째의 요소에 접근한다.
           subscript(index: Int) -> Element { get set }
           ```
               
    - 이니셜라이저가 무엇을 만들어내는지 명시해라
        ```swift
        /// `x`를 `n`개 가진 객체를 만든다.
        init(count n: Int, repeatedElement x: Element)
        ```
          
    - 모든 선언에서 **선언된 엔터티가 무엇인지 설명해라**
        ```swift
        /// 모든 위치에서 효율적인 삽입/삭제를 지원하는 컬렉션 
        struct List {
        
        /// `self`의 첫번째 요소, 혹은 비어있을 때 `nil` 
        var first: Element?
        ...
        ```
       
- **선택적으로**, 하나 이상의 단락이나 불렛 아이템으로 진행해라. 이때 단락은 빈 라인으로 구분되며 완전한 문장을 쓴다. 
    ```
    /// output의 각 요소 문자 묘사값을 출력하고               ← 요약
    ///                                              ← Blank line
    /// 각 `x`의 문자 묘사값(textual representation)은    ← Additional discussion
    /// `String(x)` 표현으로 생성된다.
    ///
    /// - Parameter separator: 아이템 사이에 출력되는      ⎫
    ///   텍스트                                       ⎟
    /// - Parameter terminator: 끝에 출력되는 텍스트.      ⎬ Parameters section
    ///                                              ⎭
    /// - Note: 문자열 끝에 새로운 라인을 포함하지 않고        ⎫
    ///   출력하기 위해, `terminator: ""` 전달            ⎟
    ///                                              ⎬ Symbol commands
    /// - 참고:     `CustomDebugStringConvertible`,   ⎟
    ///   `CustomStringConvertible`, `debugPrint`.   ⎭
    public func print(
    _ items: Any..., separator: String = " ", terminator: String = "\n")
    
    ```
        
    - 요약 이후에 정보들은 잘 알려진 [Symbol documentation markup](https://developer.apple.com/library/archive/documentation/Xcode/Reference/xcode_markup_formatting_ref/SymbolDocumentation.html#//apple_ref/doc/uid/TP40016497-CH51-SW1)요소를 적절히 사용한다. 
    - 불렛 아이템을 사용할 때는 [symbol command symbol](https://developer.apple.com/library/archive/documentation/Xcode/Reference/xcode_markup_formatting_ref/SymbolDocumentation.html#//apple_ref/doc/uid/TP40016497-CH51-SW13)를 사용한다. Xcode와 같은 툴은 아래 불렛 아이템들을 특별취급한다.   
           
        |---|---|---|---|
        | Attention | Author | Authors | Bug |    
        | Complexity | Copyright | Date | Experiment |   
        | Important | Invariant | Note | Parameter |     
        | Parameters | Postcondition | PreCondition | Remark |
        | Requires | Returns | SeeAlso | Since |
        | Throws | ToDo | Version | Warning |
        
           
# 이름짓기

## 정확한 사용을 지향해라

- 코드를 읽는 사람이 가질 수 있는 **애매모호함을 피하기 위해 필요한 모든 단어를 포함시켜라.**
    - 예를 들어, 콜렉션의 특정 위치에 요소를 지우는 메서드를 생각해보자
        ```swift
        extension List {
            public mutating func remove(at position: Index) -> Element
        }
        employees.remove(at: x)
        ```
    
        만약 위 메서드 시그네쳐에서 `at`을 지운다면, 독자에게 x 값의 요소를 찾아서 지우라는 것을 암시할 수 있다. 실제로는 x가 요소의 포지션인데도 말이다.
        ```swift
        employees.remove(x) // 불확실함: x라는 요소를 지우는 것인가?
        ```
    
- **필요없는 단어를 지워라.** 이름안의 모든 단어는 사용처에서 핵심적인 정보를 전달해야 한다.
    - 더 많은 단어가 의도를 명확히하고 의미의 애매모호함을 없애기 위해 필요할지모른다. 다만, 독자가 이미 가지고 있는 정보를 반복해서 가지고 있는 단어는 생략돼야 한다. 특히, 단순히 타입 정보만을 반복하는 단어는 생략해라.
        ```swift 
        public mutating func removeElement(_ member: Element) -> Element?

        allViews.removeElement(cancelButton)
        ```
        
        이 경우에 Element는 유의미한 정보를 전달하고 있지 않다고 본다. 아래로 개선할 수 있다.
        ```swift
        public mutating func remove(_ member: Element) -> Element?

        allViews.remove(cancelButton) // clearer
        ```
          
        때때로, 타입 정보를 반복하는 것이 애매모호함을 줄이기 위해 필요할 때도 있다, 하지만 보통 파라미터의 역할을 적시하는 것이 타입을 적는 것보다 좋다. 디테일을 위해 다음 아이템을 보자.

## 유창한 사용을 위해 노력해라
- 메서드 이름은 영어 문법에 맞는 문장이 되도록 노력해라
    ```swift
    /// 옳은 예시
    x.insert(y, at: z)          “x, insert y at z”
    x.subViews(havingColor: y)  “x's subviews having color y”
    x.capitalizingNouns()       “x, capitalizing nouns”
    ```
    
    ```swift
    /// 나쁜 예시
    x.insert(y, position: z)
    x.subViews(color: y)
    x.nounCapitalize()
    ```
      
    만약 첫 번째나 두 번째 argument 이후의 argument가 call의 핵심 의미에 영향을 주지 않는다면 그 땐 문장의 문법에 신경쓰지 않아도 좋다.
      
    ```swift
    AudioUnit.instantiate(
    with: description, 
    options: [.inProcess], completionHandler: stopProgressBar)
    ```
    
- **팩토리 메서드의 이름을 "make"로 시작해라**, e.g. x.makeIterator().
- 이니셜라이저나 팩토리 메서드의 첫번째 argument는 메서드 이름으로 시작되는 문장의 일부가 돼서는 안된다.
    ```swift
    /// 옳은 예시
    let foreground = Color(red: 32, green: 64, blue: 128)
    let newPart = factory.makeWidget(gears: 42, spindles: 14)
    let ref = Link(target: destination)
    ```
    
    ```swift
    /// 나쁜 예시
    let foreground = Color(havingRGBValuesRed: 32, green: 64, andBlue: 128)
    let newPart = factory.makeWidget(havingGearCount: 42, andSpindleCount: 14)
    let ref = Link(to: destination)
    ```
    
    실제론 이 규칙과 argument labels 규칙이 합쳐서 value preserving type conversion이 아닌 경우에만 첫 argument의 label이 가능한 것으로 이해하면 된다.
    
    ```swift
    let rgbForeground = RGBColor(cmykForeground)
    ```

- 부수 효과(side-effect)를 고려해서 메서드 이름을 지어라.
    - 부수 효과가 없는 메서드는 명사로 읽혀져야 한다, e.g. x.distance(to: y), i.successor().
    - 부수 효과가 있는 경우 명령형의 동사를 사용한다, e.g. print(x), x.sort(), x.append(y)
    - Mutating/nonmutating 메서드 쌍을 일관되게 이름 짓는다. Mutating 메서드는 보통 같은 의미의 nonmutating 메서드를 가진다. 그 자리에서 개체를 업데이트하는 mutating 메서드와 다르게 nonmutating 메서드는 새로운 값을 만들어 리턴한다.
        - 메서드의 동작이 자연스럽게 동사로 표현되는 경우, mutating 메서드에는 명령형을 쓰고, "ed", "ing" 등의 접미사를 써서 nonmutating 메서드를 네이밍해라.  
            |---|---|
            |x.sort()|z = x.sorted()|
            |x.append(y)|z = x.appending(y)|
            
            - nonmutating 메서드는 과거형(흔히 "ed")를 쓰는 것을 지향해라.
                ```swift
                /// `self`를 그 자리에서 뒤집는다.
                mutating func reverse()

                /// `self`의 복사본에서 그 값을 뒤집어 리턴한다.
                func reversed() -> Self
                ...
                x.reverse()
                let y = x.reversed()
                ```
                
            - "ed"를 영어 문법 상 붙이기 어색할때, nonmutating 메서드를 현재형("ing")으로 선언해라.
                ```swift
                /// `self`의 newlines에서 공백을 제거한다.
                mutating func stripNewlines()

                ///  `self`의 복사본에서 공백을 제거하고 리턴한다.
                func strippingNewlines() -> String
                ...
                s.stripNewlines()
                let oneLine = t.strippingNewlines()
                ```
                
        - 동작이 자연스럽게 명사로 표현된 경우, nonmutating 메서드는 명사로 표현하고 mutating 메서드는 "form" 접두어를 붙여 표현해라.
            
            |---|---|
            |x = y.union(z)|y.formUnion(z)|
            |j = c.successor(i)|c.formSuccessor(&i)|
            
- 불리언 메서드와 프로퍼티는 nonmutating일 경우 **주장(assertion)으로 받아들여질 수 있어야한다,**e.g. x.isEmpty, line1.intersects(line2).
- 무엇인지 표현하는 프로토콜은 명사로 표현된다(e.g. Collection). 
- Capability를 나타내는 프로토토콜은 able, ible, **or** ing 접미사로 표현한다(e.g. Equatable, ProgressReporting).
- 다른 타입, 프로퍼티, 변수, 그리고 상수는 명사로 읽혀야 한다.


## 전문 용어(Terminology)를 잘 사용해라

> Terminology: 특정한 전문 분야에서 사용되는 언어로 특별한 의미가 있다.

- 만약 좀 더 일반적인 언어로 충분히 대체 가능하다면 **잘 알려져 있지 않은 단어를 피해라.** 만약 "skin"이 목적 달성 상 충분하다면 "epidermis"라는 단어를 쓰지 마라. 전문용어는 커뮤니케이션 툴로서 필수적인 역할을 하지만 정말 필요할때가 아니라면 쓰이지 않아야 한다.
- 전문 용어를 사용할 땐 **이미 정의된 의미를 사용하라.**
    전문용어를 사용할 때는 일반적인 단어를 사용했을 때 그 의미가 애매모호하거나 명확하지 않았을 때다. 그러므로 API에서 사용되는 단어는 일반적으로 받아들여지는 의미를 엄격하게 지켜야한다.
    
    - **전문가를 놀래키지 마라.**: 이미 그 단어와 익숙한 누군가는 당신이 단어에 새로운 의미를 부여했을 때 놀라거나, 혹은 화낼지도 모른다.
    - **신입을 헷갈리게 하지마라.**: 그 단어를 처음 보는 누군가는 높은 확률로 웹의 전통적인 의미를 찾을 것이다.

- **축약어(Abbreviation)를 피해라.** 축약어, 특히 일반적인 것이 아니라면 피해야한다.
    > 축약어의 의도된 의미는 웹 서치로 쉽게 찾을 수 있어야 한다.
    
- **관례를 버리지마라.** 신입을 위해 단어를 최적화 하는데 그것이 현재의 관례에 위배되는 것이라면 잘못된 것이다.
    연속된 데이터 구조를 List라고 칭하지 않고 Array라고 칭하는 것이 좋다. 초보자는 List라는 단어의 의미를 좀 더 빨리 파악할지라도 말이다. Array는 현대 컴퓨팅의 핵심적인 요소로, 모든 개발자가 알고있고 혹은 곧 배울 것이다. 거의 모든 프로그래머가 익숙하거나, 그들이 웹 서치나 질문으로 충분히 원하는 정보를 얻을 수 있는 단어를 써야한다.   
       
    특정 프로그래밍 영역, 예를들어 수학에서는, 관례적으로 sin(x)가 verticalPositionOnUnitCircleAtOriginOfEndOfRadiusWithAngle(x)보다 널리 받아들여진다. 이 경우, 관례를 따라야한다는 원칙이, 축약어를 사용하면 안된다는 원칙보다 더 중요하다(완전한 단어는 엄밀히 말하면 sine이지만 말이다). "sin(x)"는 프로그래머가 몇 십년동안 사용해왔고, 수학자들의 경우에는 몇 세기나 사용해온 단어다.   
    

# 관습(Conventions)

## 일반적인 관습(General Conventions)
- **O(1)이 아닌 computed 프로퍼티의 복잡도를 항상 표시해라.** 사람들은 보통 프로퍼티 접근엔 대단한 연산이 필요하지 않다고 가정한다. 이는 stored 프로퍼티의 개념이 그들에게 익숙하기 때문이다. 독자들에게 이 가정이 틀릴 수 있음을 명시해라.
- **free function(non-member function)보다 메서드나 프로퍼티를 지향해라.** Free function은 특별한 경우에만 쓰인다.
    1. 명확한 self가 없는 경우
        ```swift
        min(x, y, z)
        ```
    2. function이 제한이 없는 generic인 경우
        ```swift
        print(x)
        ```
    3. function의 문법이 이미 존재하는 domain notation인 경우
        ```swift
        sin(x)
        ```
        
- **대, 소문자 관습을 따라라.** 데이터 타입이나 프로토콜의의 경우 UpperCamelCase, 다른 모든 것은 lowerCamelCase를 따른다.
    미국의 영어 기준 일반적으로 대문자로 나타나는 두문자어-[Acronyms and initialisms](https://en.wikipedia.org/wiki/Acronym)는 모두 함께 uppercased되거나 downcased돼야 한다.<br>
    
    ```swift
    var utf8Bytes: [UTF8.CodeUnit]
    var isRepresentableAsASCII = true
    var userSMTPServer: SecureSMTPServer
    ```
    
    다른 두문자어의 경우 일반적인 단어로 취급된다.<br>
    
    ```swift
    var radarDetector: RadarScanner
    var enjoysScubaDiving = true
    ```
    
- 메서드들이 같은 의미를 공유하거나 서로 다른 도메인에서 작동한다면 **메서드는 이름을 공유할 수 있다.**   
    예를들어 아래는 권장할만 하다. 메서드들은 본질적으로 같은 일을 하기 때문이다.<br>
        
    ```swift
    /// 좋은 예시
    extension Shape {
    /// Returns `true` iff `other` is within the area of `self`.
    func contains(_ other: Point) -> Bool { ... }

    /// Returns `true` iff `other` is entirely within the area of `self`.
    func contains(_ other: Shape) -> Bool { ... }

    /// Returns `true` iff `other` is within the area of `self`.
    func contains(_ other: LineSegment) -> Bool { ... }
    }
    ```
    
    그리고 geometric 타입과 collection은 서로 다른 도메인이므로, 아래의 경우도 가능하다.<br>
    
    ```swift
    /// 좋은 예시
    extension Collection where Element : Equatable {
    /// Returns `true` iff `self` contains an element equal to
    /// `sought`.
    func contains(_ sought: Element) -> Bool { ... }
    }
    ```
    
    하지만 아래의 index 메서드들은 다른 의미를 가지므로, 다르게 이름 지어져야 한다. <br>
        
    ```swift
    /// 나쁜 예시
    extension Database {
    /// Rebuilds 데이터 베이스 탐색 인덱스를 리빌드한다.
    func index() { ... }

    /// 테이블의 n번 째 로우를 리턴한다.
    func index(_ n: Int, inTable: TableID) -> TableRow { ... }
    }
    ```
    
    마지막으로 "리턴 타입만 다른 오버로딩"을 피해라. 이는 현재 무슨 타입을 기대할지 애매하게 만든다.<br>
    
    ```swift
    /// 나쁜 예시
    extension Box {
    /// Returns the `Int` stored in `self`, if any, and
    /// `nil` otherwise.
    func value() -> Int? { ... }

    /// Returns the `String` stored in `self`, if any, and
    /// `nil` otherwise.
    func value() -> String? { ... }
    }
    ```

## 파라미터

> func move(from **start**: Point, to **end**: Point)

- 문서화를 위한 파라미터 이름을 선택해라. 파라미터 이름은 메서드를 이용할 때 사용되진 않지만 메서드에 대해 설명하는데 중요한 역할을 한다.
    문서를 읽기 쉽게 파라미터 이름을 설정해라. 예를들어 아래의 이름은 문서를 읽기 쉽게 한다.<br>
    
    ```swift
    /// 좋은 예시
    
    /// `predicate`를 만족하는 `Array`를 리턴한다.
    func filter(_ predicate: (Element) -> Bool) -> [Generator.Element]

    /// `subRange`의 요소를 `newElements`로 대체한다.
    mutating func replaceRange(_ subRange: Range, with newElements: [E])
    
    ```
    
    하지만 아래는 문서화를 어색하게 만든다.<br>
    
    ```swift
    /// 나쁜 예시
    /// `includedInResult`를 만족하는 `Array`를 리턴한다. 
    func filter(_ includedInResult: (Element) -> Bool) -> [Generator.Element]

    /// `r`로 표현되는 range의 요소를 `with` 로 바꾼다.
    mutating func replaceRange(_ r: Range, with: [E])
    ``` 
    
- 일반적인 사용을 편하게 하기 위해 **default 파라미터를 적극활용해라.** 사용성이 단일한 파라미터는 default 값으로 대체 가능하다.
    예를들어 Default arguments는 관련없는 정보를 숨겨서 가독성을 향상시킨다.<br>
    
    ```swift
    /// 나쁜 예시
    let order = lastName.compare(
    royalFamilyName, options: [], range: nil, locale: nil)
    ```
    
    위의 코드는 아래로 간단히 바뀔 수 있다.
    
    ```swift
    let order = lastName.compare(royalFamilyName)
    ```
    
    Default argument는 일반적으로 메서드 family의 사용에서 선호된다. 이를 통해 코드를 이해하는 필요한 burden을 낮출 수 있다.
    
    ```swift
    extension String {
    /// ...description...
    public func compare(
        _ other: String, options: CompareOptions = [],
        range: Range? = nil, locale: Locale? = nil
    ) -> Ordering
    }
    ```
        
    위의 코드는 간단해 보이지 않을 수 있지만 아래와 비교하면 훨씬 간단하단 걸 알 수 있다.
    
    ```swift
    extension String {
        /// ...description 1...
        public func compare(_ other: String) -> Ordering
        /// ...description 2...
        public func compare(_ other: String, options: CompareOptions) -> Ordering
        /// ...description 3...
        public func compare(
            _ other: String, options: CompareOptions, range: Range) -> Ordering
        /// ...description 4...
        public func compare(
            _ other: String, options: StringCompareOptions,
            range: Range, locale: Locale) -> Ordering
    }
    ```
        
    모든 메서드 family의 멤버들은 각각 문서화되고 유저는 이를 읽는다. 이 때 메서드를 선택하기 위해 유저들은 모든 메서드를 이해해야한다. 메서드들 사이의 직관적이지 않은 관계들( 예를들어 foo(bar: nil)과 foo()는 항상 같은 동작을 하는 것이 아니라는 것)은 문서에서도 거의 똑같은 문장을 반복하는 등 불필요한 프로세스를 증가시킨다. 따라서 메서드 패밀리 대신 하나의 메서드에서 default 파라미터를 쓰는 것이 우월하다.
    
- **Default 값을 가진 파라미터는 파라미터 리스트 순서상 끝 쪽으로 밀어라.** 메서드에서 보통 default 값이 없는 파라미터가 의미상 좀 더 중요한 역할을 하고, 이를 앞쪽에 둬서 메서드가 불릴 때 안정적인 initial pattern을 만들 수 있다.    


## Argument Labels
> func move(**from** start: Point, **to** end: Point)   
> x.move(**from**: x, **to**: y)

- **arguments간 구분이 딱히 필요하지 않을 때는 모든 레이블을 생략하라.** 예를들어 min(number1, number2), zip(sequence1, sequence2)
- **이니셜라이저가 value preserving type conversion를 수행할 때는 첫 번째 레이블을 생략해라.** 예를들어 Int64(someUInt32)
     첫 번째 argument는 항상 conversion의 소스가 돼야한다.<br>
    
    ```swift
    extension String {
    // `x`의 radix값을 기준으로 한 textual representation을 만든다.
    init(_ x: BigInt, radix: Int = 10)   ← 첫 부분의 언더스코어를 보라
    }

    text = "The value is: "
    text += String(veryLargeNumber)
    text += " and in hexadecimal, it's"
    text += String(veryLargeNumber, radix: 16)
    ```
    
    하지만 그 범위를 좁히는 type conversion에서는, 무엇을 어떻게 줄이는지에 대한 레이블을 적어주면 좋다.
     
    ```swift
    extension UInt32 {
    /// Creates an instance having the specified `value`.
    init(_ value: Int16)            ← Widening, so no label
    /// Creates an instance having the lowest 32 bits of `source`.
    init(truncating source: UInt64)
    /// Creates an instance having the nearest representable
    /// approximation of `valueToApproximate`.
    init(saturating valueToApproximate: UInt64)
    }
    ```
    
- **첫 번째 argument가 전치사구와 같은 역할을 할 때는, 레이블을 적어라.** augument label는 보통 전치사로 시작해야한다, e.g. x.removeBoxes(havingLength: 12).
    예외는 첫 번째 두 argument가 똑같은 추상의 일부분씩 표현하는 경우, 예를들어
    
    ```swift
    /// 나쁜 예시
    a.move(toX: b, y: c)
    a.fade(fromRed: b, green: c, blue: d)
    ```
        
    위와 같은 경우 전치사 이후에 argument label을 시작해서 그 의미를 명확하게 할 수 있다.
    
    ```swift
    /// 좋은 예시
    a.moveTo(x: b, y: c)
    a.fadeFrom(red: b, green: c, blue: d)
    ```
        
- **하지만 argument label이 아니라 argument 자체가 문법에 맞는 구절을 만들어낸다면, 레이블을 생략해라.** 예를들어 x.addSubview(y)
    반대로 만약 argument가 그 자체로 문법에 맞는 구절을 만들어내지 않는다면, 레이블을 붙여라.
    
    ```swift
    view.dismiss(animated: false)
    let text = words.split(maxSplits: 12)
    let studentsByName = students.sorted(isOrderedBefore: Student.namePrecedes)
    ```
        
    구절이 항상 옳은 의미를 전달하는 것이 중요하단 걸 생각해라. 아래의 예는 문법상 맞지만 의미가 애매모호하다.
    
    ```swift
    view.dismiss(false)   // dismiss하지 말라는 것일까? 혹은 false를 dismiss하라는 것일까? 
    words.split(12)       // 숫자 12를 나누라는 것일까?
    ```
        
- **다른 모든 augument는 label을 붙인다.** 

# Special Instructions

- API에 나타나는 튜플의 멤버와 클로져의 parameter을 레이블링 하라.
    이러한 이름들로 설명하기가 쉬워지고, 해당 요소들이 문서에 언급될 수 있으며, 튜플 요소에 접근하는 표현을 가능하게 한다.
    
    ```swift
    /// Ensure that we hold uniquely-referenced storage for at least
    /// `requestedCapacity` elements.
    ///
    /// If more storage is needed, `allocate` is called with
    /// `byteCount` equal to the number of maximally-aligned
    /// bytes to allocate.
    ///
    /// - Returns:
    ///   - reallocated: `true` iff a new block of memory
    ///     was allocated.
    ///   - capacityChanged: `true` iff `capacity` was updated.
    mutating func ensureUniqueStorage(
    minimumCapacity requestedCapacity: Int, 
    allocate: (_ byteCount: Int) -> UnsafePointer<Void>
    ) -> (reallocated: Bool, capacityChanged: Bool)
    ```
        
    클로져 파라미터로 선택되는 이름들은 top-level functions을 위한 parameter names처럼 선택되야 한다. 클로져 argument label들은 호출시 사용이 허용되지 않는다.
    
- 오버로드 세트의 애매모호함을 피하기 위해서 unconstrained polymorphism 사용에 주의해라(e.g. Any, AnyObject, 그리고 unconstrained generic parameters).
    예를들어 아래의 오버로드 세트를 살펴보면
    
    ```swift
    struct Array {
    /// Inserts `newElement` at `self.endIndex`.
    public mutating func append(_ newElement: Element)

    /// Inserts the contents of `newElements`, in order, at
    /// `self.endIndex`.
    public mutating func append(_ newElements: S)
    where S.Generator.Element == Element
    }
    ```
        
    위의 메서드들은 의미적으로 비슷하게 묶이는데, argument의 타입들도 명확하게 구별되는 것 처럼 보인다. 하지만 Element가 Any일 땐, 하나의 요소나 요소들의 집합이 똑같이 취급될 수 있음에 주의해야 한다.
    
    ```swift
    var values: [Any] = [1, "a"]
    values.append([2, 3, 4]) // [1, "a", [2, 3, 4]] or [1, "a", 2, 3, 4]?
    ```
    
    이런 애매모호함을 없애기 위해, 두번째 오버로드를 좀 더 명확하게 적어줘야 한다.
    
    ```swift
    struct Array {
    /// Inserts `newElement` at `self.endIndex`.
    public mutating func append(_ newElement: Element)

    /// Inserts the contents of `newElements`, in order, at
    /// `self.endIndex`.
    public mutating func append(contentsOf newElements: S)
    where S.Generator.Element == Element
    }
    ```
        
    또 이때 앞서 말했던 labeling이 문서에서의 언급을 어떻게 용의하게 하는지도 알 수 있다.
    

---
    
# 실무에서 내가 제안하는 스타일 가이드

### 코드 생김새
    - 줄맞춤
        - 파일단위 줄 맞춤, 단축키로 쉽게 줄 맞춤할 수 있다.
            ```
            /// 줄 맞춤하기 원하는 영역 선택 혹은 Cmd + a
            Ctrl + i
            ```
    - 띄어쓰기
        - 콜론 ':'의 공간은 오직 오른쪽에 존재한다.
        
            ```swift
            var age: Int
            ```
            
        - 연산자 오버로딩 함수에서 함수이름과 괄호 사이에 한 칸 공백 설정
            
            ```swift
            func ** (lhs: Float, rhs: Float)
            ```
            
        - 이외 함수이름과 괄호 사이 공백은 없다.
            ```swift
            func pass(year: Int)
            ```
    - 줄바꿈
        - 최대 길이를 초과하는 경우에 매개변수를 기준으로 줄바꿈한다.
            
            ```swift
            func tableView( _ tableView: UITableView,    
            cellForRowAt indexPath: IndexPath ) -> UITableViewCell { 
                return UITableViewCell() 
            }
            ```
        
        - 매개변수로 전달될 클로저가 1개 초과의 경우, 내려쓰기한다.
            
            ```swift
            UIView.animate( withDuration: 0.2, animations: { 
                // animation
            }, completion: { finished in 
                // completion
            })
            ```
        - 옵셔널 바인딩 시 다음과 같이 줄바꿈한다.
        
            ```swift
                if let age = age, let company = company { } guard let age = age, let company = company else { return }
            ```

    - MARK 구문을 적절히 활용한다.   
        - 파일단위 extension이 1개 초과일 경우, MARK 구문을 사용한다.
        
    - import
        - 기본적으로 알파벳 순으로 구분한다.
        - 애플제공 프레임워크와 서드파티 프레임워크를 구분해서 적으며 이 둘 사이를 한 줄로 구분한다.
        
    - 클래스
        - 클래스 멤버 순서
            - 정렬 기준은 해당 클래스를 사용하는 입장에서 봤을 때 필요한 정보 순서로 나열한다.
                1. Types
                    a. public types
                    b. private types
                2. Members
                    a. inherited member, implement protocol member
                    b. static member
                    c. public member
                    d. private member
                3. Initializer
                    a. inherited ionitializer
                    b. initializer
                4. Methods
                    a. inherited methods, implement protocol methods
                    b. static methods
                    c. public methods
                    d. privaet methods
    - 네이밍
        - 가급적 함수 이름에서 `get` prefix는 사용하지 않는다.
            - 추천
                ```swift
                func address(for user: User) -> String?
                ```
            - 비추천
                ```swift
                func getAddress(for user: User) -> String?
                ```
    
        - 이 외에는 기본적인 API 가이드를 따른다.
    


















# References

- [API Design Guidelines](https://www.swift.org/documentation/api-design-guidelines/)
-  
