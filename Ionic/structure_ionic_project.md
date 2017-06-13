# Ionic2의 프로젝트 구조

src 폴더에 작업 코드가 있고, 컴파일 후 www 폴더로 전환되는 구조.

ionic serve로 앱을 실행하면 src 폴더 내용이 컴파일되고 합쳐져서 www 폴더로 저장(`rollup js` 활용)

`www/build`

main.css

> src 폴더의 모든 css 파일들이 main.css 파일들이 저장 됌

main.js

> src 폴더의 모든 TS 파일들이 js로 컴파일되고 합쳐져 main.js로 저장.

index.html을 보면 build/main.js와 build/main.css를 참조하는 것을 확인할 수 있다.