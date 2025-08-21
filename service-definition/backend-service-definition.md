# `booksfeed-member` : 사용자 기능, 팔로우/언팔로우 기능
- 사용자 기능, 팔로우/언팔로우 기능 까지만 구현하기로 결정
- 회원가입, 로그인, 로그아웃 기능의 경우 구현 범위에서 배제 (추후 시간이 된다면 추가 예정)

# `booksfeed-feed` : 글, 댓글, 대댓글 기능
- Member 서비스 내에서 댓글 기능도 구현하려 했으나, 댓글 서비스의 경우 User의 정보만 API로 조회해오고, 댓글,대댓글에 대한 처리만 담당하기로 결정
- 즉, 댓글, 대댓글은 `booksfeed-feed` 서비스로 분리

# `boosfeed-timeline` : timeline 서비스
- feed 서비스가 타임라인을 담당하는 것 같지만, 특정 유저의 팔로우한 사용자에 대한 timeline 자체를 들고오는 것은 timeline 서비스에서 구현하기로 함

# `booksfeed-notification` : 알림 서비스
- spring batch 를 사용하려고 했지만, 가급적 kubernetes 의 cronjob 을 예제로 사용해보기로 함
- `booksfeed-notification` 이라는 서비스로 정의

<br/>