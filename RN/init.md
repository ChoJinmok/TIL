# React Native 처음 설치시 있는 요소들 정리

<br />

## 1. SafeAreaView (iOS Component)

- `SafeAreaView`는 디바이스의 안전 영역 안에 콘텐츠를 렌더링하는 역할을 한다. (iOS 11 버전 이상에만 적용)

- `SafeAreaView`는 네비게이션, 탭, 툴바 등과 같은 요소들이 안전한 영역에 배치될 수 있게 자동으로 패딩을 적용하여 중첩된 콘텐츠를 렌더링한다. `SafeAreaView`의 패딩은 둥근 모서리나 카메라 노치와 같은 화면의 물리적 제한도 반영한다.

- 사용할때 `flex: 1` 스타일을 적용한 `SafeAreaView`로 최상위 view를 감싸준다. 앱 디자인과 일치하는 배경색을 적용하는데 용의하다.

구성 요소의 동작을 구현하기 위해 패딩을 사용하므로 SafeAreaView에 적용된 스타일의 패딩 규칙은 무시되며 플랫폼에 따라 다른 결과를 초래할 수 있습니다. 자세한 내용은 #22211을 참조하십시오.

> 패딩은 이미 기본 동작을 구현하는 데 사용되어 있어 `SafeAreaView`에 직접 적용한 패딩은 무시된다. ([#22211](https://github.com/facebook/react-native/issues/22211))
