
# 프롬프트
프롬프트는 다음과 같이 작성했었습니다.

```plain
Role
당신은 Git, Spring Boot 전문가입니다.


Context
모듈이 다음과 같이 존재합니다.

자바 모듈
booksfeed-code
booksfeed-jwt

서비스 모듈
booksfeed-member
booksfeed-feed
booksfeed-timeline

위 모듈들은 현재는 booksfeed-member-svc 내에 다음과 같은 디렉터리 구조로 구성되어 있습니다.
/booksfeed-member-svc
  /booksfeed-code
  /booksfeed-jwt
  /booksfeed-member

만약 booksfeed-feed-svc 를 개발한다면 다음과 같은 디렉터리 구조로 구성합니다.
/booksfeed-feed-svc
  /booksfeed-code
  /booksfeed-jwt
  /booksfeed-feed


Task
- booksfeed-member-svc 는 root 디렉터리이며 settings.gradle.kts 등과 같은 파일들을  git 을 통해 관리합니다.
- booksfeed-code, booksfeed-jwt, booksfeed-member 각각은 개별 repository 로 구성되게끔 git 환경을 구성합니다.
- booksfeed-code, booksfeed-jwt 는 이미 git repository 로 구성되어 있는 것을 clone 받은 것이라고 가정합니다.
```

# 응답
---
# Git Submodule을 이용한 멀티모듈 프로젝트 구성

## 1. 초기 설정

### 1.1 Root Repository 초기화
```bash
# booksfeed-member-svc 디렉터리 생성 및 초기화
mkdir booksfeed-member-svc
cd booksfeed-member-svc
git init
```

### 1.2 Root 프로젝트 파일 생성
```bash
# Gradle 설정 파일들 생성
touch settings.gradle.kts
touch build.gradle.kts
touch gradle.properties
```

## 2. Submodule 추가

### 2.1 기존 Repository들을 Submodule로 추가
```bash
# booksfeed-code submodule 추가
git submodule add <booksfeed-code-repository-url> booksfeed-code

# booksfeed-jwt submodule 추가  
git submodule add <booksfeed-jwt-repository-url> booksfeed-jwt

# booksfeed-member submodule 추가 (새로 생성한 경우)
git submodule add <booksfeed-member-repository-url> booksfeed-member
```

### 2.2 Submodule 초기화 및 업데이트
```bash
git submodule init
git submodule update
```

## 3. 설정 파일 구성

### 3.1 settings.gradle.kts
```kotlin
rootProject.name = "booksfeed-member-svc"

include(
    "booksfeed-code",
    "booksfeed-jwt", 
    "booksfeed-member"
)
```

### 3.2 Root build.gradle.kts
```kotlin
plugins {
    java
    id("org.springframework.boot") version "3.2.0" apply false
    id("io.spring.dependency-management") version "1.1.4" apply false
}

allprojects {
    group = "com.booksfeed"
    version = "1.0.0"
    
    repositories {
        mavenCentral()
    }
}

subprojects {
    apply(plugin = "java")
    apply(plugin = "org.springframework.boot")
    apply(plugin = "io.spring.dependency-management")
    
    java {
        sourceCompatibility = JavaVersion.VERSION_17
        targetCompatibility = JavaVersion.VERSION_17
    }
    
    dependencies {
        implementation("org.springframework.boot:spring-boot-starter")
        testImplementation("org.springframework.boot:spring-boot-starter-test")
    }
}
```

## 4. 개별 모듈 Repository 구성

### 4.1 booksfeed-member 모듈의 build.gradle.kts 예시
```kotlin
dependencies {
    implementation(project(":booksfeed-code"))
    implementation(project(":booksfeed-jwt"))
    implementation("org.springframework.boot:spring-boot-starter-web")
    implementation("org.springframework.boot:spring-boot-starter-data-jpa")
    // 기타 의존성들...
}
```

## 5. Git 작업 흐름

### 5.1 일반적인 개발 작업
```bash
# 특정 submodule에서 작업
cd booksfeed-member
git checkout main
# 코드 수정 후
git add .
git commit -m "feat: add new feature"
git push origin main

# Root 프로젝트로 돌아와서
cd ..
git add booksfeed-member
git commit -m "update: booksfeed-member submodule"
git push origin main
```

### 5.2 Submodule 업데이트
```bash
# 모든 submodule을 최신 상태로 업데이트
git submodule update --remote

# 특정 submodule만 업데이트
git submodule update --remote booksfeed-code
```

### 5.3 새로운 환경에서 클론
```bash
# Submodule과 함께 클론
git clone --recurse-submodules <booksfeed-member-svc-repository-url>

# 또는 클론 후 submodule 초기화
git clone <booksfeed-member-svc-repository-url>
cd booksfeed-member-svc
git submodule init
git submodule update
```

## 6. booksfeed-feed-svc 구성 시

### 6.1 새로운 서비스 프로젝트 구성
```bash
# 새 프로젝트 생성
mkdir booksfeed-feed-svc
cd booksfeed-feed-svc
git init

# 기존 공통 모듈들을 submodule로 추가
git submodule add <booksfeed-code-repository-url> booksfeed-code
git submodule add <booksfeed-jwt-repository-url> booksfeed-jwt
git submodule add <booksfeed-feed-repository-url> booksfeed-feed
```

### 6.2 settings.gradle.kts
```kotlin
rootProject.name = "booksfeed-feed-svc"

include(
    "booksfeed-code",
    "booksfeed-jwt",
    "booksfeed-feed"
)
```

## 7. .gitignore 설정

### 7.1 Root 프로젝트 .gitignore
```gitignore
# Gradle
.gradle/
build/
!gradle/wrapper/gradle-wrapper.jar

# IDE
.idea/
*.iml
*.ipr
*.iws

# OS
.DS_Store
Thumbs.db
```

## 8. 주의사항

### 8.1 Submodule 작업 시 주의점
- Submodule 내에서 작업할 때는 반드시 브랜치를 체크아웃해야 함 (detached HEAD 상태 방지)
- Submodule 변경사항을 커밋한 후, 부모 프로젝트에서도 submodule 참조를 업데이트해야 함
- 팀원들과 submodule 버전 동기화 필요

### 8.2 대안 방법들
- **Git Subtree**: Submodule보다 단순하지만 히스토리가 복잡해질 수 있음
- **Gradle Composite Builds**: 로컬 개발 시 유용하지만 버전 관리 측면에서는 submodule이 더 적합
- **별도 Repository + 의존성 관리**: 각 모듈을 별도 artifact로 배포하여 의존성으로 관리

## 9. 자동화 스크립트

### 9.1 submodule 일괄 업데이트 스크립트
```bash
#!/bin/bash
# update-submodules.sh

echo "Updating all submodules..."
git submodule foreach --recursive git pull origin main
git add .
git commit -m "update: refresh all submodules"
git push origin main
echo "All submodules updated!"
```

이러한 구조를 통해 공통 모듈(booksfeed-code, booksfeed-jwt)을 여러 서비스에서 재사용하면서도 각각을 독립적인 repository로 관리할 수 있습니다.