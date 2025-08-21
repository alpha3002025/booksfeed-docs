# 데이터 영역, 서비스 영역에 대한 경계 정의
- booksfeed-member-svc 를 예로 들어 정리합니다.

데이터 영역에 대한 정의
- MemberRepository, FollowRepository 는 쿼리를 수행하는 역할만을 담당하며, 비지니스로직까지는 담당하지 않음
- 데이터를 조회하는 단위기능 역할만을 수행

서비스 영역에 대한 정의
- 각각의 서비스에서 다른 서비스를 조합하는 것은 가급적 지양
- 지나친 공통화에 대한 욕심으로 인해 도메인 경계를 침범하는 것에 대해 주의
- 순환 참조가 발생할 가능성을 가급적 배제
- e.g. FollowService 에서 MemberService 를 불러오는 것은 지양
- e.g. MemberService 에서 FollowService 를 불러오는 것은 지양
- e.g. FollowService 에서 MemberService 가 꼭 필요하게 될 경우에는?
    - FollowApplicationService 를 만들어서 FollowService, MemberService 를 각각 조합해서 사용
    - 가급적 이런 일은 발생하지 않았으면...
<br/>