# 새로운 리포지터리를 서브모듈로 등록하기
예를 들어 `booksfeed-code` 라는 리포지터리를 모두 생성했고, 코드도 미리 작성되어 있다는 가정을 합니다. 
- 실제 코드 작성후 별도의 디렉터리에 복사해둔 후 새로운 리포지터리에 push 해둔 상황
- 또는 이미 개발되어 있는 서브 모듈이 별도의 git repository 에 push 해둔 상황

<br/>

이 경우 다음과 같이 추가할 수 있습니다.
```bash
## 서브모듈 등록
$ git submodule add https://github.com/alpha3002025/booksfeed-code.git booksfeed-code
```
<br/>

그런데 다음과 같은 에러들이 날 수 있습니다.
```bash
## 현재 디렉터리에서 작업하다가 mv 명령으로 외부로 옮겨뒀을때
$ git submodule add https://github.com/alpha3002025/booksfeed-code.git booksfeed-code
fatal: 'booksfeed-code' already exists in the index
```
<br/>

위 에러를 해결하기 위해 캐시를 삭제할때 다음과 같은 에러가 날 수 있기도 합니다.
```bash
$ git rm --cached booksfeed-code
fatal: not removing 'booksfeed-code' recursively without -r
```
<br/>

이 경우 다음과 같이 수행해줍니다.
```bash
## 캐시 삭제
git rm --cached -r booksfeed-code

## 서브모듈 등록
git submodule add https://github.com/alpha3002025/booksfeed-code.git booksfeed-code
```
<br/>
