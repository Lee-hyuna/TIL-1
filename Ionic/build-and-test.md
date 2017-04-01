## 아이오닉 테스트

아이오닉 테스트 하는 방법.



## 웹에서 테스트

> $ ionic serve
>
> 또는,
>
> $ ionic serve -lab

으로 테스트 해볼 수 있다.



## 빌드

안드로이드 빌드시 : 안드로이드 SDK 필요

[안드로이드 스튜디오](https://developer.android.com/studio/index.html?hl=ko)

IOS 빌드시 : Apple Developer 계정 등록 필요.(년 $99 필요)

[Apple Developer](https://developer.apple.com/) 



플랫폼 추가가 필요

> $ cordova platform add [android/ios]



빌드

> $ ionic build [android/ios]



혹시 빌드 중에 아래 오류가 나오는 경우

*could not find gradle wrapper within android sdk*

[SDK tools package](https://dl.google.com/android/repository/tools_r25.2.3-windows.zip) 

C:\Users\CURRENT_USER\AppData\Local\Android\sdk

에 압축을 풀어주고, 다시 시도해보자.



빌드 하면 apk 파일이 생성된다.



## 에뮬레이터

빌드후 안드로이드 스튜디오에 들어가

AVD를 생성 후, 실행시킨다.   [생성법](http://nowordeath.tistory.com/108)



다음

> $ ionic emulate [android/ios]

키고 위 명령어를 입력하면, 자동으로 apk가 설치되고 실행된다.



## 실제 디바이스 실행

실제 디바이스에서는 빌드후

> $ ionic run [android/ios]

를 입력하면된다. USB에 꼽은 상태로 하면 되는 것 같은데.

기기가 없어서 실제 실행은 못시켜보았다.



## Reference

[어제보다 오늘 더 블로그](http://nowordeath.tistory.com/108)

[Ionic Forum](https://forum.ionicframework.com/t/ionic-run-android-does-not-install-app-in-device/21795/6)

[Ionic Doc](http://ionicframework.com/docs/v1/cli/run.html)



