# Next.js

Next.js는 서버 사이드 렌더링 프레임워크이다.

React.js를 핵심으로 하는 Next.js는 처음 페이지를 요청할 땐 SSR방식을 사용하고, 내부에서 페이지를 이동할 때는 CSR방식을 사용한다.



### next.js의 Link 컴포넌트 vs reactStrap Button 컴포넌트

Link 컴포넌트는 클라이언트 사이드 이동(렌더링)을 한다.

Button 컴포넌트는 서버 사이드 이동(렌더링)을 한다.



getInitialProps할 때 인자로 X에 대해 콘솔을 찍어보면

서버에서는 모~~~든 인자들이 보여지는데, 클라이언트에서는 5개 정도만 보인다. 

무슨 의미가 있는 건지는 모르겠음...

```javascript
  public static async getInitialProps(test: any) {
    const isServer = !!test.req;
    const store = initStore(isServer);

    store.initPage(isServer, test.req);
    store.checkPartner(isServer, test.res);

    console.log(test);

    return { initialState: getSnapshot(store), isServer };
  }

```

