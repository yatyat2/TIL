# Value Types and Reference Types

자바스크립트는 value로 취급되는 `Boolean`, `null`, `undefined`, `String`, `Number` 라는 primitive types을 가지고 있다. 



자바스크립트는 reference로 취급되는 `Array`, `Function`, `Object` 라는 object를 가진다.



### Primitives

만약 primitive type이 변수에 할당된다면, 우리는 변수가 primitive value를 포함한다고 생각할 수 있다. 

```javascript
var x = 10;
var y = 'abc';
var z = null
```

x는 10을 가진다.

y는 abc를 가진다. 

우리가 위의 변수들을 다른 변수에 `=` 을 사용해서 할당하려고 할 때, 우리는 value를 다른 새로운 변수에 복사한다. 변수들은 value에 의해 복사된다.



```
var x = 10;
var a = x
```

variable / values

x / 10

a / 10

한 변수를 바꾸는 것은 다른 한 변수를 바꾸는 것이 아니다. 서로 아무런 관계가 없다. 



### Object

여기 좀 헷갈린텐데 버티고 잘 읽어봐요. 한 번 읽어보면 매우 쉬워보일 거예요.



non-primitice value가 할당된 변수는 value에 reference가 주어집니다. reference는 메모리 안에 있는 obejct의 위치를 가르킵니다. 이 변수는 value를 포함하지 않습니다. 



object는 컴퓨터의 메모리의 어떤 위치에서 만들어집니다. 우리가 `arr = []` 를 쓴다는 것은 메모리 안에 array를 만든 것입니다. 변수 `arr` 이 받은 것은 배열의 주소이고, 위치입니다. 







