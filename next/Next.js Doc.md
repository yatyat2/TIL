# Next.js Docs

설치는

npm install —save next react react-dom



그리고 package.json에 스크립트 추가

```javascript
{
  "scripts": {
    "dev": "next",
    "build": "next build",
    "start": "next start"
  }
}
```



이렇게 하면 그 파일 시스템은 메인 API가 된다. (After that, the file-system is the main API. 여기서 의미하는 파일 시스템, API가 뭘 의미하는 거지…?) 모든 `.js` 파일은 자동으로 처리되고 렌더링 되는 루트가 된다. 



`./pages/index.js` 를 프로젝트에 만드세요



```javascript
function Home(){
    return <div>Welcome to next.js!</div>
}

export default Home
```



그리고 `npm run dev` 입력한 뒤 `localhost:3000` 에 접속해라, 다른 포트를 사용하고 싶으면 `npm run dev -- -p <너가 원하는 포트>` 를 명령해라



이렇게 하면 우리는

1. 자동으로 트랜스파일래이션과 빌딩을 한다. (webpack과 babel을 이용해서)
2. Hot code reloading (코드를 수정했을 때 앱에 반영되는 속도가 빠르다.)
3. 서버 렌더링과 페이지에 이름을 붙일 수 있다.(?)
4. 정적 파일을 제공하는 `./static/` 은 `/static/` 과 매칭된다. 



### Automatic code splitting

당신이 선언한 모든  `import` 는 번들되어서 , 각 페이지로 제공한다.. 페이지가 불필요한 코드를 불러들일 필요가 없다는 것을 의미합니다. 



### CSS

##### Built-in CSS support

우리는 styled-jsx를 번들하면서 고립된 CSS 스코프를 만들 수 있습니다. 웹 컴포넌트와 비슷한 shadow CSS를 제공하기 위해서 입니다. 웹 컴포넌트는 JS-only이며 서버렌더링을 지원하지 못합니다.



```
function HelloWorld(){
    return (
    <div>
    	Hello world
    	<p>scoped!</p>
    	<style jsx>{
        `
        	
        	`
        
            
    	}
    )
}
```



##### CSS-in-JS

기존의 자바스크립트에서 CSS를 쓰는 방식은 사용할 수 있습니다. 가장 간단한 인라인 스타일을 볼까요

```javascript
function HiThere(){
    return <p style={{color:"red"}}hi there</p>
}

export default HiThere
```



자바스크립트에서 CSS를 더 잘 사용하기 위해서는, 일반적으로 서버 사이드 렌더링을 위한 스타일 플러싱을 사용해야한다.(그게 뭐지)  커스텀 `<Document>` 컴포넌트로 각 페이지를 감싸는 방법으로 위의 기능을 구현할 수 있다. 



#### Improting CSS / Sass / Less / Stylus files

CSS / Sass / Less / Stylus 파일을 지원하기 위해서 아래의 모듈을 사용해야 합니다. 이 모듈들은 서버 렌더링 앱을 위한 적절한 기본 설정을 합니다.



Static file seving (e.g.: images)

`static` 이라고 불리는 폴더를 너의 프로젝트 루트 디렉토리에 만들어라. 너의 코드에서 `/static/` 을 통해서 참조할 수 있다.



```
function MyImage(){
    return <img src="/static/my-image.png" alt="my image" />
}

export default MyImage
```



주의하세요! - 다른 어떤 것도 `static` 이라는 이름으로 디렉토리명을 만들지 마세요! 이 이름은 필수적이고, next.js가 static 파일을 제공하기 위해 사용하는 폴더입니다!



### Populating `<head>`

우리는 페이지의 `<head>` 에 (We expose a built-in component for appending elements to the `<head>` of the page.)



중복된 `<head>` 태그를 피하기 위해서 우리는 `key` 속성을 사용할 수 있습니다. `key` 는 태그가 오직 한 번만 렌더링 되게 합니다.



```javascript
import Head from 'next/head'

function IndexPage() {
  return (
    <div>
      <Head>
        <title>My page title</title>
        <meta name="viewport" content="initial-scale=1.0, width=device-width" key="viewport" />
      </Head>
      <Head>
        <meta name="viewport" content="initial-scale=1.2, width=device-width" key="viewport" />
      </Head>
      <p>Hello world!</p>
    </div>
  )
}

export default IndexPage

```

이 케이스에서 오직 두 번째 `<meta name="viewport" />` 만이 렌더링 됩니다.



주의하세요! - `<head>` 의 내용물은 컴포넌트가 언마운팅 될 때 지워지고, 



주의하세요! - `<title>` , `<meta>` 요소는 `<head>` 요소의 직계 자식이거나 `<React.Fragment>` 로 감싸져야 합니다. 그렇지 않으면 정확하게 클라이언트 사이드 이동을 할 수 없습니다.



### Fetching data and component lifecycle

state, lifecycle hooks, initial data population 등이 필요할 때 `React.Component` 를 내보낼 수 있습니다. (상태가 없는 함수형 컴포넌트 대신에)



```javascript
import React from 'react'

class HelloUA extends React.Component {
  static async getInitialProps({ req }) {
    const userAgent = req ? req.headers['user-agent'] : navigator.userAgent
    return { userAgent }
  }

  render() {
    return (
      <div>
        Hello World {this.props.userAgent}
      </div>
    )
  }
}

export default HelloUA
```



페이지를 로드할 때 데이터를 로드하려면, 우리는 async static 메서드인  `getInitialProps` 를 사용해야 합니다. 이 메서드는 비동기적으로 어떤 데이터든 가져와서 `props` 를 가지고 있는 자바스크립트 순수(?) `Object` 로 풀어냅니다. 



`getInitialProps` 로부터 반환 된 데이터는 서버 렌더링할 때 `JSON.stringify` 와 비슷하게 직렬화(?)됩니다. `getInitialProps` 는 순수 `Object` 를 반환해야 합니다. `Date` `map` `set` 등은 안 됩니다.



초기 페이지를 로드할 때 `getInitialProps` 는 오직 서번에서만 실행될 것입니다. `getInitialProps` 는 `Link` 캄포넌트나 라우팅 API들을 사용해서 다른 페이지로 이동할 때만 클라이언트에서 실행됩니다.



주의 - `getInitialProps` 는 자식 컴포넌트에서 사용될 수 없고 오직 페이지에서만 사용할 수 있습니다.



If you are using some server only modules inside `getInitialProps`, make sure to [import them properly](https://arunoda.me/blog/ssr-and-server-only-modules). Otherwise, it'll slow down your app.



또한 state가 없는 컴포넌트를 위해서  `getInitialProps`  라이프사이클 메서드를 정의할 수 있습니다.

```javascript
function Page({ stars }) {
  return <div>Next stars: {stars}</div>
}

Page.getInitialProps = async ({ req }) => {
  const res = await fetch('https://api.github.com/repos/zeit/next.js')
  const json = await res.json()
  return { stars: json.stargazers_count }
}

export default Page

```



`getInitialProps` 는 아래의 속성들을 context object를 받습니다. 

- pathname - URL의 경로
- query - URL의 query string 부분을 오브젝트로 parse 한 것
- asPath - 브라우저에 보여지는 실질적인 URL 
- req - HTTP 요청 오브젝트 (오직 서버)
- res - HTTP 반응 오브젝트 (오직 서버)
- jsonPageRes - Fetch Response object(오직 클라이언트)
- err - 렌더링이 되는 동안 에러가 일어났다면 생기는 에러 오브젝트





### Routing

Next.js는 다른 접근 가능한 경로에 대한 정보를 주지 않는다. 그래서 현재 페이지는 클라이언트 사이드일 때 다른 페이지에 대해서 알 수 없다. 다른 모든 경로들은 확작성을 위해서 후에 로딩이 됩니다.



##### With `<Link>`

routes 사이에 클라이언트 사이드 이동은 `<Link>` 를 통해서 일어납니다.

```javascript
// pages/index.js
import Link from 'next/link'

function Home() {
  return (
    <div>
      Click{' '}
      <Link href="/about">
        <a>here</a>
      </Link>{' '}
      to read more
    </div>
  )
}

export default Home

```

```javascript
// pages/about.js
function About() {
  return <p>Welcome to About!</p>
}

export default About

```



##### Custom routes (URL의 props를 사용함)

`<Link>` 컴포넌트는 2개의 주된 props를 가지고 있습니다.

- `href` : pages 디렉토리 안에 있는 페이지와 쿼리 스트링을 입력해주는 경로입니다.
- `as` : 브라우저 URL bar에 나타나는 경로입니다.



예를들어:

1. /post/:slug URL이 있다고 가정한다.
2. pages/posts.js 를 만든다.

```javascript
class Post extends React.Component {
  static async getInitialProps({query}) {
    console.log('SLUG', query.slug)
    return {}
  }
  render() {
    return <h1>My blog post</h1>
  }
}

export default Post

```

3. sesrver.js 파일에 있는 express에 경로를 추가한다. 이 것은 /post/:slug URL을 pages/post.js로 안내할 것입니다. 그리고 getInitialProps에서 query 변수의 부분으로 `slug` 를 제공할 것입니다.

   ```javascript
   server.get("/post/:slug", (req, res) => {
     return app.render(req, res, "/post", { slug: req.params.slug })
   })
   
   ```

4. 클라이언트 사이드 라우팅을 할 때는 `next/link` 를 사용하세요

   ```javascript
   <Link href="/post?slug=something" as="/post/something">
   
   ```



꿀팁 - 때때로 백그라운드에서 연결, 프리페치를 하기위해서  `<Link prefetch>` 를 사용하면 성능이 좋아진다. 



클라이언트 사이드 라우팅은 브라우저처럼 행동한다.

1. 컴포넌트를 불러온다.
2. `getInitialProps` 가 정의되어 있다면 데이터를 가져오고, 에러가 일어나면 `_error.js` 가 렌더링 된다.

3. 1, 2번 과정이 끝난 후에, `pushState` 가 실행된다. 그리고 새로운 컴포넌트가 렌더링된다.



`pathname` , `query` , `asPath` 를  컴포넌트에 넣고 싶다면 withRouter를 사용해야한다.



##### With URL object

`<Link>` 컴포넌트는 URL 오브젝트를 받을 수 있고 URL 오브젝트를 자동으로 URL strung으로 만들 수 있다.

```
// pages/index.js
import Link from 'next/link'

function Home() {
  return (
    <div>
      Click{' '}
      <Link href={{ pathname: '/about', query: { name: 'Zeit' } }}>
        <a>here</a>
      </Link>{' '}
      to read more
    </div>
  )
}

export default Home

```

위의 코드는 `/about?name=Zeit` 를 만들어낸다. 사용할 수 있는 속성은 이 문서에 정의되어 있습니다. 

https://nodejs.org/api/url.html#url_url_strings_and_url_objects



##### Replace instead of push url

`<Link>` 컴포넌트의 기본 행동은 stack에 새로운 url을 push하는 것입니다. 새로운 entry를 막기위해 `replace` prop을 사용할 수 있습니다.



##### Using a component that supports `onClick`

`<Link>` 컴포넌트는 onClick 이벤트를 지원하는 컴포넌트를 지원합니다. (뭔 개소리냐) 아래의 코드같은 경우 `<a>` 태그를 제공하지 않았지만, onClick 이벤트 핸들러가 추가되며, won't pass the `href` property.



##### Forcing the Link to expose `href` to its child

만약 자식 컴포넌트가 `<a>` 태그이고, href 속성을 가지고 있지 않다면 우리는 사용자가 필요로하지 않는 반복을 위해 나태내야 합니다. 그러나 때떄로 `<a>` 안쪽으로 들어가고 싶을 때가 있을 것인다. 그리고 `<Link>` 는 `<a>` 태그를 하이퍼링크로 인지하지 못할 것입니다. 그 결과 `href` 는 자식 컴포넌트에게 전해질 수 없을 것입니다. 이와같은 케이스에서 `<Link>` 컴포넌트의 `passHref` 속성을 정의해야 합니다. 이렇게 하면 `href` 는 자식 컴포넌트에게 보여질 것입니다…?



(링크 컴포넌트는 특정 태그로 감싸지지 않은 string을 자식으로 할 수 없다.)

**Please note**: using a tag other than `a` and failing to pass `passHref` may result in links that appear to navigate correctly, but, when being crawled by search engines, will not be recognized as links (owing to the lack of `href`attribute). This may result in negative effects on your sites SEO. (머라는거냐 개짜증나게)

하지만 검색 엔진에 의해 찾아질 때는 링크로 인지하지 못할 것이다. 



##### Disabling the scroll changes to top on page

`<Link>` 컴포넌트는 기본적으로 스크롤을 페이지의 상단으로 이동시킨다. 하지만 특정 프로퍼티를 설정해주면 특정 id로 스크롤한다.(마치 a 태그처럼) 가장 상단으로 스크롱이 가는 것을 막으려면 `scroll={false}` 를 `<Link>`에 추가하면 된다. 

```
<Link scroll={false} href="/?counter=10"><a>Disables scrolling</a></Link>
<Link href="/?counter=10"><a>Changes with scrolling to top</a></Link>
```





##### Imperatively

우리는 `next/router` 를 통해서도 클라이언트 사이드 이동을 할 수 있다.

```
import Router from 'next/router'

function ReadMore() {
  return (
    <div>
      Click <span onClick={() => Router.push('/about')}>here</span> to read more
    </div>
  )
}

export default ReadMore

```



##### Intercepting `popstate`

커스텀 라우터를 사용하는 것 같은 몇몇 케이스에서, `popstate` 를 listen하고, Router가 작동하기 전에 반응하고 싶을 수도 있다. 예를 들어 request를 manipulate하거나, SSR 새로고침을 하는 등...



```javascript
import Router from 'next/router'

Router.beforePopState(({ url, as, options }) => {
  // I only want to allow these two routes!
  if (as !== '/' || as !== '/other') {
    // Have SSR render bad routes as a 404.
    window.location.href = as
    return false
  }

  return true
})

```



만약 이 `beforePopState` 함수에서 `false` 를 리턴한다면, `Router` 는 `popstate` 를 처리하지 않습니다. 그리고 이 경우엔 우리가 처리할 필요가 있습니다. 



위의  `Router` 객체는 아래의 API들을 갖습니다.

- `route` - 현재 route를 문자열로 표현한 값
- `pathname` - 쿼리 스트링을 제외한 현재 루트를 문자열로 표현한 값
- `query` - 쿼리 스트링이 파싱된 객체, 기본 값은 `{}` 이다.
- `asPath` - 브라우저에 보여지는 실질적인 경로(쿼리 스트링 포함)를 문자열로 표현한 값
- `push(url, as=url)` - 주어진 url을  `pushState` 한다.
- `replace(url, as=url)` - 주어진 url을 `replaceState` 한다.
- `beforePopState (cb=function)` - 라우터가 이벤트를 진행하기 전에 popstate를 가로챈다.



`push` 그리고 `reaplce` 의 두번 째 인자 `as` 는 URL의 decoration을 위한 옵셔널한 것이다. 만약 서버에 대해 커스텀 루트를 설정했다면 유용할 것이다.



##### With URL object

`<Link>` 컴포넌트에서 사용했던 것과 동일하게 URL 오브젝트를 사용할 수 있다.

```javascript
import Router from 'next/router';

const handler = () => {
  Router.push({
    pathname: '/about',
    query: { name: 'Zeit' }
  });
};

function ReadMore() {
  return (
    <div>
      Click <span onClick={handler}>here</span> to read more
    </div>
  );
}

export default ReadMore;

```



##### Router Events



##### Shallow Routing

`getInitialProps` 없이 라우팅을 한다.



##### Customizing webpack config

모듈로써 이용가능 하도록 일반적으로 요구받는 특징들



주의 - `webpack` function은 두 가지가 동시에 실행된다. 하나는 서버에서 하나는 클라이언트에서





