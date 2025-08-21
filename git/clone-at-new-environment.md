# 새로운 환경에서 clone 받을때
새로운 환경에서 clone 받을때 두 가지 방법으로 환경 구성이 가능합니다.
- 1\. `--recurse-submodules` 를 통해 한번에 재귀적으로 모든 서브모듈들을 clone
- 2\. clone 후 수동으로 모든 submodule 초기화


# 1. `--recurse-submodules` 를 이용해 재귀적으로 submodule 들을 clone
```bash
git clone --recurse-submodules <booksfeed-member-svc-repository-url>
```
<br/>

# 2. clone 후 수동으로 모든 submodule 초기화
```bash
git clone <booksfeed-member-svc-repository-url>
cd booksfeed-member-svc
git submodule init
git submodule update
```
<br/>

