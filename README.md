# projectManager README

# 프로젝트 매니저 :notebook: 
> 프로젝트를 todoList, doingList, doneList 세개의 테이블 섹션으로 구분지어 관리하는 앱
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

### week2

|    날짜    | 내용 |
|:----:| ---- |
| 2023.05.22 | ToDoWriteVC create(작성), edit(편집) 모드 분기처리 |
| 2023.05.23 | longPressGestureRecognizer 생성으로 popover 뜨도록 구현 |
| 2023.05.24 | popover선택시 cell을 선택 tableView로 이동하게끔 구현, swipeAction 구현 |
| 2023.05.25 | updateCircleView 매서드로 headerView의 cirecleView lable count 변경구현|
| 2023.05.26 | README 작성 |

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
│   │   └── ToDoTableViewCell
│   ├── ViewController
│   │   ├── ToDoListViewContorller
│   │   └── ToDoWriteViewController
│   ├── Assets
│   ├── Main
│   ├── Info
└─  └── GoogleService-Info
```

 <br/>

# 실행 화면


|1. 저장|2. 편집|
| :---: | :---: |
| <img src = "https://github.com/Goatt8/ios-project-manager/assets/62684851/d3dad4a3-01f2-4120-b1dd-b7154f465215" width= 400> |<img src = "https://github.com/Goatt8/ios-project-manager/assets/62684851/5874c525-b0bb-4572-b0ae-ca4f40a665ea" width= 400>|

|3. 이동|4. 삭제|
| :---: | :---: |
| <img src = "https://github.com/Goatt8/ios-project-manager/assets/62684851/afc02f15-6b3c-4193-a95b-6cfaaa9a3376" width= 400> |<img src = "https://github.com/Goatt8/ios-project-manager/assets/62684851/b41622a3-1994-4769-9a94-b84fbb671645" width= 400>|





|    화면    | <center>설명</center>|
|:----:| ---- |
| 1 | 우측 상단 +버튼으로 modalVC에 내용 전달하고 done버튼 클릭시 TODOTableView에 데이터 cell로 저장 |
| 2 | cell 짧게 클릭시 modalVC가 edit모드로 생성되고 내용편집후 edit클릭시 내용변경저장|
| 3 | cell 길게 클릭시 moveToTableView popover가 생성되며, 선택시 원하는 List로 cell 이동 |
| 4 | swipe delete시 선택 cell 삭제|


<br/>


# 트러블 슈팅

### :fire: 1. modalVC에 navigationBar 띄워지지않는 에러

* 위처럼 모달로 띄운 뷰컨트롤러에 navigationBar가 띄워지지않는 에러발생

| 수정 전 | 수정 후 |
|:----:| :----: |
| <img src = "https://github.com/yagom-academy/ios-project-manager/assets/62684851/f997d45f-07c0-4c15-bd8f-1bd6ca98f96d" width = 400> | <img src = "https://github.com/yagom-academy/ios-project-manager/assets/62684851/8a6c3de0-d797-4910-94e1-53b654e85b25" width = 400> | 

### 수정전
```swift
@objc private func plusButtonTapped() {
       let toDoWriteViewController = UINavigationController(rootViewController: ToDoWriteViewController())
        toDoWriteViewController.modalPresentationStyle = .formSheet
        toDoWriteViewController.delegate = self
        self.present(toDoWriteViewController, animated: true)
}
```

### 수정후
```swift
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

<br/>

### :fire: 2. popover에서 선택한 cell을 이동시킬때 cell의 indexPath를 구해야하는 문제
#### - feat.recognizer.View.indexPathForRow(at: touchPoint)
<img src = "https://github.com/yagom-academy/ios-project-manager/assets/62684851/fe9e7025-0fa9-476c-82a2-73ad9ff27b5a" width = 400>

* popvover에서 원하는`moveToList -> moveToList` 를 할때 각 3개의 tableView마다 경우의 수가 2개씩 나오기때문에 선택한 tableView의 indexPath 값을 구해서 cell을 제거하고 이동시키고싶은 tableView에 해당 cell을 업데이트하는 과정에서 해당 Indexpath를 구하는 부분에서 한참 에러를 해결하지 못했던거같습니다.

* `longTouchPressed()`매서드에서 테이블뷰를 분기처리해서 `showPopover()` popover를 띄워주고 handler를 이용해서 `convertCellInTableView()` 매서드로 todoList, doingList, doneList 세개의 배열에 값을 삭제하고 추가해주는 과정의 로직을 구현했습니다.

* indexPath는 상위매서드인 `longTouchPressed()`에서 `recognizer.view`를 타고 선택된 테이블뷰의 indexPathForRow(at:)매서드를 통해 구하는 방식을 선택해서 하위 매서드인 `convertCellInTableView()`의 매개변수에 전달해주는 방식으로 로직을 구현했습니다.

* convertCellInTableView(indextPath: IndexPath, firstChoiceTableView: TableViewCategory, targetTableView: TableViewCategory) 매서드에서는 받은 indexPath값으로 선택된 배열의 아이템을 삭제하고 추가하고싶은 list 배열에 append하는 역할을 허도록 구현했습니다.

```swift
@objc private func longTouchPressed(_ recognizer: UILongPressGestureRecognizer) {
        guard let selectedTableView = recognizer.view as? UITableView else { return }
        let touchPoint = recognizer.location(in: selectedTableView)
        guard let selectedIndexPathRow = selectedTableView.indexPathForRow(at: touchPoint) else { return }
    
    if recognizer.state == .began {
            switch selectedTableView {
            case toDoTableView:
                showPopover(firstTitle: "MOVE TO DOING",
                            secondTitle: "MOVE TO DONE",
                            firstHandler: { _ in self.convertCellInTableView(indextPath: selectedIndexPathRow,
                                                                             firstChoiceTableView: .todoTableView,
                                                                             targetTableView: .doingTableView) },
                            secondHandler: { _ in self.convertCellInTableView(indextPath: selectedIndexPathRow,
                                                                              firstChoiceTableView: .todoTableView,
                                                                              targetTableView: .doneTableView) })
             // case doingTableView
             // case doneTableView
```

<br/>

### :fire: 3. ipad에서 actionSheet사용시 발생하는 에러 -> popoverPresentationController로 변경

<img src = "https://github.com/yagom-academy/ios-project-manager/assets/62684851/8feb26c6-1098-4b42-88d0-fb72b864188e" width = 400>

#### 1차 문제점 - actionSheet 사용불가
* 요구사항에 popover는 actionSheet로 대체해도 된다는 텍스트가 있어 마우스 longTouchPress시 actionSheet로 cell을 이동시키는 버튼이 뜨도록 구현하려했으나 ipad모드에서는 actionSheet를 사용하면 위와같은 에러가 발생했습니다.
* 아무래도 가로 비율이 넓은 ipad인터페이스라 하단부를 꽉채우는 actionSheet 스타일이 부적절하다는 경고문인것 같았습니다.

```swift
let alertController = UIAlertController(title: "이동하고싶은 ",
                                        message: "",
                                        preferredStyle: .actionSheet)
guard let popoverController = alertController.popoverPresentationController 
    else { return }
```
* 따라서 위처럼 alertController스타일을 popoverPresentationContoller로 변경해 처리했습니다

#### 2차 문제점 - popover present의 위치값 구하는 문제
* 요구사항에는 선택한 cell에 화살표 포인터를 두고 기점으로 popover가 나타나는 스타일이지만

| 요구사항 | 1차 구현 |
|:----:| :----: |
| <img src = "https://github.com/yagom-academy/ios-project-manager/assets/62684851/bcf5fda9-7d22-45e8-969d-6c3d767cc681" width = 400> | <img src = "https://github.com/yagom-academy/ios-project-manager/assets/62684851/5ebc241f-f4f3-4f40-a645-ce83c2b56f86" width = 400> | 

* popover 위치 마우스포인터로 뜨게끔 해결하지못해서, view의 센터에 뜨도록 구현했었으나, `popoverPresentationController.sourceRect` 에 들어가야할 포인터위치 CGRect에 상위매서드인 `longTouchPressed`의 longTouchPoint, CGPoint값을 매개변수(point)로 넣어줘서 해결했습니다.
```swift
func longTouchPressed(_ recognizer: UILongPressGestureRecognizer) {
 let touchPoint = recognizer.location(in: selectedTableView)
 
 showPopover(point: touchPoint)
}



 private func showPopover(point: CGPoint, 
                          firstTitle: String,
                          secondTitle: String,
                          firstHandler:((UIAlertAction) -> Void)?,
                          secondHandler:((UIAlertAction) -> Void)?)

 popoverController.sourceRect = CGRect(x: point.x + 50,
                                       y: point.y + 50,
                                       width: 0,
                                       height: 0)
```


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


<details>
<summary><big>✅ 셀 데이터 업데이트, firstIndex로 값 비교 </big></summary> 

    
- [firstIndex 참고블로그](https://min-i0212.tistory.com/17)


```swift
func sendTodoList(data: ToDoList, isCreatMode: Bool) {
        if isCreatMode == true {
            toDoList?.append(data)
        } else {
            if let index = toDoList?.firstIndex(where: { $0.title == data.title}) {
                toDoList?[index] = data
            }
        }
```

* create 모드, edit 모드를 나눌 수 있는 `isCreateMode: Bool` 매개변수로 데이터를 받아올 때 분기처리하고, 받아온 toDoList data를
    *  create모드일때는 전역변수 [toDoList]에 append,
    *  edit모드일때는 [toDoList]의 firstIndex로 값을 필터링해 매칭되는 배열, cell 만 데이터를 변경해주도록 처리했습니다.
</details>


<br/>
    
# 참고 링크
## 블로그
- [NavigationBar modal VC에서 자체생성하기](https://velog.io/@sun02/iOS-UINavigationBar-%EC%BD%94%EB%93%9C%EB%A1%9C-%EC%9E%91%EC%84%B1%ED%95%98%EA%B8%B0)
- [DatePicker](https://kasroid.github.io/posts/ios/20201030-uikit-date-picker/)
- [firstIndex 참고블로그](https://min-i0212.tistory.com/17)
            
## 공식 문서
- [AppleDevelopment - Dateformatter](https://developer.apple.com/documentation/foundation/dateformatter)
- [AppleDevelopment - UITextview](https://developer.apple.com/documentation/uikit/uitextview)
- [AppleDevelopment - UIStackView](https://developer.apple.com/documentation/uikit/uistackview)
- [AppleDevelopment - Locale](https://developer.apple.com/documentation/foundation/locale)
- [AppleDevelopment - CoreData](https://developer.apple.com/documentation/coredata)
- [AppleDevelopment - DatePicker](https://developer.apple.com/documentation/swiftui/datepicker)
- [AppleDevelopment - longPressGesture](https://developer.apple.com/documentation/swiftui/longpressgesture)
- [AppleDevelopment - firstIndex(of:)](https://developer.apple.com/documentation/swift/array/firstindex(of:))
- [AppleDevelopment - popvoer](https://developer.apple.com/documentation/uikit/windows_and_screens/displaying_transient_content_in_a_popover)
