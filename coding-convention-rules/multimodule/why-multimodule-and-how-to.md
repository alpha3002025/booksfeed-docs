# 왜 멀티모듈을 적용했는지?
중복된 코드를 공통화 해야 하는 코드가 있어서 멀티모듈을 적용하기로 결정했습니다. 멀티모듈 적용 전에는 굳이 해야하나? 했는데 `enum` 이나 JWT 내에서 memberId 를 가져오는 로직 등은 모든 서비스(member,feed,timeline,notification)에 일관되게 적용해야 했기에 멀티 모듈을 도입했습니다.<br/>

jwt 관련해서 뭔가를 고칠때 member,feed,timeline,notification 을 돌아가면서 고친걸 붙여넣어줘야 하기에 오히려 시간을 단축하기 위해 결국은... 멀티모듈이라는 칼을 들게 되었고, 최소한도 내에서 멀티모듈을 구성하자고 다짐하게 됐습니다.<br/>
<br/>

# 멀티 모듈 구성
루트 디렉터리는 `-svc` 로 끝나는 디렉터리이며 서브 모듈들은 각각의 repository 를 가진 git submodule 들입니다. 이 서브 모듈들은 gradle 프로젝트이거나 infrastructure 모듈입니다.<br/>
각각의 멀티모듈은 각각 git repository 로 관뢰되며 submodule 로 추가하거나 뺄 수 있습니다.<br/>
<br/>

## booksfeed-member-svc
예를 들어 회원 서비스의 경우 다음과 같은 프로젝트 구조입니다.
```plain
/booksfeed-member-svc
  /booksfeed-member ## 회원 로직, git submodule
  /booksfeed-jwt  ## jwt 로직, git submodule
  /booksfeed-code ## enum,response,response code 등의 로직, git submodule
  /booksfeed-infrastructure ## docker, helm, sql 등을 관리, git submodule
```
이 중 booksfeed-infrastructure 는 java 디렉터리는 아니며 gradle 서브모듈로는 추가되지 않습니다.
<br/>

## booksfeed-feed-svc
예를 들어 feed 서비스의 경우 다음과 같은 프로젝트 구조입니다. feed 는 member-svc 와 API 통신을 수행합니다.
```plain
/booksfeed-feed-svc
  /booksfeed-feed ## 회원 로직, git submodule
  /booksfeed-jwt  ## jwt 로직, git submodule
  /booksfeed-code ## enum,response,response code 등의 로직, git submodule
  /booksfeed-infrastructure ## docker, helm, sql 등을 관리, git submodule
```
이 중 booksfeed-infrastructure 는 java 디렉터리는 아니며 gradle 서브모듈로는 추가되지 않습니다.
<br/>

## booksfeed-timeline-svc
timeline 서비스의 경우 다음과 같은 프로젝트 구조입니다. timeline 서비스는 member-svc 와 API 통신을 수행합니다. feed 에서 글,댓글,대댓글 에 대한 관리를 수행한다면, timeline 은 사용자의 타임라인을 보여주는 역할만을 전담하는 프로젝트입니다.
```plain
/booksfeed-timeline-svc
  /booksfeed-timeline ## 회원 로직, git submodule
  /booksfeed-jwt  ## jwt 로직, git submodule
  /booksfeed-code ## enum,response,response code 등의 로직, git submodule
  /booksfeed-infrastructure ## docker, helm, sql 등을 관리, git submodule
```
이 중 booksfeed-infrastructure 는 java 디렉터리는 아니며 gradle 서브모듈로는 추가되지 않습니다.
<br/>

