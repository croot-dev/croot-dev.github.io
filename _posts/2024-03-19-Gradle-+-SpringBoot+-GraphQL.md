---
layout: post
title: "Gradle + SpringBoot+ GraphQL"
created: 2024-03-19
edited: 2024-04-22
category: [기술]
tags: [SpringBoot:rgb(238 224 218):rgb(68 42 30),Gradle:rgb(245 224 233):rgb(76 35 55),VSCode:rgb(245 224 233):rgb(76 35 55),MariaDB:rgb(232 222 238):rgb(65 36 84),Java:rgb(250 222 201):rgb(73 41 14),GraphQL:rgb(211 229 239):rgb(24 51 71)]
---


<div class="callout" style="display:flex;width:100%;border-radius:4px;background:rgb(241,241,239);padding: 16px 16px 16px 12px;">
<div style="display:flex;align-items:center;justify-content:center;height:24px;width:24px;border-radius:0.25em;flex-shrink:0;">💡</div>
<div style="white-space:pre-wrap;word-break:break-word;caret-color:rgb(55, 53, 47);margin-left:8px;padding-left:2px;padding-right:2px;">전체 소스 코드는 <a href="https://github.com/croot-dev/springboot-graphql/tree/notion/6c50e956396b41449b42a8e134d97c4e" target="_blank">여기</a>를 참고 해주세요.</div>
</div>


## 프로젝트 환경

- VSCode 1.86.0
- Gradle 8.6
- SpringBoot 3.2.3
- java 17
- mariaDB (feat. dbeaver)

## 설치


### Database

1. DBeaver 설치
2. “achro” 이름의 Database 생성 (본 예제는 MariaDB 기본 설정 이용)

### JDK 17 설치


[https://www.oracle.com/java/technologies/javase/jdk17-archive-downloads.html](https://www.oracle.com/java/technologies/javase/jdk17-archive-downloads.html)


### VSCode Extensions

- [Extension Pack for Java ](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-pack)`vscjava.vscode-java-pack`
- [Gradle for Java](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-gradle) `vscjava.vscode-gradle`
- [Spring Boot Tools](https://marketplace.visualstudio.com/items?itemName=vmware.vscode-spring-boot) `vmware.vscode-spring-boot`
- [Spring Initializr Java Support ](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-spring-initializr)`vscjava.vscode-spring-initializr`

### VSCode settings


```json
// ~/.vscode/settings.json

{
    "java.compile.nullAnalysis.mode": "automatic",
    "java.configuration.updateBuildConfiguration": "automatic"
}
```


## 프로젝트 생성

1. `Ctrl + Shift + P` or `Command + Shift + P`
2. `Java: Create Java Project…` 선택
3. Select the project type ⇒ Spring Boot
4. Select prject type. ⇒ Gradle project
5. Specify Spring Boot version ⇒ 3.2.3
6. Specify project language. ⇒ Java
7. Input Group Id for your project. ⇒ com.croot
8. Input Artifact Id for your project. ⇒ achro
9. Specify packaging type. ⇒ Jar
10. Specify Java version ⇒ 17
11. Search dependencies. ⇒ none

### Gradle 설정


`~/build.gradle`


```javascript
// build.gradle

plugins {
	id 'java'
	id 'org.springframework.boot' version '3.2.3'
	id 'io.spring.dependency-management' version '1.1.4'
}

java {
	group = 'com.achro'
	version = '0.0.1-SNAPSHOT'
	sourceCompatibility = '17'
}

repositories {
    mavenCentral()
}

dependencies {
	// Spring Boot
	implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
    implementation 'org.springframework.boot:spring-boot-starter-web'
	implementation 'com.graphql-java-kickstart:graphql-spring-boot-starter:15.0.0'
    testImplementation 'com.graphql-java-kickstart:graphql-spring-boot-starter-test:15.0.0'
	compileOnly 'org.projectlombok:lombok'
	annotationProcessor 'org.projectlombok:lombok'

	// DB
	runtimeOnly 'org.mariadb.jdbc:mariadb-java-client:3.3.3'
}
```


> **Gradle 의존성 관리 속성**

	- `complieClasspath` : 프로젝트 소스 코드 컴파일 시 필요한 모든 클래스, 라이브러리
	- `runtimeClasspath` : 프로젝트 실행 시 필요한 모든 클래스, 라이브러리
	- `runtimeOnly` : 런타임 시점에만 필요할 경우
	- `compileOnly` : 컴파일 시점에만 필요할 경우
	- `implementation` : 컴파일 및 런타임 모두 필요

### 프로젝트 설정


`~/src/main/resources/application.properties`


```yaml
# application.properties

server.port=8000
spring.application.name=achro

graphql.servlet.enabled=true
graphql.altair.enabled=true
graphql.playground.enabled=true


# DB Properties
spring.jpa.hibernate.ddl-auto=create
spring.datasource.driverClassName=org.mariadb.jdbc.Driver
spring.datasource.url=jdbc:mariadb://localhost:3306/achro
spring.datasource.username=root
spring.datasource.password=

```


`~/src/main/resources/graphql/schema.graphqls`


```java
// schema.graphqls

schema{
    query: Query
}

type Member{
    id: ID!
    name: String!
    age: Int!
}

type Query{
    getMember(id: ID!): Member
    getAllMembers: [Member]
}

```


### 디렉토리 구조 및 소스코드


```markdown
~/src/main/java/com/croot/achro
|- model/
|- repository/
|- service/
`- ApiApplication.java
```


**model/**

<details>
<summary>`Member.java`</summary>

```java
// ~/src/main/java/com/croot/achro/model/Member.java

package com.croot.achro.model;

import lombok.Getter;
import lombok.Setter;

import jakarta.persistence.*;

@Entity
@Table(name="MEMBER")
@Getter
@Setter
public class Member {
    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    @Column(name = "MEMBER_ID")
    private Integer id;
    @Column(name = "MEMBER_NAME")
    private String name;
    @Column(name = "MEMBER_AGE")
    private Integer age;
}

```


</details>


**repository/**

<details>
<summary>`MemberRepository.java`</summary>

```java
// ~/src/main/java/com/croot/achro/repository/MemberRepository.java

package com.croot.achro.repository;

import com.croot.achro.model.Member;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

@Repository
public interface MemberRepository extends JpaRepository<Member, Integer> {
}

```


</details>


**service/**

<details>
<summary>`MemberService.java`</summary>

```java
// ~/src/main/java/com/croot/achro/service/MemberService.java

package com.croot.achro.service;

import graphql.kickstart.tools.GraphQLQueryResolver;

import java.util.List;

import org.springframework.stereotype.Service;

import com.croot.achro.model.Member;
import com.croot.achro.repository.MemberRepository;

@Service
public class MemberService implements GraphQLQueryResolver {
    private final MemberRepository memberRepository;

    public MemberService(MemberRepository memberRepository) {
        this.memberRepository = memberRepository;
    }
    public List<Member> getAllMembers(){
        return memberRepository.findAll();
    }
    public Member getMember(Integer id){
        return memberRepository.findById(id).get();
    }

}
```


</details>

<details>
<summary>`LoadDataService.java` : 초기 데이터를 넣기 위한 서비스</summary>

```java
// ~/src/main/java/com/croot/achro/service/LoadDataService.java

package com.croot.achro.service;

import com.croot.achro.model.Member;
import com.croot.achro.repository.MemberRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import jakarta.annotation.PostConstruct;

@Service
public class DataLoaderService {

    @Autowired
    private MemberRepository memberRepository;

    @PostConstruct
    public void load() {
        // 데이터 추가
        for (int i = 1; i <= 10; i++) {
            Member member = new Member();
            member.setName("Member " + i); // Member 이름 설정
            member.setAge(i * 10); // Member 나이 설정
            memberRepository.save(member);
        }
    }
}
```


</details>


## 실행

1. [http://localhost:8000/playground](http://localhost:8000/playground) 접속
2. 쿼리 작성

	```graphql
	query {
	  getAllMembers{
	    id
	    name
	    age
	  }
	}
	```

3. 결과 확인

	![0](/assets/img/2024-03-19-Gradle-+-SpringBoot+-GraphQL.md/0.png)_Untitled.png_


## REFS.


GraphQL Java Kickstart Getting started - [https://www.graphql-java-kickstart.com/spring-boot/getting-started/](https://www.graphql-java-kickstart.com/spring-boot/getting-started/)


## ERROR.LOG


**Prob.**


VSCode에서 `build.gradle` 수정


**Solv.**

1. 해당 파일 우클릭
2. “Reload Projects” 를 선택 해주어야한다.

**Prob.**


Run Java 시 아래와 같은 에러 발생.


```javascript
Type definition for root mutation type 'Mutation' not found!
```


**Solv.**


`~/src/main/resources/graphql/schema.graphqls` 에 “Mutation”을 정의했으나 리졸버(Resolver)가 없어서 발생.

1. schema.graphqls 수정 or Resolver 작성
