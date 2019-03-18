# history

#### history란

`history` 는 자바스크립트 가동 환경에서 쉽게 세션 관리를 도와주는 라이브러리이다. `history` 는 다양한 환경들의 다른 점들을 추상화하고 history stack, naviagete, confirm navigation, 세션 간의 state를 유지하는 것을 관리할 수 있게 해주는 최소한의 API를 제공한다. 



#### 설치

npm install —save history



#### 사용법

 ` history` 는 사용자의 환경에 따라 3개의 방법으로 객체를 생성한다. 

- `createBrowserHistory` 는 HTML5 Histoy API를 지원하는 모던 웹 브라우저에서 사용할 수 있다.
- `createMemoryHistoy` 는 *reference implementation*로써 사용된다. 또한 React Native나 tests 같은 non-DOM 환경에서 사용되기도 한다.
- `createHashHistory` 는 레커시 웹 브라우져에서 사용된다.



ow







reference implementation* (참조 구현)

다른 사람들이 어떠한 하드웨어 혹은 소프트웨어를 구현 하는 것을 돕기 위해 제공하는 샘플 프로그램이다. 샘플 구현 또는 모델 구현이라고도 불린다.