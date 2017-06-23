# Merge vs Rebase

merge를 하면 branch를 생성한 시점의 베이스를 기준으로 합병이 된다

rebase는 `git rebase 브랜치` 에서 지정한 브랜치를 베이스로 기준 삼아 합병이 된다

## merge

`master` 와 `slave`가 있고 `master` 에서 `git merge slave` 를 하면

1. `master`의 커밋 내역 이후 `slave`의 커밋내역이 단순 합쳐짐.
2. 커밋을 묶음으로 관리 : 마지막 `merge` 커밋을 `reset` 하면 통째로 모든 `merge `커밋들이 사라짐.

## rebase

- `master` 에서 `git rebase slave`를 하면

  슬레이브 뒤에 마스터의 커밋 이력이 쌓임.


-  `slave` 에서 `git rebase master`를 하면

  마스터 커밋 뒤로 슬레이브 이력이 쌓임.

1. 트리 그래프도 브랜치 없이 일렬로 나타남.
2. 중복된 수정 내용은 로그가 남지 않음.
3. reset시 통째로 merge 커밋들이 사라지지 않음.

## rebase 과정

1. 커밋로그 보기

`git log --decorate --all --oneline --graph`

2. 데브의 마지막 커밋을 베이스로 머지함

`git rebase dev`

3. conflict 수정 후

`git add 파일 전체경로`

4. 상태 확인

`git status`

5. 리베이스 계속

`git rebase --continue`



## Reference

[http://dogfeet.github.io/articles/2012/git-merge-rebase.html](http://dogfeet.github.io/articles/2012/git-merge-rebase.html)

[https://blog.outsider.ne.kr/666](https://blog.outsider.ne.kr/666)
