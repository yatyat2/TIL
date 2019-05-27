# Call Stack

자바스크립트는 single threaded single concurrent language이다. 한 가지 task 혹은 코드 조각을 하나씩만 다룰 수 있다. 자바스크립트는 하나의 call stack과 Javascript Concurrency Model을 구성하는 heap, queue을 가지고 있다. 



### Call Stack

함수 호출을 기록하는 데이터 구조이다. 함수를 실행하기 위해서 호출하면 stack으로 무언가를 push합니다. 그리고 함수로부터 반환받을 때 우리는 stack의 가장 위에 있는 것이 튀어나옵니다.



브라우저 콘솔에서 빨간색 에러 stack을 본적이 있을 것 입니다. 이 에러는 일반적으로 call stack의 현재 상태를 나타내며 마치 스택처럼 함수가 실패한 가장 위쪽부터 아래쪽까지 상태를 보여줍니다.



때론, 재귀적으로 여러 함수를 호출하다보면 무한 루프가 일어나게 됩니다. stack의 사이즈 제한은 16,000프레임인데 이보다 높아지면 에러가 발생하고 `Max Stack Error Reacjed ` 에러를 반환합니다.



### Heap

Object는 heap에 할당된다. 즉 메모리에서 가장 구조적이지 않은 부분입니다. 변수나 object에 대한 메모리 할당은 모두 여기서 일어납니다.



### Queue

자바스크립트 런타임은 message queue를 유지한다. message queue는 처리되어야하는 메시지의 리스트, 관련 callback 함수를 실행하기 위한 리스트입니다. Stack이 넉넉할 때, message는 queue밖으로 나가게됩니다. 그리고 메시지는 관련함수 들의 호출로 처리됩니다. message 과정은 stack이 비어질 때까지 계속됩니다. 기본적으로, 이런 message들은 (콜백 함수로 제공되어진)외부 비동기 이벤트의 응답을 기다리는 중입니다. 예를들어 유저가 버튼을 클릭했지만 콜백 함수가 제공되지 않으면, message에도 대기하지 않습니다. 



### Event Loop

기본적으로, 자바스크립트 코드의 성능을 평가할 때, Stack안에 있는 함수가 선능을 결정합니다. `console.log`()

 는 빠르지만 `for` , `while` 같은 반복문들이 많을수록 느려지고, 스택을 차지할 것입니다. 이런 것을 Webpage Speed Insights에서는  `bloking script` 라고 부릅니다 .  

네트워크 요청은 느려질 수 있고, 이미지 요청도 느려질 수 있다. 하지만 다행히도 서버 요청은 비동기 함수인 AJAX로 처리할 수 있다. 

네트워크 요청이 동기 함수로 만들어져있다고 가정해보자. 무슨일이 일어날까? 네트워크 요청은 어딘가에 있는 컴퓨너나 기계인 서버로 보내고, 컴퓨터는 응답을 느리게 받을 수 있다. 만약 컴퓨터에서 call to action 버튼을 클릭하거나 렌더링될 필요가 있다면, 스택이 block되어 있기 때문에 아무 일도 일어나지 않을 것이다.

멀티 스레드 언어인 루비에서 위의 일들은 그냥 처리된다. 하지만 자바스크립트같은 싱글 스레드 언어에서는 stack안에 있는 함수가 값을 반환하기 전에는 불가능한 일이다. 웹페이지는 브라우저가 아무것도 못할 만큼 혼란스러워질 것입니다. 어떻게 해결할 수 있을까요?



*자바스크립트에서의 동시성 - 한 번에 한 가지만, 비동기 callback을 제외하고*



가장 쉬운 솔루션은 비동기 callback을 사용하는 것입니다. callback은 코드를 실행한뒤 callback 함수를 줘서 후에 실행하도록 하는 것입니다. 우리는 대부분은 `$.get(),setTimeout(),setInterval(), Promises` 를 사용해서 AJAX 요청을 마주하게됩니다. Node는 비동기 함수 실행에 대한 모든것 입니다. 비동기 callback은 바로 실행하는 것이 아니라 시간이 흐른 후애 실행하게 됩니다. 그래서 동기함수처럼 stack에 바로 push되지 않습니다. 비동기함수는 어떻게 처리될까요??



```javascript
function loadDoc(){
  var xhttp = new XMLHttpRequest();
  xhttp.onreadystatechange = function(){
    if(this.readyState == 4 && this.status == 200){
      console.log(this.responseText);
    }
  }
  xhttp.open("GET","ajax_info.txt",true);
  xhttp.send();
	console.log("Script call done");
}
```

위의 코드처럼 자바스크립트에서 네트워크 요청이 있다고 가정해보자. 

1. request 함수가 실행되면서 `onreadystatechange` 이벤트는 응답이 있을 때 실행되는 콜백으로 보내진다. 
2. "Script all done" 은 console에 바로 보입니다.
3. 시간이 흐른 후에 응당이 오게 되면 callback이 실행되고 함수 body 부분에 있는 console.log()가 실행된다. 



response로부터 caller를 분리하면 자바스크립트 런타임 때 비동기 실행을 기다리는 동안 다른 것을 할 수 있습니다. 이것은 브라우저가 실행되고 호출하는 곳입니다. 기본적으로 DOM event, http request, setTimeout같은 비동기 이벤트들을 처리하기 위해 C++로 실행이 되는 브라우저에서 생성한 스레드입니다. (?뭐라는거야?)





*Browser Web APIs* : 브라우저에서 만든 스레드입니다. 브라우저는 C++로 비동기 이벤트를 다룹니다.

https://medium.com/@antwan29/browser-and-web-apis-d48c3fd8739 



Now WebAPIs는 stack에 스스로 실행 코드를 넣을 수 없습니다. 만약 넣어졌다면, 코드의 중간에서 랜덤하게 나타날 것입니다. message callback queue는 위의 방법들을 결정합니다. `3`어떤 WebAPI가 실행될 때 callback을 queue에 넣는다. 그러면 Event Loop는 queue안에 있는 callback 실행에 반응합니다. 그리고 stack이 비어져있다면, callback을 stack에 넣습니다. `4` Event loop의 기본적인 역할은 stack과 task queue를 둘 다 보면서, stack이 비어져 있으면 queue의 첫 번째 것을 stack에 넣어줍니다. 각 message나 callback은 다른 Message가 처리되기 전에 완벽하게 처리됩니다. 



```
while (queue.waitForMessage()){
	queue.processNextMessage();
}
```



웹 브라우저에서, 이벤트가 일어날 때마다 message가 추가됩니다. 그리고 이벤트 리스너는 message에 attached됩니다. 만약 리스터가 없다면, 이벤트는 사라지게됩니다. 즉, click 이벤트 핸들러를 가진 element를 클릭하면 message가 추가됩니다. 이 callback 함수 호출은 call stack 안에 초기 frame을 제공하비낟. 그리고 자바스크립트가 싱글 스레드이기 때문에, message polling과 processing은 stack에 있는 모든 호출들이 반환될 때까지 멈춰진다. 







---

### MDN - call stack

call stack은 interpreter를 위한 여러 함수를 호출하는 스크립트의 위치를 추적하기 위한 메커니즘입니다.  (여기서 함수는 현재 실행중인 함수 , 함수 안에서 호출되는 함수등등...)

- 스크립트가 함수를 호출할 때 interpreter는 함수를 call stack에 추가하고 함수 실행을 시작합니다. 
- 함수에 의해 호출된 함수들은 call stack의 더 위쪽에 추가됩니다. 그리고 호출이 도달했을 때 실행합니다.(이게 머슨소리..)

- 현재 함수 실행이 끝나면, interpreter는 stack에서 제거하고 지난 코드 리스트의 중단된 부분에서 실행을 재개합니다.
- stack이 수용할 수 있는 공간보다 더 많은 공간을 할당하게 되면 이것을 "stack overflow" 에러라고 합니다.







---



`concurrent language` : 여러개의 task가 있을 때 스케줄링을 통해 서로 다른 task들이 번갈아가며 처리되는 그런 느낌...

`polling` : refers to actively sampling the status of an [external device](https://en.wikipedia.org/wiki/External_device) by a [client program](https://en.wikipedia.org/wiki/Client_program) as a synchronous activity. (I/O라고 생각하면 되는듯)

