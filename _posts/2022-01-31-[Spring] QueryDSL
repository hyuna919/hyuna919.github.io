---
title: JPA - QueryDSL
categories:
- Spring
excerpt: |
  A pot still is a type of still used in distilling spirits such as whisky or brandy. Heat is applied directly to the pot containing the wash (for whisky) or wine (for brandy).
feature_text: |
  ## The Pot Still
  The modern pot still is a descendant of the alembic, an earlier distillation device
feature_image: "https://picsum.photos/2560/600?image=733"
image: "https://picsum.photos/2560/600?image=733"
---

- **이클립스에서 적용 방법**
    
    (1) 이클립스 상단 메뉴에서 window 클릭
    
    (2) Show View -> other -> gradle 검색 -> Gradle Task 클릭
    
    (3) Gradle Task에서 해당 프로젝트를 더블클릭
    
    (4) build 폴더로 가서 build를 선택 후 마우스 오른쪽 클릭
    
    (5) Run Gradle Tasks를 클릭하면 src 밑에 generated 폴더가 생성된다.
    
    1. 프로젝트에 새로 생긴 generated의 경로를 추가해줘야 사용 가능
    
    (1) 프로젝트 우클릭 -> Properties ->Java build Path
    
    (2) Source 탭에서 Add Folder... 클릭
    
    (3) src 밑에 새로 생긴 generated폴더 체크 후 확인
    
    (4) apply 클릭 후 확인
    
    (5) 이제 src/main/java에서 src/main/generated를 접근해서 사용할 수 있다.
    
- **설정**
    
    해당 작업은 com.wish.config.JpaConfig.java에서 진행한다.
    
    1. 쿼리를 빌드하려면 JPAQueryFactory인스턴스가 필요하다
    2. JPAQueryFactory는 EntityManager를 필요로한다.
    3. EntityManager는 아래와 같은 방법 중 하나를 통해 사용할 수 있다.
        1. *EntityManagerFactory.createEntityManager()* 호출
        2. *@PersistenceContext 주입*
- **기본문법**
    1. update
        
        ```java
        jpaQueryFactory.update(user)
          .where(user.login.eq("Ash"))
          .set(user.login, "Ash2")
          .set(user.disabled, true)
        	.setNull(userDetail.address)
          .execute();
        ```
        
    2. delete
        
        ```java
        jpaQueryFactory.delete(user)
          .where(user.login.eq("David"))
          .execute();
        ```
        
    3. select - fetch
        
        ```java
        User c = queryFactory.selectFrom(user)
          .where(user.login.eq("David"))
          .fetchOne();
        ```
        
        - fetch : 조회 대상이 여러건일 경우. 컬렉션 반환
        - fetchOne
            - 조회 대상이 1건일 경우 + generic에 지정한 타입으로 반환
            - 1건 이상일 경우 throw *NonUniqueResultException*
        - fetchFirst : 조회 대상이 1건이든 1건 이상이든 무조건 1건만 반환. 내부에 보면 `return limit(1).fetchOne()` 으로 되어있음
        - fetchCount : 개수 조회. long 타입 반환
        - fetchResults : 조회한 리스트 + 전체 개수를 포함한 QueryResults 반환. count 쿼리가 추가로 실행된다.
        
    4. select - orderBy
        
        ```java
        List<User> c = queryFactory.selectFrom(user)
          .orderBy(user.login.asc())
          .fetch();
        ```
        
        - 이 구문은 *Path* 클래스에 *.asc()* 및 *.desc()* 메서드가 있기 때문에 가능합니다
        - *.orderBy()* 메서드에 여러 인수를 지정 하여 여러 필드를 기준으로 정렬할 수도 있습니다.
        
    5. select - groupBy + 별칭으로 정렬
        
        ```java
        // 별칭으로 정렬하려면 Path<>로 정렬할 컬럼을 선언한 후, as로 받아야
        NumberPath<Long> count = Expressions.numberPath(Long.class, "c");
        
        List<Tuple> userTitleCounts = queryFactory.select(
          blogPost.title, blogPost.id.count().as(count))
          .from(blogPost)
          .groupBy(blogPost.title)
          .orderBy(count.desc())
          .fetch();
        ```
        
        별칭 사용을 위해서는 사전에 정의해야한다.
        
        **별칭 기본패턴은 subquery예문 확인**
        
    6. select - join + 별칭
        
        ```java
        // 별칭
        QBlogPost blogPost = QBlogPost.blogPost;
        
        List<User> users = queryFactory.selectFrom(user)
          .innerJoin(user.blogPosts, blogPost)
          .on(blogPost.title.eq("Hello World!"))
          .fetch();
        ```
        
    7. select - subquery
        
        ```java
        // 별칭
        QBlogPost blogPost = QBlogPost.blogPost;
        
        List<User> users = queryFactory.selectFrom(user)
          .where(user.id.in(
            JPAExpressions.select(blogPost.user.id)
              .from(blogPost)
              .where(blogPost.title.eq("Hello World!"))))
          .fetch();
        ```
        
        *JPAExpressions* 팩토리 메서드로 시작
        
- **주의**
    1. insert문 없다. 데이터 삽입 위해서는
        1. To insert data, you should simply persist the entities with EntityManager. ⇒ 무슨소리지?
        2. querydsl-sql라이브러리의 SQLQueryFactory클래스 사용

- **참고 문서**
    
    **공식문서**
    
    [Querydsl Reference Guide](http://querydsl.com/static/querydsl/4.4.0/reference/html_single/)
    
    **공식문서 한글판**
    
    [Querydsl - 레퍼런스 문서](http://querydsl.com/static/querydsl/4.0.1/reference/ko-KR/html_single/)
    
    **Intro to Querydsl**
    
    [](https://www.baeldung.com/intro-to-querydsl)
