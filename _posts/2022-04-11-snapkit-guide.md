---
layout: single
title: "How to use SnapKit"
date: "2022-04-11"
last_modified_at: "2022-04-24"
---

<br>
# SnapKit 간단 소개

> SnapKit is a DSL(Domain Specific Language) to make Auto Layout easy on both iOS and OS X.

<br>
<br>
SnapKit은 가벼운 DSL로 Auto Layout과 constraint를 쉽게 다룰 수 있게 해준다. Auto Layout은 서로 다른 뷰나 복잡한 뷰 hierarchy에서 뷰 간 관계나 constraint를 설정하는 파워풀한 도구지만, 이런 코드를 작성하는 것은 보통 처음엔 직관적이지 않을 수 있다.  
<br>

몇년 전까지만 해도, 이런 constraint를 코드로 작성하는 것은 Visual Formatting Language를 이용하거나 수동으로 'NSLayoutConstraint'를 사용해야 하는 상당히 헷갈리고 지루하고 장황한 일이었다. iOS9에서 이 메커니즘을 **Layout Anchors** 도입을 통해 크게 개선했다. 하지만 이 보다 좀 더 산뜻하게 constraints를 주기 위해 SnapKit 과 같은 도구가 도입됐다.<br>
<br>
# SnapKit 기본

## DSL이란?

A domain-specific language(DSL)은 특정 도메인에 특화된 비교적 작고 간단한 프로그래밍 언어다.<br>
<br>
SnapKit의 경우에는 Auto Layout Constraints를 더욱 직관적이고 쉽게 쓰기위한 syntax를 목표로 만들어졌다.<br>
<br>
중요한 것은, SnapKit의 대부분은 **syntatic sugar**로서 SnapKit으로 할 수 있는 모든 것을 SnapKit없이도 할 수 있다.<br>
<br>
예를들어 superview의 모든 모서리에 각 하위뷰의 모서리를 붙이는 AutoLayout을 작성한다고 생각해보자.<br>
```swift
child.translatesAutoresizingMaskIntoConstraints = false

NSLayoutConstraint.activate([
  child.leadingAnchor.constraint(equalTo: parent.leadingAnchor),
  child.topAnchor.constraint(equalTo: parent.topAnchor),
  child.trailingAnchor.constraint(equalTo: parent.trailingAnchor),
  child.bottomAnchor.constraint(equalTo: parent.bottomAnchor),
])
```
<br>
상당히 선언적인 구문이지만 SnapKit을 이용하여 개선할 수 있다.<br>
<br>
SnapKit은 모든 `UIView`에(혹은 macOS의 `NSView`) `snp`라는 nameplace를 도입했다. 이 nameplace와 `makeConstraints(_:)` 메서드는 SnapKit의 핵심이다.<br>
<br>
스냅킷을 이용하면 위의 코드는 아래와 같이 나타낼 수 있다.<br>
```swift
child.snp.makeConstraints { make in
  make.leading.equalToSuperview()
  make.top.equalToSuperview()
  make.trailing.equalToSuperview()
  make.bottom.equalToSuperview()
}
```
<br>
비슷한 양의 코드같이 보이지만 아래와 같은 두가지 이점을 찾을 수 있다.<br>
1. `equalToSuperview()`메서드 덕에 `parent`를 참조할 필요가 없다. 이는 `child`가 다른 parent view로 옮겨가더라도 이를 코드를 수정할 필요가 없다는 말을 의미한다.
2. `make` syntax는 거의 자연스러운 영어 문장으로 읽힌다.
<br>
 
## 결합성(Composability) & 체이닝(Chaining)

SnapKit의 진가는 결합 가능성에서 나온다. 결합 가능성이 무엇이냐하면, anchor나 constraint를 체이닝 하는 것이다.<br>
<br>
위 예시의 코드를 아래와 같이 적을 수 있다.<br>
<br>
```swift
child.snp.makeConstraints { make in
  make.leading.top.trailing.bottom.equalToSuperview()
}
```
<br>
아니면 좀더 간결하게 아래와 같이 적을 수 있다.<br>
```swift
child.snp.makeConstraints { make in
  make.edges.equalToSuperview()
}
```
<br>
16만큼의 inset을 주고 싶으면 아래와 같이 적을 수 있다.<br>
```swift
child.snp.makeConstraints { make in
  make.edges.equalToSuperview().inset(16)
}
```
<br>
이렇게 SnapKit을 이용해 결합과 체이닝을 사용해보자.<br>
<br>

## Updating Constraints

Constraints에 constant만을 업데이트하고 싶을 때, SnapKit은 `updateConsraints(_:)`라는 유용한 메서드를 제공한다.
```swift
// MARK: - Orientation Transition Handling
extension QuizViewController {
  override func willTransition(
    to newCollection: UITraitCollection,
    with coordinator: UIViewControllerTransitionCoordinator
    ) {
    super.willTransition(to: newCollection, with: coordinator)
    // 1
    let isPortrait = UIDevice.current.orientation.isPortrait
    
    // 2
    lblTimer.snp.updateConstraints { make in
      make.height.equalTo(isPortrait ? 45 : 65)
    }
    
    // 3
    lblTimer.font = UIFont.systemFont(ofSize: isPortrait ? 20 : 32, weight: .light)
  }
}
```
<br>

## Remaking Constraints

만약 단순히 몇개의 Constant만을 바꾸는 것이 아닌, constraint 전체를 바꾸고 싶다면, SnapKit은 `remakeConstraints(_:)`라는 유용한 메서드를 제공한다. 

```swift
  viewProgress.snp.remakeConstraints { make in
    make.top.equalTo(view.safeAreaLayoutGuide)
    make.width.equalToSuperview().multipliedBy(progress)
    make.height.equalTo(32)
    make.leading.equalToSuperview()
    }
```
<br>

## Keeping Reference

기본 `NSLayoutConstraint`에서도 constraint를 저장해서 후에 수정하는 것이 가능하다. 이것은 SnapKit에서 또한 가능한데, `Constraint`타입을 사용하면 된다.

```swift
var topConstraint: Constraint?

lblTimer.snp.makeConstraints { make in 
  // constraint를 저장
  self.topConstraint = make.top.equalToSuperview().inset(16)
  make.leading.trailing.bottom.equalToSuperView()
}

// 이후에 이렇게 수정할 수 있다.
self.topConstraint?.update(inset: 32)

// 아니라면 비활성화 할 수도 있다.
self.topConstraint?.deactivate()
```
<br>

## SnapKit을 이용한 AutoLayout 디버깅

Auto Layout을 사용하다 Constraint들 사이에 모순이 발생하면 엄청난 양의 로그가 콘솔에 찍히는데 이런 로그는 직관적으로 이해하기 힘들다. 이를 해결하기 위해 SnapKit은 `labeled(_:)`라는 추가적인 modifier를 제공한다.

```swift
lblTimer.snp.makeConstraints { make in
  make.width.equalToSuperview().multipliedBy(0.45).labeled("timerWidth")
  make.height.equalTo(45).labeled("timerHeight")
  make.top.equalTo(viewProgress.snp.bottom).offset(32).labeled("timerTop")
  make.centerX.equalToSuperview().labeled("timerCenterX")
  make.centerY.equalToSuperview().labeled("timerCenterY")
}

```
<br>
각 constraint 뒤의 `labeled(_:)`가 해당 constraint를 설명하는 title이 되어준다. 이렇게하면 로그 창의 메모리 주소를 보며 스트레스를 받을일이 현저히 줄어든다. 


# Reference

- [SnapKit for iOS: Constraints in a Snap](https://www.raywenderlich.com/3225401-snapkit-for-ios-constraints-in-a-snap)
- [SnapKit Demonstration Sample Project](https://github.com/jung-yun/snapkit-demo-swift)
