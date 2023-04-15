# SwiftUI Style Guide

쿠링에서 작성한 SwiftUI 한국어 스타일 가이드

## 기본사항

이 섹션에서는 Swift 자체에 대한 기본적인 컨벤션을 제공합니다

### 파일

- **라인수는 최대 100줄을 넘기지 않습니다.**
    
    > **왜?** 100줄이 넘어가면 가독성이 떨어집니다. 
    
    Xcode 👉🏻 Preferences 👉🏻 Text Editing 👉🏻 ✅ Page guide at column: 100
    
- **파일의 코드 수는 최대 300줄을 넘기지 않습니다.**
    
    기능에 따라 extension으로 나눠서 파일을 분리하도록 합니다.
    
- **파일명은 클래스 이름을 사용하고, extension의 경우 `*.SearcherDelegate.swift` 와 같이 기능을 명시합니다.**
    
    예) `SearchEngine.SearcherDelegate.swift`
    
- **프레임워크의 클래스에 extension을 넣는 경우 `*.KuringLite.swift` 와 같이 프로젝트명을 명시합니다**
    
    예) `View.KuringLite.swift`
    
### 선언
- Swift의 타입 명시가 필요하지 않은 장점을 살리기 위해 선언되는 변수들의 타입은 선택사항으로 작성하되, 타입이 유추가능한 변수명을 선택해야 합니다.
    
    ```swift
    // ✅ GOOD
    var title: String = "This is title"
    var notices: [Notice]
    
    // 👎 BAD
    var title = "This is title"
    ```
    
### 네이밍

- **1글자 네이밍 또는 약어만 사용하지 않습니다**
    
    ```swift
    // ⛔️ 이렇게 하면 안돼요!
    let btn = UIButton()
    
    // ✅ 이렇게 해주세요!
    let button = UIButton()
    let saveButton = UIButton()
    ```
    
    ```swift
    
    Kuring.someAsyncMethod(parameter1) { error in 
        // ⛔️ 이렇게 하면 안돼요!
        if let e = error { ... }
    
        // ✅ 이렇게 해주세요!
        if let error = error { ... }
    }
    ```
    
- **PascalCase(대문자로 시작)하는 경우는 오직 프로토콜, 타입(클래스, 구조체) 에서만 가능합니다. 그 외의 경우는 lowerCamelCase를 사용합니다.**
    
    ```swift
    protocol SomeDelegate: AnyObject {
        func someMethod(_ param: Type)
    }
    ```
    
- **약어의 경우 앞에 오는 경우를 제외하고 전부 대문자로 작성합니다.**
    
    ```swift
    // ⛔️ 이렇게 하면 안돼요!
    let httpUrl = URL(string: "https://")
    
    // ✅ 이렇게 해주세요!
    let httpURL = URL(string: "https://")
    let urlString = "https://"
    ```
    
- **Bool 타입은 `is + 형용사` 또는 `3인칭단수형` 동사를 사용합니다**
    
    다른 타입과 헷갈리지 않게 Boolean 타입임을 확실히 명시합니다.
    
    예) `isConnected`, `hasMember`, `showsNotice`
    
- **타입에 대한 힌트를 이름에 넣어야 합니다.**
    
    이름만 보고 어떤 역할을 하는지 짐작할 수 있도록 하여 가독성을 높일 수 있습니다.
    
    ```swift
    var saveButton: UIButton!
    var titleLabel: UILabel!
    
    var messageList: some View { ... }
    struct SearchView: View { ... }
    ```

# SwiftUI Style Guide | 스위프트UI 스타일 가이드

## 네이밍

### 변수명

- lowerCamelCase를 사용합니다.
    ```swift
    var name: String
    var name = "" // 타입 선언은 선택적
    var showsSubsription: Bool
    @State var isOnboardPresented: Bool
    var notices: [Notice]
    ```
    
- Bool 타입의 경우, 하단의 규칙을 따릅니다.
    - `is + 과거분사` : `@State` 속성의 변수는 반드시 이 규칙을 사용합니다. `Binding` 타입을 갖는 parameter 는 반드시 이 규칙을 사용합니다.
    - `3인칭동사` : 일반 변수에서 사용합니다. parameter에서 사용합니다.
    - `과거분사` : 일반 parameter 에서는 `is+과거분사` 형태대신 에서 사용합니다. 예) `enabled`
    
    | 구분 | State 또는 Binding 을 사용 | 사용하지 않음(기본형태) |
    | --- | --- |
    | 변수 | is + 과거분사 | 3인칭동사 |
    | 파라미터 | is + 과거분사 | 3인칭 동사 / 과거분사만 |
    ```swift
    // ✅ GOOD
    @State var isPresented: Bool = false
    
    func activeMenu(showsMenu: Bool) { }
    func activeMenu(activated: Bool) { }
    func someNoticeMethod(bookmarked: Bool) -> [Notice] // isBookmarked 🙅
    
    init(isViewPresented: Binding<Bool>, showsOnboardView: Bool)
    
    // ❌ BAD
    @State var presented: Bool = false // presented & presents 🙅
    func activeMenu(isActive: Bool) { }  // isActive 🙅

    func someNoticeMethod(isBookmarked: Bool) -> [Notice] // isBookmarked 🙅
    ````
    
- Array를 선언하는 경우, 복수의 의미를 가진 단어를 선택합니다. 단, `List` 는 뷰 네이밍에 쓰이므로 가급적 복수형태 타입의 변수명에는 사용하지 않습니다.
    ```swift
    var notices: [Notice] // noticeList 🙅
    ```

### 타입명 (Class, Enum, Actor, Struct)
- UpperCamelCase를 사용합니다.

- **뷰에 대한 네이밍 시** 역할에 따라 다음의 네이밍 규칙을 **반드시** 준수해야합니다. 
    ```swift
    // 리스트를 담당하는 뷰의 경우 `List` 명시
    struct NoticeList: View { ... }
    
    // 리스트의 아이템을 나타는 뷰의 경우 `Row` 명시
    struct NoticeRow: View { ... }
    
    // 좌우 스크롤 리스트의 아이템을 나타내는 뷰의 경우 `Column` 명시
    struct NoticeTypeColumn: View { ... }
    
    // 선택하는 아이템들의 집합을 나타내는 뷰의 경우 `Selection` 또는 `Selector` 명시
    struct SubscriptionSelection: View { ... }
    
    // 일반 뷰의 경우 `View` 명시
    struct FeedbackView: View { ... }
    ```
    
- (MV State 패턴을 사용하는 경우) `ObservableObject`를 준수하는 DataModel의 경우 ViewModel 형식의 네이밍을 가급적 피하도록 합니다.
    ```swift
    class Subscription: ObservableObject { ... }

    struct SubscriptionView: View {
	      // ✅ GOOD!
        @StateObject var subscription = Subscription()
      	// ❌ Not MV State Pattern naming
        @StateObject var viewModel = Subscription()
        // ❌ Not MV State Pattern naming...
        @StateObject var viewModel = SubscriptionViewModel()
    }
    ```
- 단, 뷰 이름 또는 이미 존재하는 Model과 중복되는 경우 뒤에 Model를 명시합니다. (MV State 패턴을 사용하는 경우) 뷰에서는 dataModel 이라는 네이밍을 사용합니다.

    ```swift
    class NoticeListModel: ObservableObject { ... }

    struct NoticeList: View {
        // ✅ GOOD!
        @StateObject var dataModel = NoticeListModel()
        // ❌ Not MV State Pattern naming
        @StateObject var viewModel = NoticeListModel()
        // ❌ Not MV State Pattern naming
        @StateObject var viewModel = NoticeListViewModel()
    }
    ```

### 함수명

- 함수 네이밍 시, 첫 파라미터 네이밍과 함수명이 중복인 경우 `_ {parameter.name}` 형태로 선언하여 파라미터 이름이 외부에서 노출되지 않게 합니다.
    ```swift
    func readNotice(_ notice: Notice)
    ```
- 선언된 함수를 사용할 때 하나의 영어문장이 되게끔 작명합니다.
    ```swift
    func searchNotice(named name: String) { ... }

    // 사용 시
    searchNotice(named: "2023")
    ```

## 선언 순서

### 변수

- 변수 선언은 하단의 순서로 공백라인 없이 작성합니다.
    - Property wrapper
        - `@Enviroment`
        - `@StateObject`
        - `@EnvironmentObject`
        - `@ObservedObject`
        - `@Published` or `@State`
        - `@Binding`
    - 기본 타입의 변수
    - `didSet` 과 같은 클로져가 있는 변수
        ```swift
        @Enviroment(\.appearance) var appearance
        @StateObject private var dataModel1 = DataModel()
        @EnvironmentObject private var dataModel2: DataModel
        @ObservedObject var dataModel3: DataModel
        @State private var isPresented: Bool = false
        @Binding var searchText: String
        
        var showsMenu: Bool = true
        var bookmarkedNotices: [Notice] {
            willSet { dataModel1.updateBookmark(newValue) }
        }
        ```
### 모듈(import)

- `import` 는 모듈명 길이순으로 공백라인 없이 작성하고, 중복된 모듈구성을 피합니다.
    
    ```swift
    // ✅ GOOD
    
    import SwiftUI // 길이가 같은 경우 중요도나 사용빈도가 높은 순으로 배치합니다
    import Combine
    import Firebase
    import KuringSDK
    import KuringCommons
    
    // 👎 BAD
    
    import KuringSDK (❌: UIKit 이나 SwiftUI 보다 모듈명이 긴 경우 -> import 순서 바뀌어야 함)
    import UIKit
    import SwiftUI (❌: 중복된 모듈)
    
    ```
    

## 액세스 레벨

---

- `@State`, `@StateObject`, `@EnvironmentObject` 는 가급적 private 으로 선언합니다.
    
    ```swift
    // ✅ GOOD
    @StateObject private var subscription = Subscription()
    
    @State private var isPresented: Bool = false
    
    @EnviromentObject private var subscription: Subscription
    ```
    

## 주석

---

- 코드에 대한 설명은 `///` 로 `/` 3개를 이어 작성합니다.
    - public 한 프로퍼티 또는 메소드는 반드시 ```swift 키워드를 사용하여 코드사용예시를 같이 작성해주어야 합니다.
    - 코드 설명은 한국어로 작성합니다.
- 유사한 로직 처리를 위한 함수들은 `// MARK: -` 를 이용하여 처리해 줍니다.
- extension에 작성하게 되는 프로토콜의 경우, extension code block 당 1개의 프로토콜만 구현되어 있어야 합니다.
    
    ```swift
    // ✅ GOOD
    /// 온보딩 보여주기
    .fullScreenCover(isPresented: $isOnboardPresented) {
        // ...
    }
    
    // MARK: - UNUserNotificationCenter
    extension AppDelegate: UNUserNotificationCenter { }
    
    // MARK: - Firebase MessagingDelegate 
    extension AppDelegate: MessagingDelegate { }
    
    // 👎 BAD
    // MARK: - UITableViewDelegate, UITableViewDatasource 
    extension AppDelegate: UNUserNotificationCenter, MessagingDelegate { }
    ```
