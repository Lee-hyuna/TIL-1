## OAuth의 이해

Token 인증 방식을 쓰는 이유는

- 세션 없이 인증하기 위해
- 크로스 도메인 관리 용이

라고 지난 시간에 공부했다.



이런 토큰 인증을 사용하는 방식이 Oauth이다.



Oauth에서는

권한인증을 담당하는 **Consumer**을 둠으로써.



내 Id/Pw를 해당 어플리케이션에 노출하지 않고.

신뢰할 수 있는 Consumer에게만, 권한 요청을 하고.

토큰을 받아 인증을 한다.



따라서 신뢰할 수 없는 어플리케이션에

**ID/PW를 노출하지 않는 장점**

이 있다고 할 수 있다.



## Oauth 1.0과 2.0 비교

2.0 부터는 https를 사용하기 때문에

- token을 따로 암호화 하지 않는다.(Bearer 토큰)

명칭이 변경 되었다.

-  oauth_token -> access_token

Access Token이 노출될 위험을 막기 위해

유효기간을 최대한 짧게 하고,

토큰을 갱신할 수 있는 refresh Token을 두었다.

- Refresh Token을 사용한다.



## Reference

[Oauth 2.0의 이해](http://linuxism.tistory.com/1545)





