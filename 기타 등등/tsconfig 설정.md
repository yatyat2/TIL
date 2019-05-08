# tsconfig 설정

```javascript
{
  "compilerOptions": {
    
    // js 파일에 문법 오류가 있는지 없는지 확인하는 옵션
    "allowJs": true,

    // export default 를 export 한 값들을 가지는 객체로 설정
    "allowSyntheticDefaultImports": true,

    // jsx 사용 여부라는데.. 흠.
    "jsx": "preserve",

    // 컴파일에 포함될 라이브러리 파일 목록
    "lib": ["dom", "es2017"],

    // 모듈 설정
    "module": "esnext",

    // 모듈 해석 방식 결정
    "moduleResolution": "node",

    // 결과 파일을 저장하지 않음
    "noEmit": true,

    // 사용하지 않는 지역변수에 대한 오류 보고 여부
    "noUnusedLocals": true,

    // 사용하지 않은 파라미터에 대한 오류 보고 여부
    "noUnusedParameters": true,
    
    // const enum형 선언을 지우지 않을건지 여부 
    "preserveConstEnums": true,

    // 주석 삭제
    "removeComments": false,

    // 모든 선언파일(*.d.ts)의 유형검사를 건너뛸지 여부
    "skipLibCheck": true,

    // 소스맵 파일 생성 여부
    "sourceMap": true,

    // 엄격한 타입 검사 옵션 활성화
    "strict": true,

    // 사용할 ECMAScript 버전 설정
    "target": "esnext"

		// ES Decorator에 대한 실험적 기능 사용 여부를 의미한다.
		"experimentalDecorators": true

  }
}
```

