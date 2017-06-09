# Firebase

웹과 모바일 개발에 필요한 기능을 제공하는 BaaS(BackEnd as  a Service)로, 프론트 엔드개발에 더축 집중할 수 있음.



## 시작하기

프로젝트를 만든다.

### Authentication

`로그인방법` 탬에서 원하는 인증방식을 활성화 한다. `Authorized Domains`탬에서 리다이렉션 도메인을 추가할 수 있다.

`템플릿` 탭에서는 이메일 주소 인증을 위한 템플릿을 설정할 수 있음.

### Database

`데이터` 탭에서 저장된 데이터를 확인

- 모든 데이터는 Json으로 저장
- 데이터를 import/export 할 수 있음.
- 캐싱 방식으로 사용되기 때문에, Offline시에도 사용이 가능.
- 모든 클라이언트에게 데이터 변경시, 모든 클라이언트에게 전송이됌.

`규칙` 탭에서는 데이터의 읽기 쓰기 권한 규칙을 설정

- 시뮬레이터를 활용하여 권한이 제대로 설정되었는지 확인 가능.

## 로컬 개발환경 셋팅

1. http://nodejs.org 에서 nodejs 설치
2. firebase cli 설치

> \> npm install firebase-tools -g

3. firebase cli에 로그인

컴퓨터와 파이어베이스를 연결하고, 프로젝트에 대한 Access를 허용시켜줌.

> \> firebase login
>
> \> y

이후 권한허용 여부를 묻는 브라우저 새창이 나오면 Firebase CLI 허용 버튼을 클릭

4. firebase 프로젝트 설정

> \> cd [파이어베이스 프로젝트 경로]
>
> \> firebase init

![img](http://i.imgur.com/fLauqJj.png "오오오오오옷!!")

> 오오오옷!!!! 간지

'Y'를 눌러 진행해준다

5. Database 셋팅

- 설정 항목중에서 첫번째 Database를 선택
- 위에서 생성한 프로젝트를 선택해준다.



CLI를 통해 자동으로 index.html및 기본 파일들이 설정되는데, 안되는 경우 다음과 같이 파일을 확인.

- database.rule.json : DB 데이터 읽기 쓰기 규칙. 

````json
// 디폴트 규칙
{
  "rules": {
    ".read": "auth != null",
    ".write": "auth != null"
  }
}
````

- public/index.html : 폴더를 생성하고 index파일을 생성한다.

```html
<html>
<head>
   .... 이곳에 웹용 파이어베이스 코드를 추가해준다
  <script src="https://www.gstatic.com/firebasejs/4.1.2/firebase.js"></script>
  <script>
    // Initialize Firebase
    var config = {
      ...
    };
    firebase.initializeApp(config);
  </script>
</head>
<body>
	성공!
</body>
</html>
```

- firebase.json : 호스팅시 어떤 폴더를 이용할 것이며, 어떤 경로와 어떤 파일을 연결할지 설정

````json
{
  "database": {
    "rules": "database.rules.json"
  },
  "hosting": {
    "public": "public", // 페이지가 들어 있는 폴더명
    "rewrites": [{
      "source": "**",   // 페이지 주소
      "destination": "/index.html" // 연결할 페이지
     }]
  }
}
````

6. 서버 구동

> \> firebase serve

위 키워드를 입력하고 http://localhost:5000 을 접속하여 페이지가 나오는 것을 확인한다.

## Reference

https://www.inflearn.com/course-status-2/





