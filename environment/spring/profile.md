# Run/Debug
## Profile = local
- application-local.yaml

### Database
- docker 기반의 mysql 에서 진행
- docker-compose : `/infra/mysql/docker/docker-compose.yaml`
- mysql : `localhost:23306`
- mysql user : `root`
- mysql password : `test1357`


## dev
### Database
- aws rds
### kubernetes
- EKS
- telepresence

## prod
- 미운영

# TEST
## Profile = `mysql-test`
docker 기반으로 테스트를 수행 (멱등성 연산)

### Database
- docker-compose : `src/test/resources/docker/mysql-test`
- mysql : `localhost:33306`
- mysql user : `root`
- mysql password : `test1357`


## Profile = `mysql-local`
docker 기반으토 테스트 수행 (실제 데이터 결과 확인 용도)

### Database
- docker-compose : `src/test/resources/docker/mysql-test`
- mysql : `localhost:33306`
- mysql user : `root`
- mysql password : `test1357`



