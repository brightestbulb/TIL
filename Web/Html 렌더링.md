# 웹 브라우저에서 HTML 문서를 어떻게 렌더링 할까?

## 웹 브라우저의 HTML문서 렌더링 과정

![](https://github.com/brightestbulb/TIL/origin/master/Web/img/renderingEngine.png?raw=true)

1. HTML 파싱과 DOM 트리 구성 : 사용자가 페이지를 요청하면 네트워크를 통해 마크업을 받아온다. 그리고 마크업 문자열을 토큰 형태로 잘라서 트리를 구축하고 파싱 작업을 시작한다. 그런 다음 DOM 트리를 생성한다.
2. 렌더 트리 구성(DOM + Style 규칙) : DOM 트리를 생성한 다음 Style Sheet 규칙을 적용하여 렌더 트리를 만든다. display:none 속성처럼 DOM 트리에는 있지만 화면에 보이면 안되는 요소를 걸러낸것이 렌더 트리이다.
3. 렌더 트리의 배치 : 최종적으로 스타일 규칙에 따라 각 요소를 화면 어느곳에 배치할지 좌표를 설정한다.
4. 렌더 트리 그리기 : 요소의 좌표가 설정되면 브라우저에 순차적으로 화면을 그린다. 


## 브라우저 구조

![](https://github.com/brightestbulb/TIL/origin/master/Web/img/browser.png?raw=true)

브라우저는 화면을 조정하는 영역, 데이터를 조작하는 영역으로 구분할 수 있다.

1. UserInterface : 사용자가 직접 사용하는 부분
2. Browser engine : 사용자 인터페이스가 렌더링 엔진에 쿼리를 전달할 수 있게 조작한다.
3. Rendering engine : HTML, CSS 문서를 파싱/해석하여 화면에 표현한다.
4. Networking : Http 요청을 할 수 있으며, 네트워크를 호출할 수 있다.
5. JavaScript Interpreter : JavaScript 코드를 해석하고 실행한다.
6. UI Backend : select box, input 등 기본적인 위젯을 그리는 인터페이스이다.
7. Data Persistence : Cookie 등 모든 자료를 하드디스크에 저장한다.
