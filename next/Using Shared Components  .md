# Using Shared Components

우리는 Next.js는 pages에 관한 전부라는 것을 알고 있습니다. 우리는 React Component를 내보내는 방식으로 페이지를 만들 수 있습니다. 그리고 그 컴포넌트 들을 `pages` 디렉토리에 넣어서 관리되는 중입니다. 그 다음 파일 이름에 기반하여 고정된 URL을 가지게 됩니다.



페이지가 자바스크립트 모듈을 내보내기 때문에 우리는 다른 자바스크립트 컴포넌트를 받아들일 수 있습니다. (Since exported pages are JavaScript modules, we can import other JavaScript components into them as well. )



`이 특징은 자바스크립트 프레임워크에게 바라던 것입니다.`



이 수업에서 우리는 공통된 헤더 컴포넌트를 만들고 여러 페이지에서 사용할 것이빈다. 그리고 우리는 레이아웃 컴포넌트의 실행을 보고, 이 것이 어떻게 여러 페이지의 look을 정의하도록 우리를 돕는지 볼 것입니다.