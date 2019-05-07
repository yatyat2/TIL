# 아폴로 클라이언트 - Local state manegement

 어떻게 아폴로 클라이언트에서 로컬 데이터가 작동하는지 알아보자



우리는 apollo client를 사용해서 GraphQL 서버로부터 리모트 데이터를 관리하는 방법을 배우고 있다. 그렇다며 로컬 데이터는 어떻게 관리해야 할까요? Apollo 캐시가 클라이언트 앱에 믿을만한 단일 정보가 되도록 하고싶어합니다.



2.5 버전 이상의 Apollo Client는 local state를 다루는 기능을 내재해서, 리모트 데이터와 함께 로컬 데이터를 Apollo 캐시에 저장할 수 있게 해줍니다. 로컬 데이터에 접근하려면 GraphQL로 질의해라. 로컬, 서버 데이터 모두 같은 쿼리로 요청할 수 있다.



이 섹션에서는 아폴로 클라이언트로 앱에서 local state 관리하는 방법을 배울거다. 클라이언트 사이드 리졸버가 어떻게 우리가 로컬 쿼리와 뮤테이션을 실행하도록 돕는지 살펴볼 것이다. 또한 @client를 사용해서 캐시에 어떻게 질의하고 업데이트하는지 알아보자.



### local state 업데이트하기

local state 변화시키기 위한 방법은 크게 두 가지가 있다. 

첫 번째 방법은 `cache.writeData` 를 호출해서 직접적으로 캐시에 쓰는 방법이 있다. 직접쓰기가 좋은 경우는 (일반적으로 캐시 안에 있는)데이터에 의존하지 않는 일회성 변화의 경우와 단일 값을 쓰는 경우이다. 



두 번째 방법은 Mutation 컴포넌트를  (로컬 클라이언트 리졸버를 호출하는)GraphQL mutation과 함께 생성하는 것이다.



만약 리스트에 아이템을 추가하거나, boolean을 토글하는 경우엔 캐시 안에 있는 값에 의존한 mutation이기 때문에 리졸버를 사용하는 것을 추천하다. 



### 로컬 리졸버

GraphQL mutation으로 local state를 업데이트를 실행하고 싶다면  local resolver map에 함수를 선언해야 한다.

resolver map은 각 GraphQL object type을 위한 resolver 기능들을 가진 객체이다. 어떤 기능들이 있는지 보려면, GraphQL의 query 혹은 mutation을 각 필드에 대한 함수 호출들의 트리로 생각해보면 유용하다.  이런 함수 호출들은 데이터나 다른 함수 호출을 resolve합니다. GraphQL의 query가 Apollo Client를 통해서 실행될 때 query의 각 필드에서 필수적으로 실행되는 함수들을 찾습니다. When it finds an `@client` directive on a field, it turns to its internal resolver map looking for a function it can run for that field









