# 아폴로 클라이언트

Mutation 컴포넌트로 어떻게 데이터를 업데이트하는지 알아보자

아폴로 클라이언트를 사용한다면 graph API로 데이터를 업데이트하는 일은 함수를 호출하는 것처럼 쉽습니다.

부가적으로 아폴로 클라이언트 캐시는 대부분의 케이스를 자동으로 업데이트할만큼 똑똑합니다.



### 뮤테이션 컴포넌트가 뭘까?

Mutation 컴포넌트는 아폴로 앱에서 또다른 중요한 블록입니다. (다른 하나는 Query 컴포넌트)

이 리액트 컴포넌트는 GraphQL Mutation을 실행하는 함수를 제공한다. 부가적으로, 로딩, 완료, 그리고 mutation의 에러 상태를 추적합니다.



react-apollo에서 Mutaiton 컴포넌트로 데이터를 업데이트하는 것은 Query 컴포넌트로 데이터를 가져오는 것과 매우 흡사하다. 가장 큰 차이는 Mutation render prop function의 첫 번째 인자가 mutate function이라는 점이다. mutation function이 호출될 때마다 실제로 변화를 유발한다. Mutation render prop function의 두 번째 인자는 mutation으로 생긴로딩, 에러 상태, 반환 값을 포함하고 있는 결과 객체다. 예시를 보자



### Mutation으로 데이터 업데이트하기

첫 번째 스탭은 GraphQL Mutation을 정의하는 것입니다. XX 페이지로 이동하고, 로그인 화면을 만들기 위한 코드를 복사해보세요

```javascript
import React from 'react';
import { Mutation, ApolloConsumer } from 'react-apollo';
import gql from 'graphql-tag';

import { LoginForm, Loading } from '../components';

const LOGIN_USER = gql`
  mutation login($email: String!) {
    login(email: $email)
  }
`;

```

예전과 동일하게, 우리는 `gql` 함수로 GraphQL mutation을 감싸줘서 AST로 변환한다. 



*abstract syntax tree = AST = 추상화된 문법 트리: 그래프큐엘 서버는 쿼리를 받고, 일반적으로 이 쿼리를 string으로 처리한다. 이 스트링은 토큰화되고 나누어져서 기계가 이해할 수 있도록 표현된다. 이 표현을 AST라고 한다.*



```javascript
export default function Login() {
  return (
    <Mutation mutation={LOGIN_USER}>
      {(login, { data }) => <LoginForm login={login} />}
    </Mutation>
  );
}

```

우리 Mutation 컴포넌트는 render prop function를 login같은 mutate 함수 그리고 데이터 오브젝트를 노출시키는 자식으로 취급한다.  — 마지막으로 우리는 login 함수를 LoginForm 컴포넌트로 넘긴다.



유저들이 더 나은 경험을 하려면 세션간 로그인을 유지하는 것이 좋다. 그렇게 하기 위해선 로그인 토큰을 `localStorage` 에 저장해야한다.  `Mutation` 에 있는 `onCompleted` 핸들러를 어떻게 사용해야 하는지 공부해서 로그인을 유지해보자



















