# Why Apollo Client? - 데이터를 관리할 때 Apollo Client를 사용해야하는 이유가 뭘까요

데이터 관리가 어려울 필요는 없습니다! 만약 React 앱에서 remote, local data 관리하는 것을 쉽게하고 싶다면, 잘 찾아오셨습니다 : ) 

[example app Pupstagram](https://codesandbox.io/s/r5qp83z0yq) 에 영향을 받은 실용적인 예시들을 통해서, Apollo의 똑똑한 캐싱과, 데이터 fetching에 대한 선언적 접근 방법을 배워볼 것입니다. 



*declarative programming* : 선언형 프로그래밍이라는 의미, 선언적 접근 방법도 이러한 프로그래밍 방식을 의미하는 것 같음



### Declarative data fetching

데이터 fetching에 대한 Apollo의 declarative 접근을 사용하면 

1. 데이터 검색을 위한 모든 로직
2. 로딩, 에러 상태 추적
3. UI 업데이트

이 세 가지가 한 개의 Query 컴포넌트-