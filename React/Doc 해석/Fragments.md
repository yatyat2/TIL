# Fragments

리액트에서 흔한 패턴은 컴포넌트가 여러 요소를 반환하는 것입니다. Fragments는 DOM에 노드 추가 없이 자식들을 그룹화 할 수 있습니다.



```react
render() {
  return (
    <React.Fragment>
      <ChildA />
      <ChildB />
      <ChildC />
    </React.Fragment>
  );
}
```



위 문법을 줄여서 쓸 수 있습니다. 하지만 대부분의 툴에서는 지원하지 않습니다.



### Motivation

하위 요소들을 반환하는 컴포넌트는 흔한 패턴입니다. 아래의 리액트 코드 쪼가리를 봅시당.

```react
class Table extends React.Component {
  render() {
    return (
      <table>
        <tr>
          <Columns />
        </tr>
      </table>
    );
  }
}

```

<Columns />가 <td> 요소 여러 개 반환해야 렌더링 된 HTML이 유효할 수 있습니다. 만약 div 태그가 `render()` 함수 안에 있는 <Columns />에서 반환 된다면, HTML 결과물은 유효하지 못하게 됩니다.(<tr> 태그 사이에 <div>가 들어갈 수 없기 때문이다.) 



```react
class Columns extends React.Component {
  render() {
    return (
      <div>
        <td>Hello</td>
        <td>World</td>
      </div>
    );
  }
}
```



```react
<table>
  <tr>
    <div>
      <td>Hello</td>
      <td>World</td>
    </div>
  </tr>
</table>

```

이런식이 되기 때문이다.



이런 문제점을 Fragments가 해결해 준다.



### Usage

```react
class Columns extends React.Component {
  render() {
    return (
      <React.Fragment>
        <td>Hello</td>
        <td>World</td>
      </React.Fragment>
    );
  }
}
```

그냥 이렇게 쓰면 Columns 컴포넌트의 output은

```react
<table>
  <tr>
    <td>Hello</td>
    <td>World</td>
  </tr>
</table>
```

가 된다.



### Short Syntax

새로운 단축 문법이 있다. 빈 태그로 정의하면 된다.

```
class Columns extends React.Component {
  render() {
    return (
      <>
        <td>Hello</td>
        <td>World</td>
      </>
    );
  }
}
```

우리는 `<></>`를 지금까지 사용한 요소들과 동일하게 사용할 수 있습니다. 단 keys, attributes를 제외합니다.

대부분의 툴이 위 단축 문법을 지원하지 않는다는 것을 알아두세요! 



### Keyed Fragments

`<React.Fragment>`로 정의한 Fragments는 keys를 가지고 있을 지도 모른다. 아래의 유즈 케이스는 Fragment 배열 모음을 감싸고 있습니다. 

```react
function Glossary(props) {
  return (
    <dl>
      {props.items.map(item => (
        // Without the `key`, React will fire a key warning
        <React.Fragment key={item.id}>
          <dt>{item.term}</dt>
          <dd>{item.description}</dd>
        </React.Fragment>
      ))}
    </dl>
  );
}

```

`key`는  `Fragment`로 보낸 속성에 불과합니다. 나중에 속성을 위해 이벤트 핸들러 같은 것을 추가할 지도 모른다. 