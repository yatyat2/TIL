# Caching data - 아폴로 캐시를 커스마이징하고 직접적으로 접근하는 방법

### InMemoryCache

`apollo-cache-inmemory` 는 Apollo Client 2.0을 실행하기 위한 디폴트 캐시입니다.

`InMemoryCache` 는 Redux에 대한 의존 없이 Apollo Client 1.0의 모든 특징을 지원하는 표준화된 데이터 store 입니다.



때때로 mutation 후에 store를 업데이트하는 것처럼 캐시를 직접적으로 다뤄야할 때가 있을 겁니다. 그런 케이스들을 다뤄볼겠습니다. => [here](https://www.apollographql.com/docs/react/advanced/caching#recipes)



### Installation

```javascript
npm install apollo-cache-inmemory --save

```

패키지를 설치한 후에 캐시 생성자를 초기화 한 뒤 새롭게 생성된 캐시를 ApolloClient에 넘겨줄 수 있습니다.

```javascript
import { InMemoryCache } from 'apollo-cache-inmemory';
import { HttpLink } from 'apollo-link-http';
import { ApolloClient } from 'apollo-client';

const cache = new InMemoryCache();

const client = new ApolloClient({
  link: new HttpLink(),
  cache
});

```



### Configuration

위의 `InMemoryCache` 생성자는 옵셔널한 config 오브젝트를 통해서 캐시를 커스마이즈할 수 있습니다.

- `addTypename` : document에 __typename을 추가할 것인지 아닌지를 결정하는 boolean 값입니다. 

- `dataIdFromObject` : 데이터 객체를 가지며, store 안에 표준화된 데이터에 사용되는 유니크한 identifier를 반환하는 합수입니다. 자세하게 알고싶다면 => [Normalization](https://www.apollographql.com/docs/react/advanced/caching#normalization)

- `fragmentMatche` : 기존적으로 `InMemoryCache` 는 heuristic fragment matcher를 사용합니다. 만약 만약 유니온 그리고 인터페이스에 대한 fragments를 사용한다면 `IntrospectionFragmentMatcher` 을 사용해야 합니다. 자세하게 알고싶다면 => [our guide to setting up fragment matching for unions & interfaces](https://www.apollographql.com/docs/react/advanced/fragments.html#fragment-matcher).

- `cacheRedirects(전에는 cacheResolvers or customResolvers으로 알려져있습니다.)` : 쿼리를 다른 entry로 redierct하기 위한 map of functions 입니다. 

  해석 불가 ㅠ… (A map of functions to redirect a query to another entry in the cache before a request takes place. This is useful if you have a list of items and want to use the data from the list query on a detail page where you're querying an individual item. More on that [here](https://www.apollographql.com/docs/react/advanced/caching.html#cacheRedirect).)



### Normalization

`InMemoryCache` data를 store에 저장하기 전에 데이터를 표준화 합니다. 

1. 결과를 각각의 오브젝트로 나누고
2. 각 오브젝트에 유니크한 identifier을 생성해주고
3. 그런 오브젝트들을 flattened 데이터 구조로 저장합니다.

기본적으로 `InMemoryCache` 는 공통적으로 보여지는 primary keys인  `id` 와 `_id` 를 사용해서 유니크한 identifier를 

뭐징

`InMemoryCache` will attempt to use the commonly found primary keys of `id` and `_id`for the unique identifier if they exist along with `__typename` on an object.



















