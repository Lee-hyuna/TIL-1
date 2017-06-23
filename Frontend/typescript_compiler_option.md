# Typescript 컴파일 옵션

> $ tsc --init

다음 커맨드를 입력하면, tsconfig.json이 생성된다. 해당 파일이 있으면 `tsc 파일`커맨드 입력시 해당 설정을 통해서 컴파일 한다.

- tsconfig.json

root 폴더에 `tsconfig.json`을 생성한다

```json
{
    "compilerOptions": {
        "outDir": "./public/",
        "sourceMap": true,
        "noImplicitAny": true,
        "module": "commonjs",
        "target": "es5",
      	"typeRoots": ["./node_modules/@types",
                      "./[경로지정]"],
      	"paths": {
          "@modules/.*" [
            "./modules/app/*"
          ]
      	}
    },
    "include": [
        "./src/**/*"
    ]
}
```

**compileOnSave**

- 파일을 저장하면 자동으로 컴파일, 최상단에 설정해야 함, 특정 IDE를 사용해야만 함.(Visual Studio 2015 이상, atom-typescript 플러그인)

**extends**

- tsconfig.json을 상속받아 사용할 수 있음.

**compilerOptions**

- target : 어떤 JS형태로 컴파일 할 것인가(default : es3)

- lib : 기본 type definition 라이브러리를 어떤 것을 사용할 것이냐.

  default는 target에 맞는 해당 lib를 사용.

> ex) target이  'es5'면, 디폴트로 dom, es5, scriptshot를 사용

- typeRoots/types : type definition의 위치를 설정.(typescript 2.0 이상)

  default 는 ./node_modules/@types/ 안의 모듈이름에서 찾아옴.

> type definition 코드 : typesciprt의 타입정보가 들어 있음, JS에서 자동완성, 빨간줄 제거를 도와줌, 과거에는 tsd나 typing을 통해 d.ts 파일을 불러왔으나, @types 가 생긴 뒤로 모듈에 d.ts파일을 함께 배포하는 추세. ex) index.d.ts

- outDir : 컴파일 된 파일이 위치하는 장소
- sourceMap : 소스맵 사용 여부

> 소스맵이란 : 자바스크립트를 한 파일로 합치거나 사이즈를 줄이기 위해서 압축하거나 난독화해서 배포하는 방식을 많이 취하는데 이 방법은 성능에는 좋지만 사실 디버깅이 어려워지는 문제가 있다. 소스맵은 이 원본 소스와 최종소스를 매핑해서 추적할 수 있는 방법

- noImplicityAny : `any` 로 선언된 식과 정의에 대해 에러를 발생시킴.

- module : 결과물로 어떤 모듈 표준을 사용할 것인가.

  디폴트 : target : es6이면 es6가 디폴트, es6아니면 commonjs 디폴트, AMD나 System을 사용하려면, outFile이 지정돼야함.

- moduleResolution : 모듈 가져올때 ts소스에서 모듈을 사용하는 방식. node/classic이 있고 node = commonjs일때.

- paths/baseUrl/rootDirs : 상대경로 방식이 아닐때 사용.

  `import {Sample} from '@modules/sample.service'` 와 같이 paths에 지정한 key를 입력하여 사용.

- importHelpers : ts파일을 js 파일로 바꾸면, helper 함수들이 js 에 생기는데 파일이 길어지니 따로 모듈로 빼서 사용.

- removeComments : 컴파일시 주석을 빼줌

  ​

**files, include, exclude**

컴파일 대상인 파일 설정, 셋다 설정이 없으면 전부다 컴파일

- `files` 키워드로 파일을 정함.   ` "files": [  "src/index.ts"]`

- `incude/exclude`로 경로를 지정할 수 있다.

- `exclude` 

  설정 안하면 4가지 node_modules, bower_components, jspm_package, <outDir> 을 default로 제외

  <outDir>은 항상 제외(include에 있어도)

우선순위 : files > exclude > include



> 더 자세한 옵션 값을 보려면
>
> http://json.schemastore.org/tsconfig
>
> https://www.typescriptlang.org/docs/handbook/compiler-options.html



## Refrence

https://www.typescriptlang.org/docs/handbook/react-&-webpack.html

https://www.inflearn.com/course/%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EC%BD%94%EB%A6%AC%EC%95%84-1705-%EA%B8%B0%EC%B4%88-%EC%84%B8%EB%AF%B8%EB%82%98/

baseUrl, types : http://blog.naver.com/PostView.nhn?blogId=yjw1250&logNo=220903209853&parentCategoryNo=&categoryNo=&viewDate=&isShowPopularPosts=false&from=postView

