# Primitive Types

### 기본

객체는 properties의 모임입니다. property는 object 혹은 원시타입을 참조할 수 있습니다. Primitives는 값이며, properties를 가지지 않습니다. 



자바스크립트에는 `undefined`, `null`, `boolean`, `string`, `number` 의 5가지 primitive 타입이 있습니다. 나머지는 모두 object입니다. primitive type인 boolean, string, number는 대응하는 object가 있어서 object로 감싸질 수 있습니다. 이런 object는 각각의 `Boolean`, `String`, `Number` 생성자의 인스턴스입니다. 

아래는 예시입니다. 

```javascript
typeof true; // "boolean"
typeof new Boolean(true) // "object"
```



 ### primitive 타입은 properties가 없다고 했는데 `"abc".length` 는 왜 값을 반환하나요?

그 이유는 자바스크립트가 primitives와 object를 아무렇지않게 강제하기(coerce) 때문입니다. 이 케이스에서 string value는 property인 length에 접근하기 위해서 string object로 coerce됩니다.

