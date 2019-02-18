# ref

### ref가 필요한 경우

DOM에 직접적인 접근을 해야할 때

1. input / textarea 등에 포커스를 할 때
2. 특정 DOM의 크기를 가져올 때
3. 특정 DOM에서 스크롤 위치를 가져오거나 설정할 때
4. 외부 라이브러리를 사용할 때



```
class Test extends React.Component{
    private input = null;
    
    render(){
        return (
        <div>
        	<input
        	ref={ref ={
				this.input = ref
			}}
        	>
        	<button
        		onClick={this.onFocus}
        	/>
        </div>
        )
    }
    
    private onFocus = () =>{
        this.input.focus();
    }
    
}
```

컴포넌트 내부에 변수를 선언하고(타입스크립트를 사용 중이라면 Event 뭐시기 타입 지정해야 하더라...)

ref 값을 설정하고 싶은 element에 ref={ref=>{this.input=ref}}를 넣어? 준다.

그리고 this.input을 이용하면 해당 DOM에 접근이 가능

