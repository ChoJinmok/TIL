# 노마드코더's 왕초보를 위한 React Native 101

> 노마드코더에서 [왕초보를 위한 React Native 101](https://nomadcoders.co/react-native-for-beginners/lobby) 강의를 듣고 필요한 부분을 정리

## Layout 시스템

- View의 스타일 기본 설정값
  - `display: flex;`
  - `flexDirection: column`
- View의 width, height 설정
  - 숫자로 static하게 넣는 경우는 잘 없음. (스크린 사이즈에 따라 다르게 보일 수 밖에 없어짐. 아이콘, 아바타 등의 경우에는 예외)
  - 반응형을 고려하며(다양한 스크린에서 동일하게 보이게) 작업하려면 `flex` 스타일 속성으로 비율로 너비와 높이를 정해준다.
