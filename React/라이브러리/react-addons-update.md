# react-addons-update



### update(parm1,parm2)



```javascript
this.setState({
    array:update(
    	this.state.array,
        {
            $push:[element1,element2,element3]
        }
    )
})
```

메소드의 첫 파라미터는 처리할 배열이다.

두 번째 파라미터는 처리할 명령들을 지니고 있는 객체이다.

- `$push : 배열타입의변수`
  - `update()` 의 첫 파라미터로 들어온 배열에 배열을 추가한다.