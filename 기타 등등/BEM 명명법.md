# BEM 명명법

### 목표

CSS 셀렉터 이름을 명확하게 하는 것



### Block

- 재사용 할 수 있는 기능적으로 독립적인 페이지 구성 요소(컨테이너 같은 애들을 말하는 건가)
- 형태가 아닌 목적에 맞게 결정해야 한다.
- 블록은 환경에 영향을 받지 않아야 한다. 즉, 여백이나 위치를 설정하면 안된다.
- 태그, id 선택자를 사용하면 안된다.
- 블록은 서로 중첩해서 작성할 수 있다.



### Element

- 블록 안에서 특정 기능을 담당하는 부분
- blockName__elementName 형태로 사용한다. (언더바 2 개)
- 형태가 아닌 목적에 맞게 결정해야 한다.
- 요소는 블록의 부분으로만 사용할 수 있고 다른 요소의 부분으로 사용할 수 없다.

