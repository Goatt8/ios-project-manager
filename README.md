# projectManager README

# README
# 프로젝트 매니저 :notebook: 
> 프로젝트를 해야할 일, 진행중인 일, 완료된 일 세가지로 구분지어 관리하는 앱
> 
> 프로젝트 기간: 2023.05.15-2023.06.02
> 

## 팀원
| Goat |
| :--------: | 
| <Img src ="https://i.imgur.com/yoWVC56.png" width="200" height="200"/> |
| [Github Profile](https://github.com/Goatt8) |


## 목차
1. [타임라인](#타임라인)
2. [프로젝트 구조](#프로젝트-구조)
3. [실행 화면](#실행-화면)
4. [트러블 슈팅](#트러블-슈팅) 
5. [핵심경험](#핵심경험)
6. [참고 링크](#참고-링크)

# 타임라인 

### week1

|    날짜    | 내용 |
|:----:| ---- |
| 2023.05.15 | 기술검색 및 자료수집 |
| 2023.05.16 | UI, LocalDB, RemoteDB, 의존성관리도구, 비동기프로그램 기술설정 |
| 2023.05.17 | tableView 및 UI포맷 구현 |
| 2023.05.18 | textField, datePicker, textView, navigationBar UI 구현|
| 2023.05.19 | delegate패턴으로 생성한 데이터 VC간 전달해서 화면에 생성되도록 구현|


<br/>


# 프로젝트 구조

## File Tree
```typescript
ProjectManager
├── ProjectManager
│   ├── Application
│   │   ├── AppDelegate
│   │   ├── LaunchScreen
│   │   └── SceneDelegate
│   ├── Model
│   │   ├── ToDoProtocol
│   │   └── ToDoList
│   │   └── Manager
│   │   │   ├── DateFormatterManager
│   ├── View
│   │   ├── ToDoTableViewCell
│   │   └── extension + UITextView
│   ├── ViewController
│   │   └── ToDoListViewContorller
│   ├── Assets
│   ├── Main
│   ├── Info
└─  └── GoogleService-Info
```

 <br/>

# 실행 화면


|<center> ToDoWriteVC에서 가져온 데이터 <br> tableView에 생성 </center>|
| :---: |
| <img src = "https://github.com/Goatt8/ios-project-manager/assets/62684851/d3dad4a3-01f2-4120-b1dd-b7154f465215" width= 400> | 
|ToDoWriteVC(detailVC)에서 가져온 데이터 <br> ToDoListVC(mainVC)에 생성|





# 트러블 슈팅

## 1️⃣ :fire: modal로 띄운 ViewController 에 navigationBar 뜨지않는 현상
#### 문제점
* 아래처럼 모달로 띄운 뷰컨트롤러에 navigationBar가 띄워지지않는 에러발생

| 수정 전 ( 에러 ) | 수정 후 |
|:----:| :----: |
| <img src = "https://hackmd.io/_uploads/Sy8JTwQB2.png" width = 400> | <img src = "https://hackmd.io/_uploads/BkL1TPQH3.png" width = 400> | 

```swift
//수정전
    @objc private func plusButtonTapped() {
        let toDoWriteViewController = ToDoWriteViewController()
        toDoWriteViewController.modalPresentationStyle = .formSheet
        self.present(toDoWriteViewController, animated: true)
    }

// 수정후
    @objc private func plusButtonTapped() {
        let toDoWriteViewController = UINavigationController(rootViewController: ToDoWriteViewController())
        toDoWriteViewController.modalPresentationStyle = .formSheet
        self.present(toDoWriteViewController, animated: true)
    }


// 2차 수정
@objc private func plusButtonTapped() {
        let toDoWriteViewController = ToDoWriteViewController()
        toDoWriteViewController.modalPresentationStyle = .formSheet
        toDoWriteViewController.delegate = self
        self.present(toDoWriteViewController, animated: true)
}

//ToDoWriteVC - secondVC 네비게이션바 직접 생성
 private let navigationBar: UINavigationBar = {
        let navigationBar = UINavigationBar()
        return navigationBar
 }()
    
```

#### 1차 문제해결
* viewController 자체로 `push`해서 이동하는 방식이 아닌 `modal.present` 했을 때는 
navigationController에 연결된 secondViewController가 아닌 secondViewController자체에 접근하므로 navigationBar가 뜨지않는 에러가 발생합니다.
-> 따라서 `UINavigationController(rootViewContoller:secondViewController)`처럼 NavigationViewController자체에 접근해 이동시키면서 modal방식과 navigationBar 둘다 사용할 수있는 방식으로 문제를 해결했습니다.

#### 2차 문제해결
* 그치만 navigation방식으로 push하면서 controller를 사용하지않고 present를 사용하고있는데 **굳이 navigationController(rootViewController)를 이동시켜가면서 화면이동을 할 필요가 없다**고 느껴졌습니다.
* 따라서 2차 수정코드처럼 viewController자체를 modal방식으로 이동시키고 navigationContoller는 건들지않은 채 **`navigationBar`를 secondVC에서 생성해주는 방식으로 문제를 해결**했습니다

# 핵심경험

<details>
<summary><big>✅ 기술설정 </big></summary>     
    
|    UI    | 아키텍쳐 |LocalDB | RemoteDB | 의존성 관리도구 | 
|:----:| :----: |:----: |:----: |:----: |
| UIkit | MVVM | Realm -> CoreData | Firebase | SPM |



### 1. UI - UIkit
* UIKit에 대한 이해도나 숙련도가 아직 능숙하지않다고 판단되어, 기본이 될 UIkit을 지속적으로 공부하는게 맞다고 판단했습니다.

### 2. 아키텍쳐 - MVVM
* 기존 프로젝트들이 거의 MVC (Model - View - Contoller) 패턴으로 만들어진 것에 반해, MVVM (Model - View - View Model) 패턴을 한번 기회가 될 때 연습하고 적용해보고 싶었습니다. 또 프로젝트들을 진행하면서 Contoller의 역할이 비대해지는것에 대해 MMVM패턴 관련 피드백을 몇번 들었던 것 같아 공부가 꼭 필요하다고 생각되었습니다.

### 3. LocalDB - Realm (Realm Database)
* Realm이 플랫폼간 연동가능과 사용이 간편하며, 성능이 비교적 우수합니다.

### 변경사항
* Realm이 아직 낯선기술이기도하고, 보편적으로 사용되는 기술은 아닌거같아 좀 더 보편적으로 사용하는 first party인 CoreData에 더 연습이 필요하다고 생각되어 리뷰어와 상의후에 기술설정을 변경했습니다.

|   LocalDB    | 내용 |
|:----:| :---- |
| CoreData | iOS에서 자체제공이라 안정적, 객체 형식으로 저장하고 관리할때 사용하기 좋다. 속도가 빠르다는 장점 <br> @FetchRequest 라는 프로퍼티 래퍼를 사용해서 편하게 쓸 수 있다.<br>SQLite보다 많은 메모리사용, 저장공간도 많이필요 <br>다른 OS와 공유x
| SQLite | 오픈 소스 기반 DB, 모바일 로컬에서 많이 사용되며, 별도 설치X <br> 안드로이드와 iOS 양쪽에서 사용할 수 있어서 공유가능 <br> 가장 많이 사용되기 때문에, 레퍼런스가 많다. <br> 매우 작고 가볍다. | 
| Realm | 최근들어 사용빈도가 올라가는 DB, 안드로이드에서도 사용한다. <br> 다른 DB들보다 성능이 우수 observe하여 update를 체크하여 view를 갱신하는 reactive functionality 제공 <br> Realm Studio가 있어서 DB 자체를 쉽게 확인 가능 | 



### 4. Remote DB - firebase

* 아직 remote DB를 경험해본적이없어서 설명만으로는 크게와닿지 않으나 firebase가 더 보편적으로 사용되고 자료량도 많은거같아 사용성, 접근성면에서 더 우수하다고 생각되어 firebase를 선택하기로 했습니다.
* 또 구글기반의 플랫폼이라 지속가능성에 대해서도 신뢰가 간다고 생각됩니다.

</details>

<details>
<summary><big>✅ modalPresentationStyle - formSheet </big></summary> 

### :fire: formsheet
* 예시화면과 같은 스타일로 VC를 modal present하기위해서는 옵션 modalpresentationStyle을 formsheet로 값을 주는것이 필요했습니다.
* default값은 pageSheet방식 -> 화면을 더 넓게 가림
<img src = "https://hackmd.io/_uploads/BJZOQLXBn.png" width = 400>
</details>

<br/>
    
# 참고 링크
## 블로그
- [NavigationBar modal VC에서 자체생성하기](https://velog.io/@sun02/iOS-UINavigationBar-%EC%BD%94%EB%93%9C%EB%A1%9C-%EC%9E%91%EC%84%B1%ED%95%98%EA%B8%B0)
- [DatePicker](https://kasroid.github.io/posts/ios/20201030-uikit-date-picker/)
            
## 공식 문서
- [AppleDevelopment - Dateformatter](https://developer.apple.com/documentation/foundation/dateformatter)
- [AppleDevelopment - UITextview](https://developer.apple.com/documentation/uikit/uitextview)
- [AppleDevelopment - UIStackView](https://developer.apple.com/documentation/uikit/uistackview)
- [AppleDevelopment - Locale](https://developer.apple.com/documentation/foundation/locale)
- [AppleDevelopment - CoreData](https://developer.apple.com/documentation/coredata)
- [AppleDevelopment - DatePicker](https://developer.apple.com/documentation/swiftui/datepicker)


