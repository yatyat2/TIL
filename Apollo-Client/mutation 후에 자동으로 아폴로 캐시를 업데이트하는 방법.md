# mutation 후에 Apolo client의 캐시를 업데이트하는 방법

<https://medium.freecodecamp.org/how-to-update-the-apollo-clients-cache-after-a-mutation-79a0df79b840>

Apollo Client는 GraphQL 서버로 부터 데이터를 가져올 대 사용됩니다. Apollo Client는 작고, 유연하고, 많은 놀라운 장점을 가지고 있습니다. 그 중에도 가장 고평가 되는 장점은 아마도 캐시를 자동으로 업데이트하는 것입니다.



 기본적으로 Apollo Client는 자동으로 query와 mutation의 트래픽을 살펴봅니다. (그리고 무튼 뭐 도움이 되게 해준다고 한다.)



query로 데이터를 가져오고, mutation으로 데이터를 가져오면 자동으로 캐시가 업데이트 됩니다. 그 이유는 두 가지가 있다.

1. mutation response에 수정하는 데이터의 id를 포함하고 있다.
2. 위의 resoponse에는 title이 있다. (title은 수정된 값)

실제로 두 필드의 id 값이 일치한다면 title 필드는 우리 UI의 모든 곳에서 자동으로 업데이트가 일어난다.



기본적으로 mutation의 결과 값은 모두 가져오는 것이 좋다. 이 전에 가져온 데이터들을 업데이트 하기위해서 필수적이기 때문이다. 



That’s also why is a best practice to use [fragments](https://www.apollographql.com/docs/react/advanced/fragments.html) to share fields among all queries and mutations that are related.



그러나 실제로는 자동으로 업데이트가 되지 않는 경우가 있다. 예를 들면

- A creation
- A deletion
- filtered lists of A

등등...



무튼 위의 문제를 해결하기 위한 두 가지 방법이 있다.

- mutaiton 후에 브라우저를 새로고침해라 :D
- `update` 함수를 사용해서 로컬 캐시에 직접적으로 접근하다.  `update` 함수는 데이터를 다시 가져오지 않아도 mutation 후에 수동으로 업데이트하도록 도와줍니다.



** considering you’re using the `cache-first `default [fetchPolicy](https://www.apollographql.com/docs/react/api/react-apollo.html#graphql-config-options-fetchPolicy)

(뭐라는거야?)



refetchQueries는 세 번째 옵션이다. `update` 는 query 후에 캐시를 업데이트하는 방법 중에 Apollo가 추천하는  방법이다. 자세한 설명은 여기서 보아라~  [here](https://www.apollographql.com/docs/react/api/react-apollo.html#graphql-mutation-options-update).

