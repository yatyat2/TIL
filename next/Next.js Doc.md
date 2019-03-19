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