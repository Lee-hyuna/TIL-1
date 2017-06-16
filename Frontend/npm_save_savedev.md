# npm install --save와 --save-dev 차이

1. npm install

./node_modules 디렉터리에 패키지 설치를 하고 종료.



2. npm install --save 또는 npm install --save-dev

`./node_modules` 디렉터리에 패키지 설치 + `./package.json` 디렉터리에 업데이트를 같이 해준다. 또한 -dev를 하면 `package.json` 파일 내에 `devDependencies` object에 추가를 해주고, 그렇지 않을때는 `dependencies` object에 추가해준다.



#### dependencies와 devDependencies의 차이

dependencies는 항상 설치되고, devDependencies 는 --production 옵션을 붙이면 빠진다.



## Reference

http://ohyecloudy.com/ddiary/2016/09/04/til-npm-install-save-or-save-dev/