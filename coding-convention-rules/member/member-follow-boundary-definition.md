# Member 영역과 Follow 영역간의 경계 정의
Member 도메인 경계 영역 정의
- 사용자 id, 사용자 이름, 사용자 email 등 사용자에 관련된 정보만을 전담해서 조회/수정/업데이트 하는 역할
- MemberRepository 를 사용해 사용자에 관련된 정보들을 가져옴

Follow 도메인 경계 영역 정의
- 특정 사용자가 어떤 사용자를 팔로우/언팔로우 하는 기능
- MemberRepository (접근 가능) : Follow 도메인에서 Follow 시 필요한 회원ID 등을 위해 MemberRepository 를 사용하는 것은 가능
- FollowRepository (접근 가능) : Follow 도메인에서 특정 사용자와 다른 사용자간 팔로우 관계 조회/팔로우/언팔로우 및 특정사용자의 팔로잉/팔로우 리스트 조회 기능을 수행하는 것은 가능
