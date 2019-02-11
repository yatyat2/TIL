# 리액트 최적화

https://medium.com/@joomiguelcunha/react-performance-tips-5fa199a450b2 번역..인데 틀릴 확률 높높

### Wasted renders

크고 복잡한 앱에서 state는 수시로 변경됩니다. 그리고 여러 번 re-render하는 경우가 생깁니다. reconciliation을 피하는 것은 매우 중요한 일입니다. (조정할 필요가 없어야 한다는 의미인 듯)



먼저, wasted render가 무엇일까요?

wasted render란 (1)컴포넌트가 변하지도 않았는데 re-render되는 것과 (2)렌더링 하기까지 낭비되는 시간입니다. 만약 컴포넌트가 바뀌지 않았다면, 렌더링 되는 것을 막아야 합니다.



wasted render를 탐지하기 위해서 react developer tools를 설치하세요.

이 프로그램은 최적화 정도를 파악하는데 큰 도움이 됩니다.



(프로그램 깔고 테스트 하는 과정은 직접 링크에서 보는 게 좋을 듯)



문제가 있는 컴포넌트를 확인해야한다. 어떻게 해야할까

So you’ve tracked down those pesky components that are rendering needlessly and are taking up rendering time? 이런 애들은 shouldComponentUpdate로 처리하자.



> `shouldComponentUpdate()`를 사용해서 컴포넌트의 아웃풋이 현재의 state,props 변화에 영향을 끼치는지 파악하세요. 기본적으로 react는 state가 바뀔 때 마다 re-render합니다. 그리고 대부분의 상황에서 기본으로 둡니다.
>
>  
>
> `shouldComponentUpdate()`는 새로운 props나 state를 받을 때 호출됩니다. 기본적으론 true값 입니다. 이 메서드는 처음 렌더할 때나 `forceUpdate()`가 사용될 때는 호출되지 않습니다.



---









