# Why Apollo Client? - 데이터를 관리할 때 Apollo Client를 사용해야하는 이유가 뭘까요

데이터 관리가 어려울 필요는 없습니다! 만약 React 앱에서 remote, local data 관리하는 것을 쉽게하고 싶다면, 잘 찾아오셨습니다 : ) 

[example app Pupstagram](https://codesandbox.io/s/r5qp83z0yq) 에 영향을 받은 실용적인 예시들을 통해서, Apollo의 똑똑한 캐싱과, 데이터 fetching에 대한 선언적 접근 방법을 배워볼 것입니다. 



*declarative programming* : 선언형 프로그래밍이라는 의미, 선언적 접근 방법도 이러한 프로그래밍 방식을 의미하는 것 같음



### Declarative data fetching

데이터 fetching에 대한 Apollo의 declarative 접근법을 통해 

1. 데이터 검색을 위한 모든 로직
2. 로딩, 에러 상태 추적
3. UI 업데이트

이 세 가지가 기능이 한 개의 Query 컴포넌트안에 encapsulated 됩니다. encapsulation은 Query 컴포넌트와 view 컴포넌트를 만들기 쉽게 해줍니다. 아래 예시를 보자

```javascript
const Feed = () => (
  <Query query={GET_DOGS}>
    {({ loading, error, data }) => {
      if (error) return <Error />
      if (loading || !data) return <Fetching />;

      return <DogList dogs={data.dogs} />
    }}
  </Query>
)

```

위 코드에서는 Query 컴포넌트로 GraphQL 서버에 있는 dogs 데이터를 가져온 뒤 ,dogs를 리스트로 보여줄 것입니다. Query 컴포넌트는 React render prop API를 사용해서 query를 컴포넌트와 bind한 뒤, query 결과에 기반하여 렌더링한다. 데이터를 받았을 때 `<DogList />` 컴포넌트는 필요하다면 바로 업데이트할 것입니다.



Apollo Client는 request 사이클의 시작과 끝을 관리합니다. 로딩과 에러 states를 트래킹하는 것까지 포함해서 말이죠. 첫 request 전에 설정해야하는 미들웨어나, 작성해야하는 보일러플레이트가 없습니다. 또한 response를 변환하거나, 캐싱에 대해 생각할 필요도 없습니다. 우리가 해야할 일은 컴포넌트의 필요성에 따라 데이터를 describe하는 것과, 팝콘을 먹는 것입니다.



Apollo Client를 도입할 때 데이터 관리와 관련된 코드들을 삭제하게 될 것입니다. 삭제할 코드 양은 어떤 app인지에 달라집니다. 어떤 팀에서는 수천 줄까지 줄였다고 들었습니다. 또한 코드를 줄였다고 해서 기능이 줄어드는 것도 아닙니다. optimistic UI, refetching, pagination은 query 컴포넌트 props에서도 쉽게 접근이 가능합니다. 



















