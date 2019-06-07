# Implicit, Explicit, Nominal, Structuring and Duck Typing

자바스크립트의 implicit coercion(묵시적 형변형)은 자바스크립트가 예상할 수 없는 value 타입을 예상 가능한 타입으로 변환하려는 것과 관련이 있습니다. 그래서 number로 기대되는 곳에 string을, string으로 기대되는 곳에 object를 전할 수 있고, 이것들을 옳은 타입으로 변환하려고 할 것입니다. 이것은 자바스크립트에서 조심해야할 특징입니다. 



### 숫자 표현에서의 non-숫자 

##### Strings

숫자 표현에 string 피연산자를 `-` `*` `/` `%` 를 포함해서 전달할 때면, 숫자의 변환 과정은 내장된 `Number` 함수를 value에 호출하는 것과 비슷합니다. 이것은 꽤 직관적이고, 어떤 string이 숫자 문자만 가지고 있다면 동일한 숫자로 변환됩니다. 그러나 string이 non-숫자 문자를 가진다면 `NaN` 을 반환하게 됩니다.



 `+` 연산자의 경우

`+` 연산자는 다른 수학적 연산자와 다르게 두 가지 기능을 합니다.

1. 덧셈
2. string 연결

string이 `+` 의 연산자일 때, 자바스크립트는 string을 number로 연산하는 대신 number를 stirng으로 변환한다.



##### Object

대부분 자바스크립트 Object 변환은 보통 [obejct, object] 라는 결과를 만듭니다. 

예를들어

```javascript
"name" + {} // "name[object, object]"
```

모든 자바스크립트 Object는 `toString` 메서드를 상속받습니다. 이 메스드는 Object가 stirng으로 변환될 때마다 호출됩니다. `toString` 메서드의 반환값은 string 연결이나 숫자 표현으로 연산됩니다. 

```javascript
const foo = {}
foo.toString() // [object, object]

const baz = {
  toString: () => "I..am... IronMan.."
}

baz + "ddack"
```



숫자식에서 자바스크립트는 메서드의 반환값을 숫자로 변환하려고 합니다.. 

```javascript
const foo = {
	toString: () => 4
}

2 + foo // 6
```



### Array objects

Array의 상속받은 `toString` 메서드는 조금 다르게 작동한다. Array에서 이 메서드는 arguments가 없는  `join` 메서드처럼 작동한다.

```javascript
[1,2,3].toString() // "1,2,3"
[].toString() // ""
[].join() // ""

"me" + [1,2,3] // "me1,2,3"
```

그래서 string이 올 것으로 생각한 곳에 array를 넘기면 자바스크립트는 `toString` 메서드의 반환값과 다른 피연산자를 연결합니다. 만약 반환 값이 숫자라면 반환 값을 숫자로 변환할 것입니다.

```javascript
4 / [2] // 2
```



### True, False and ""

```javascript
Number(true) // 1
Number(false) // 0
Number("") // 0

4 + true // 5
3 * false // 0
3 + false // 3
3 * "" // 0
3 + "" // "3" 이건 문자열임...


```



### The `valueOf` method

string, numeric value가 전달될 것으로 예상된 곳에 Object가 전달되는 경우 사용되는 `valueOf` 메서드도 정의할 수 있습니다. 

```javascript
const foo = {
	valueOf: () => 3
}
3 + foo // 6
3 * foo // 9
```



`toString` 과 `valueOf` 메서드 둘다 Object에서 정의됐을 때 자바스크립트는 `valueOf` 메서드를 사용한다.

```javascript
const bar = {
	toString: () => 2
  valueOf: () => 5
}

3 + bar // 8
```



`valueOf` 메서드는 numeric value를 표현하기 위해서 있는 메서드이다.

```javascript
const two = new Number(2)
two.valueof() // 2
```



### Falsy and Truthy























