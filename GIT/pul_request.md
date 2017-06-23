# Git Pull Request 방법 정리

1. 저장소에서 fork 버튼을 눌러, 개인 repository에 포크 한다.
2. 로컬 저장소와 원격 저장소 추가

- 로컬 저장소를 클론

> $ git clone fork깃주소 폴더명

- 로컬 저장소에 원격저장소[원본 프로젝트] 추가

> $ git remote add upstream 원본깃주소

*원본 저장소의 별칭은 보통 upstream을 사용, 다른 별칭 사용해도 무관*

3. 작업은 매번 Branch를 만들어서 Branch 단위로 한다.

- 브랜치 생성

> $ git branch [브랜치명]

- 브랜치 이동

> $ git checkout [브랜치명]

4. 작업 내역 커밋

- 인덱스 확인, 등록, 커밋

> $ git status
>
> $ git add .
>
> $ git commit -m '[메세지]'

5. 원본 저장소 내역 업데이트

- 원본 저장소의 소스를 로컬 저장소로 가져옴.

> $ git fetch upstream

- 커밋 내역을 합침

> $ git rebase upstream/master

6. Pull Request 보내기

- push 하기

> $ git push origin [브랜치명]

- fork 한 저장소에서 pull request 작성

github 포크한 저장소로 이동후, `Crete Pull Request`를 눌러 변경 사항 확인 및 pull request 메세지를 작성한다.

7. 원본 저장소에서 변경사항 Merge 하기

- 원본 저장소의 pull request 메뉴에서 `Merge` 버튼을 눌러 변경사항 Merge

Or

- 직접 명령어 입력

> $ git checkout master
>
> $ git remote add [기여자별칭] 기여자깃주소
>
> $ git fetch [기여자별칭]
>
> $ git rebase [기여자별칭/브랜치명]
>
> $ git push origin master



*그외 명령어*

- 리모트 저장소 목록 확인

> $ git remote -v

- 리모트 저장소 삭제

>  $ git remote rm [별칭 이름]



## Reference

http://dogfeet.github.io/articles/2012/how-to-github.html

https://www.slideshare.net/mobile/jungseobshin/github-pull-request