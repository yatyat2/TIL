# 리액트는 어떤 단위로 re-rendering 할까?

###  React only update What's necessary

React DOM은 엘리먼트와 엘리먼트의 자식들을 이전의 것들과 비교합니다. 그리고 변하길 바라는 DOM 부분만 업데이트합니다.

즉, 엘리먼트 단위에서 바뀐 값만 수정이 됩니다. 

