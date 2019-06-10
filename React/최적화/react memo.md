# react memo

```jsx
import React from "react"
import Test from "....."

class Index extends React.Component{
	constructor(props){
    super(props)
    this.state = {
      number: 0;
    }
  }
  
  render(){
    return <div>
      <div onClick={this.addNumber}>{this.state.number}</div>
    <Test name="asd"/>
    </div>
  }
  
  const addNumber = () =>{
    this.setState(prevState=>({
      number:prevState.number + 1
    }))
  }
}
---------------------------------------------------------------------------
class Test extends React.Component{
  
  render () {
		console.log("렌더링")
    return <div>{this.props.name}</div>
  }
}

export default Test
```

위와 같은 코드가 있을 때 onClick을 할 때마다 콘솔에 렌더링 이라는 글자가 찍힌다.



```jsx
import React from "react"

class Index extends React.Component{
	constructor(props){
    super(props)
    this.state = {
      number: 0;
    }
  }
  
  render(){
    return <div>
      <div onClick={this.addNumber}>{this.state.number}</div>
    <Test name="asd"/>
    </div>
  }
  
  const addNumber = () =>{
    this.setState(prevState=>({
      number:prevState.number + 1
    }))
  }
}
---------------------------------------------------------------------------
import React, { memo } from "react"

class Test extends React.Component{
  
  render () {
		console.log("렌더링")
    return <div>{this.props.name}</div>
  }
}

export default memo(Test)
```

근데 이렇게 하면 memo라는 놈이 props를 기억하고 있어서 바뀌지 않으면 render되지 않는다고 한다.



### 그럼 모든 컴포넌트에 memo를 붙이면 되지않나?

그런데 memo랑 비교하는 것 또한 코스트가 없는 것은 아니라고 하고, 리액트는 이미 re-redering에 대한 최적화가 매우 잘 되어있다고 한다. 그래서 작은 컴포넌트까지 모두 붙일 필요는 없고, 확실히 최적화가 필요한 컴포넌트에만 붙여주는 것을 추천한다.