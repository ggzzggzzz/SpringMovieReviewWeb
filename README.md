# 🎬 영화 리뷰 웹사이트 (Movie Review Site)

## 📖 프로젝트 소개

Spring Framework 기반으로 개발된 영화 정보 제공 및 리뷰 커뮤니티 웹사이트입니다. 사용자들은 다양한 영화 정보를 탐색하고, 직접 리뷰와 평점을 남기며 다른 사용자들과 소통할 수 있습니다.

<br>

 프로젝트의 실제 동작 스크린샷

![Project Screenshot](./project_screenshot.png)

<br>

## ✨ 주요 기능

-   **사용자 관리**:
    -   일반 회원가입 및 로그인/로그아웃 기능
    -   카카오 API를 연동한 소셜 로그인 기능
-   **영화 정보**:
    -   영화 목록 조회 (최신순, 평점순 등)
    -   영화 상세 정보 조회 (줄거리, 배우, 장르, 평점 등)
    -   영화 검색 기능
-   **리뷰 & 평점**:
    -   영화에 대한 리뷰 작성, 조회, 수정, 삭제 (CRUD)
    -   별점을 이용한 평점 등록 기능
-   **위시리스트**:
    -   관심 있는 영화를 위시리스트에 추가 및 관리하는 기능
-   **관리자 페이지**:
    -   영화, 회원, 리뷰 등 데이터를 관리할 수 있는 관리자 전용 기능

<br>

## 🛠️ 기술 스택

### Backend
-   Java 21
-   Spring MVC 6.2.7
-   Spring Security 6.2.4
-   MyBatis 3.5.19
-   Oracle Database (ojdbc10)
-   Apache Commons DBCP (DB Connection Pool)
-   Jackson (JSON 데이터 처리)
-   Logback (로깅)

### Frontend
-   JSP (JavaServer Pages) & JSTL
-   HTML5 / CSS3
-   JavaScript
-   jQuery

### Build & Server
-   Apache Maven
-   Apache Tomcat

<br>

## 🏛️ 아키텍처

이 프로젝트는 **MVC (Model-View-Controller) 패턴**을 기반으로 하는 계층형 아키텍처(Layered Architecture)를 따릅니다.

```
  Client (Web Browser)
        |
        v
DispatcherServlet (Spring Front Controller)
        |
        v
+-------------------+      +------------------+      +-----------------+
|    Controller     | <--> |      Service     | <--> |       DAO       |
| (요청/응답 처리)  |      | (비즈니스 로직)  |      | (DB 연동, MyBatis)|
+-------------------+      +------------------+      +-----------------+
        ^                            ^                       ^
        |                            |                       |
        +----------------------------+-----------------------+
                                     |
                                     v
                             +----------------+
                             |   Database     |
                             |    (Oracle)    |
                             +----------------+
```
-   **Controller**: 사용자의 HTTP 요청을 받아 비즈니스 로GIC 처리를 위해 Service 계층으로 전달하고, 처리 결과를 View에 렌더링합니다.
-   **Service**: `@Transactional`을 통해 트랜잭션을 관리하며, 여러 DAO를 조합하여 핵심 비즈니스 로직을 수행합니다.
-   **DAO (Mapper Interface)**: MyBatis와 연동하여 데이터베이스에 직접 접근(CRUD)하는 역할을 담당합니다.
-   **VO (Value Object)**: 계층 간 데이터 전송을 위한 객체입니다.

<br>

## ⚙️ 주요 설정 파일 (XML Configuration)

이 프로젝트는 XML 기반의 설정을 통해 Spring Framework와 여러 라이브러리를 연동합니다. 각 설정 파일의 역할은 다음과 같습니다.

### 1. `pom.xml`
-   **역할**: Maven 프로젝트의 모든 설정을 관리하는 핵심 파일입니다.
-   **주요 설정**:
    -   `<dependencies>`: Spring MVC, Spring Security, MyBatis, Oracle JDBC 드라이버 등 프로젝트에 필요한 모든 라이브러리 의존성을 정의합니다.
    -   `<properties>`: Java 컴파일러 버전(21), 소스 ��코딩(UTF-8) 등 프로젝트 전반에 사용될 속성을 설정합니다.
    -   `<build>`: Maven 빌드 과정에서 사용될 플러그인(compiler, war)을 설정합니다.

### 2. `web.xml`
-   **역할**: 웹 애플리케이션의 배포 서술자(Deployment Descriptor)로, WAS(Tomcat)가 처음으로 로드하는 설정 파일입니다.
-   **주요 설정**:
    -   **`DispatcherServlet`**: Spring MVC의 프론트 컨트롤러인 `DispatcherServlet`을 등록합니다. 모든 웹 요청(`<url-pattern>/</url-pattern>`)을 가장 먼저 받아 해당 요청을 처리할 컨트롤러에게 분배하는 역할을 합니다.
    -   **`contextConfigLocation`**: `DispatcherServlet`이 사용할 Spring 설정 파일(`servlet-context.xml`)의 위치를 지정합니다.
    -   **`CharacterEncodingFilter`**: 모든 요청과 응답에 대해 UTF-8 인코딩을 강제하여 한글 깨짐 문제를 방지합니다.

### 3. `servlet-context.xml`
-   **역할**: Spring의 웹 계층(Web Layer)에 관련된 설정을 담당합니다. 주로 컨트롤러, 뷰 리졸버, 정적 자원 매핑 등을 정의합니다.
-   **주요 설정**:
    -   **`<context:component-scan>`**: `kr.ac.kopo` 패키지 내의 `@Controller`, `@Service` 등 스테레오타입 어노테이션이 붙은 클래스를 스캔하여 Spring Bean��로 등록합니다.
    -   **`<mvc:annotation-driven>`**: `@RequestMapping`, `@RequestParam` 등 어노테이션 기반의 Spring MVC 기능을 활성화합니다.
    -   **`InternalResourceViewResolver`**: 컨트롤러가 반환하는 뷰 이름(String)을 실제 JSP 파일 경로로 변환합니다. (`prefix`와 `suffix`를 조합하여 `/WEB-INF/views/{viewName}.jsp`와 같은 경로를 완성)
    -   **`<mvc:resources>`**: CSS, JavaScript, 이미지 파일 등 정적 자원의 경로를 매핑하여 `DispatcherServlet`을 거치지 않고 바로 접근할 수 있게 합니다.
    -   **`<import>`**: 데이터베이스 및 비즈니스 로직 관련 설정이 정의된 `context-mybatis.xml` 파일을 불러옵니다.

### 4. `context-mybatis.xml`
-   **역할**: 데이터베이스 연결 및 MyBatis 연동 등 비즈니스 로직(Business Layer)과 데이터 접근(Data Access Layer)에 관련된 설정을 담당합니다.
-   **주요 설정**:
    -   **`BasicDataSource`**: Apache Commons DBCP를 사용하여 데이터베이스 커넥션 풀(Connection Pool)을 설정합니다. DB 연결 정보(Driver, URL, Username, Password)를 포함합니다.
    -   **`SqlSessionFactoryBean`**: MyBatis와 Spring을 연동하는 핵심 객체입니다. `DataSource`를 참조하고, MyBatis 설정 파일(`mybatis-config.xml`)과 SQL 쿼리가 담긴 Mapper XML 파일들의 위치를 지정합니다.
    -   **`MapperScannerConfigurer`**: `kr.ac.kopo.board.dao` 패키지에 있는 MyBatis Mapper 인터페이스(DAO)들을 스캔하여 Spring Bean으로 등록합니다.
    -   **`DataSourceTransactionManager` & `<tx:annotation-driven>`**: Spring의 선언적 트랜잭션 관리 기능을 활성화합니다. `@Transactional` 어노테이션을 통해 서비스 계층에서 트랜잭션을 쉽게 제어할 수 있습니다.

### 5. `mybatis-config.xml` & Mapper XMLs
-   **역할**: MyBatis 고유의 설정을 정의하고, 실제 실행될 SQL 쿼리를 작성하는 파일들입니다.
-   **주요 설정 (`mybatis-config.xml`)**:
    -   **`<settings>`**: MyBatis의 동작 방식을 미세하게 조정합니다. `mapUnderscoreToCamelCase` 설정을 통해 데이터베이스의 스네이크 케이스(snake_case) 컬럼명과 Java의 카멜 케이스(camelCase) 필드명을 자동으로 매핑합니다.
-   **역할 (Mapper XMLs)**:
    -   `MovieMapper.xml`, `MemberMapper.xml` 등 각 DAO 인터페이스에 매핑되는 SQL 쿼리(SELECT, INSERT, UPDATE, DELETE)를 `<select>`, `<insert>` 등의 태그를 사용하여 작성합니다.

<br>

## 📄 데이터베이스 ERD

> **[권장]** 아래 텍스트 설명을 ERD 이미지 파일로 대체하면 더 좋습니다. (예: `erd.png`)

주요 테이블 간의 관계는 다음과 같습니다.

-   **MEMBER**: 사용자 정보 저장
-   **MOVIE**: 영화 기본 정보 저장
-   **REVIEW**: 사용자가 작성한 리뷰 저장 (MEMBER와 MOVIE에 대한 외래 키 포함)
-   **WISHLIST**: 사용자의 위시리스트 정보 저장 (MEMBER와 MOVIE에 대한 외래 키 포함)
-   **CELEBRITY**: 배우 정보 저장
-   **GENRE**: 장르 정보 저장
-   **MOVIE_CELEBRITY**: 영화와 배우의 다대다(N:M) 관계를 위한 중간 테이블
-   **MOVIE_GENRE**: 영화와 장르의 다대다(N:M) 관계를 위한 중간 테이블

<br>

## 🚀 실행 방법

1.  **저장소 복제**
    ```bash
    git clone https://github.com/your-username/your-repository.git
    ```

2.  **데이터베이스 설정**
    -   Oracle 데이터베이스에 프로젝트에서 사용할 유저 및 테이블 스페이스를 생성합니다.
    -   `src/main/resources/db/schema.sql` (또는 유사한 DDL 파일)을 실행하여 테이블을 생성합니다.
        
    -   `src/main/webapp/WEB-INF/spring/appServlet/context-mybatis.xml` 파일의 `dataSource` bean에서 DB 접속 정보를 자신의 환경에 맞게 수정합니다.
        ```xml
        <property name="url" value="jdbc:oracle:thin:@localhost:1521/XEPDB1" />
        <property name="username" value="your_db_username" />
        <property name="password" value="your_db_password" />
        ```

3.  **프로젝트 빌드**
    -   프로젝트 루트 디렉토리에서 Maven을 사용하여 빌드합니다.
    ```bash
    mvn clean install
    ```

4.  **서버에 배포**
    -   `target` 폴더에 생성된 `movieRevieSite-0.0.1-SNAPSHOT.war` 파일을 Apache Tomcat 등 WAS(Web Application Server)의 `webapps` 폴더에 배포한 후 서버를 실행합니다.

5.  **웹사이트 접속**
    -   웹 브라우저에서 `http://localhost:8080/{프로젝트이름}` 으로 접속합니다.
