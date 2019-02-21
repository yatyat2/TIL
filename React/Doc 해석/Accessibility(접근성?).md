# Accessibility(접근성?)

### Why Accessibility? (왜 접근성인가?)

Web accessibility은 모두가 사용할 수 있는 웹사이트를 설계하고 만드는 것이다. Accessibility 지원은 웹페이지를 해석하는 기술들에게 필수적이다. 



리액트는 접근성이 좋은 웹사이트를 만드는 것을 지원합니다. 종종 표준 HTML 기술을 사용하기도 합니다.



### Standars and Guidelines

##### WCAG

Web Content Accessibility Guidelines는 접근성이 좋은 웹 사이트 만들기를 위한 가이드라인을 제공합니다.



(요약 링크들이 있는 곳)



##### WAI-ARIA

Web Accessibility Initiative - Accessible Rich Internet Applications 문서는 접근성이 좋은 자바스크립트 위젯을 만들기 위한 정보(기술)를 가지고 있습니다.



모든 `aria-*` HTML 속성은 JSX에서 완벽하게 지원됩니다. DOM의 properties, attributes는 리액트에서 카멜 케이스이므로 `aria-*` 는 일반 HTLM에서 사용되던 것 처럼 hyphen-case로 사용됩니다. 



### Sematic HTML

Semantic HTML은 웹 어플리케이션에서 접근성의 기초이다. 다양한 HTML 요소를 사용해서 웹사이트가 나타내려고 하는 정보의 의미를 강화하면, 접근성이 좋아진다.



우리는 HTML Semantics를 지키지 않는다. 예를 들면 리액트 코드가 동작하도록 만들기 위해서 <div>를 JSX에 넣거나 <table>을 만들 때 <oi>, <li>를 넣는다. 이런 케이스에서 여러 요소를 하나로 묶기 위해 React Fragments를 사용하는 것이 좋다.



예를들어

```react
import React, { Fragment } from 'react';

function ListItem({ item }) {
  return (
    <Fragment>
      <dt>{item.term}</dt>
      <dd>{item.description}</dd>
    </Fragment>
  );
}

function Glossary(props) {
  return (
    <dl>
      {props.items.map(item => (
        <ListItem item={item} key={item.id} />
      ))}
    </dl>
  );
}

```

 `item` 집합을 `Fragments`의 배열로 대응 시킬 수 있습니다. (다른 요소들을 했던 것 처럼)



```react
function Glossary(props) {
  return (
    <dl>
      {props.items.map(item => (
        // Fragments should also have a `key` prop when mapping collections
        <Fragment key={item.id}>
          <dt>{item.term}</dt>
          <dd>{item.description}</dd>
        </Fragment>
      ))}
    </dl>
  );
}

```

Fragment 태그에서 Props가 필요하지 않을 때 단축 문법을 사용할 수 있습니다.



```react
function ListItem({ item }) {
  return (
    <>
      <dt>{item.term}</dt>
      <dd>{item.description}</dd>
    </>
  );
}
```



### Accseeible Forms

##### Labeling

`<input>` , `<textarea>` 같은 모든 HTML form 제어는 접근성이 좋게 이름이 붙을 필요가 있다. 우리는 스크린에 렌더링되는 라벨을 자세한 설명과 함께 제공해야 한다.



(어떻게 하는지 알려주는 링크 1,2,3 )



표전 HTML practices는 리액트에서 직접적으로 사용될 수 있다. 하지만 `for` attribute는 JSX에서 htmlFor로 쓰여야 한다.

```react
<label htmlFor="namedInput">Name:</label>
<input id="namedInput" type="text" name="name"/>
```



##### Notifying the user of errors

에러 상황이 있을 때 유저가 이해할 수 있어야 한다. 아래 링크는 어떻게 에러 텍스트를 보여주는지에 대한 설명이다



(링크 1,2)



### Focus Control

키보드만으로 웹 어플리케이션이 작동할 수 있는지 확인하세요.



(링크 1)



##### Keyboard focus and focus outline

키보드 포커스는 DOM 안에 있는 현재 요소들을 참조한다. 선택된 요소는 키보드의 입력을 받는다. 우리는 아래 이미지와 비슷한 포커스 아웃라인을 어디서든 볼 수 있다. 

(착한 사람한테만 보이는 이미지 ^_^)



CSS를 사용해야만 이 아웃라인을 삭제할 수 있다. 예를 들어 `outline: 0` 으로 설정하면 된다. 



##### Mechanisms to skip to desired content

어플리케이션에서 유저가 과거에 보던 부분으로 넘어갈 수 있는 메커니즘을 제공하세요.

(Provide a mechanism to allow users to skip past navigation sections in your application as this assists and speeds up keyboard navigation.)



스킵 링크 or 스킵 네비케이션 링크는 유저가 키보드로 페이지와 상호작용 할 때만 보이는 네비게이션이다. 내부 앵커와 스타일링으로 만들기가 쉽다고 한다.

(링크 1)



또한 <main>, <aside> 같은 랜드마크 elements와 roles를 사용하세요. 페이지 구역의 한계를 정하면 유저는 좀 더 빠르게 원하는 섹션으로 이동할 수 있습니다.



위의 요소들을 통해 접근성을 강화하고 싶다면 아래 링크를 보세요



(링크1)



##### Programmatically managing focus(프로그램적으로 focus를 관리하는 방법??)

우리의 리액트 어플리케이션은 실행되는 동안 HTML DOM을 계속해서 수정합니다. 수정하면서 때론 키보드 포커스를 잃거나, 예상치 못한 element로 설정하기도 합니다. 이 현상을 고치기 위해서 프로그래밍적으로 키보드 포커스가 옳은 방향으로 가도록 도와줘야 합니다. 예를 들면, 모달 창이 열려있는 버튼에 대해 모달창이 닫혔을 때 재설정하는 것이라든지...(For example, by resetting keyboard focus to a button that opened a modal window after that modal window is closed.)



MDN Web Docs takes a look at this and describes how we can build [keyboard-navigable JavaScript widgets](https://developer.mozilla.org/en-US/docs/Web/Accessibility/Keyboard-navigable_JavaScript_widgets).



리액트에서 focus를 설정하려면 DOM요소에 Refs를 사용해야 한다.



이렇게 하면, 컴포넌트 클래스의 JSX 안에서 element에 대한 ref를 만들 수 있다.

```react
class CustomTextInput extends React.Component {
  constructor(props) {
    super(props);
    // Create a ref to store the textInput DOM element
    this.textInput = React.createRef();
  }
  render() {
  // Use the `ref` callback to store a reference to the text input DOM
  // element in an instance field (for example, this.textInput).
    return (
      <input
        type="text"
        ref={this.textInput}
      />
    );
  }
}
```

이렇게 하고 나면 컴포넌트 내부의 다른 곳(코드?)에서도 input element에 focus할 수 있다. 



```react
focus() {
  // Explicitly focus the text input using the raw DOM API
  // Note: we're accessing "current" to get the DOM node
  this.textInput.current.focus();
}

```

부모 컴포넌트가 자식 컴포넌트의 element의 focus 설정이 필요할 때도 있다. 부모의 ref에서 자식의 DOM node로 향하는 자식 컴포넌트의 특별한 prop을 사용하면 된다.



```react
function CustomTextInput(props) {
  return (
    <div>
      <input ref={props.inputRef} />
    </div>
  );
}

class Parent extends React.Component {
  constructor(props) {
    super(props);
    this.inputElement = React.createRef();
  }
  render() {
    return (
      <CustomTextInput inputRef={this.inputElement} />
    );
  }
}

// Now you can set focus when required.
this.inputElement.current.focus();

```



컴포넌트를 확장하기 위해서 HOC를 사용할 때, 리액트의 `forwardRef` 함수를 사용해서 ref를 감싸진 컴포넌트에게 전달하는 것이 좋다. 만약 써드 파티 HOC가 ref 전달을 지원하지 않아도, 위 패턴은 대비책으로 쓰일 수 있습니다.



매우좋은 포커스 관리 예시는 react-ara-modal입니다. 이것은 접근성이 좋은 모달 창에 대한 흔치 않은 예시입니다. 초기 포커스를 취소 버튼으로 설정하고, 포커스를 모달 안쪽으로 설정할 뿐만 아니라 초기에 모달을 열게 했던 엘리먼트로 포커스를 돌아가게 합니다.



(ㄹㅇ 오졌다. 접근성이 뭔지 알겠따 ㄹㅇㄹㅇㄹㅇ 소르미온드)











