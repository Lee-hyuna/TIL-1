# Cloud Function

AWS lambda나, 구글 클라우드 펑션은 서버코드를 클라우드화해버려서 Serverless를 지향하는 아키텍쳐이다. 서버 없이 함수 호출 횟수만큼 과금한다. 구글 클라우더 펑션 기준으로 정리를 해본다.

## 동작 조건

##### 개발 언어

node.js 6.9.1 버전 기반으로 작동.

##### 트리거

이벤트에 따라서, 코드를 수행해준다.

- Pub/Sub 메세지 큐에서 들어오는 메세지
- Firebase 모바일 SDK에 의해서 발생되는 이벤트
- Google Cloud Storage 서비스에 의해서 파일이 생성,수정,삭데 되었을때
- HTTP로 들어오는 요청 (REST API)

## 시작하기

#### 펑션 종류

- HTTP 펑션 : request, response 

````javascript
exports.httpFunction = function httpFunction (req, res) {
        switch(req.method){
          case 'POST':
           res.send('SUCCESS!');
        }
};
````

- 백그라운드 펑션 : GCS, Pub/Sub 등의 이벤트로 트리거된다고 함.

#### 배포하기

- Web UI나 CLI를 통해 배포한다. [조대협님 블로그 참조](http://bcho.tistory.com/1168)

## Reference

 [조대협의 블로그] : http://bcho.tistory.com/1168
