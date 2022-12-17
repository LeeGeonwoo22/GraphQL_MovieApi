## 개발 순서도

### GraphQl 개요

GraphQl API는 많은 Type들의 집합이다. GraphQl Server한테 서버에 있는 data의 type을 설명해 줘야한다. return 하려는 data이거나 사용자가 보낼수있는 data, 혹은 사용자가 쓸수있는 쿼리가 있다. 이것은 GraphQl Server가 실행하기전 GraphQl한테 설명해 줘야할것들이다. 

GraphQl은 REST API에 비해 필요한 정보만 데이터만 요청할 수 있다.  이와 같이, GraphQl은 `over-fetching`을 해결 하였다. 

`under-fetching`은 우리가 필요한 데이터보다 충분한 데이터를 받지 못하는 것을 말한다. 
예를 들어, 두가지의 값을 요구하였는데, 만약 두가지의 필드가 두가지의 endpoint로 나뉘어졌다면 하나의 요청으로 모두 받아올 수 없어 필연적으로 API를 두번 호출해야한다. 

그러면 단일 API 호출에 비해 네트워크 오버헤드가 추가되기 때문에 사용자가 앱 사용시 느린속도로 불편함을 느낄 수 있다. 

하지만 GraphQl을 이용하면 이러한 정보를 **한번**의 요청으로 해결할 수 있다.

### Query와 Mutation

Query : 단순 db등으로 부터 데이터를 받음. Query도 유저나 프론트엔드와 하여금 API로 소통할 수 있지만 GET Request에 가깝다.

Mutation : API와 본격 소통한다. db등 데이터를 변경(POST/PUT/DELETE)

### 테스팅

[GraphQl API](graphql.org/swapi-graphql) 여기서 테스트 한다. 

그래피QL은 브라우저 툴인데, GraphQl 쿼리를 쓰고 보낼수 있게 해준다.

![스크린샷 2022-12-17 18-44-06](https://user-images.githubusercontent.com/71261997/208235980-6ca35224-c63d-4973-ae1f-c120037e3ba5.png)

그냥 위와같이 이름만 써주고, `resolver`로 부터 `property`들을 얻고싶다고 하면된다. 

### GraphQl Apollo server 초기셋팅

1. 폴더를 만들고 `Apollo server` 를 설치 및 생성한다.

    `npm i apollo-server graphql`

2. `pakage.json`에서 `"type" : "module"` 로 설정해준다. 

    이를 통해 `import` 문을 사용하여 `Apollo-server`에서 `ApolloServer`와 `gql`을 `import` 해올수 있기 때문이다.

    if) `const {ApolloServer, gql} = require("apollo-server)"`

3. data의 shape를 설명해준다. 이를 바탕으로 GraphQl에서는 자동완성을 사용할 수 있다.

    `const typesdefs = gql`
    (스키마 정의 언이(SDL) : 데이터의 모양을 GraphQl에게 설명해줌)``

    그리고 `gql` 안에 `Query` 유형을 추가한다. 

    ![스크린샷 2022-12-17 19-16-26](https://user-images.githubusercontent.com/71261997/208236994-07c0a65a-12e5-49c3-a2fd-4dc82baf85c8.png)

    `type Mutation`은 앞서 언급하듯 db의 POST/PUST/DELETE 요청을 수행한다.

    ![스크린샷 2022-12-17 20-37-17](https://user-images.githubusercontent.com/71261997/208239889-821390d1-30da-4231-8e4b-25e4aeefd45c.png)

    만약 `GraphQl`에게 어떤 필드가 무언가 list 준다는 것을 이야기 하고 싶다면, 아래와 '같이 대괄호('[]')안에  `type`의 이름을 넣어주면 된다. 
    
    ![스크린샷 2022-12-17 20-27-09](https://user-images.githubusercontent.com/71261997/208239462-6975e74f-e8e9-4d79-af5c-c5dcc8d9ce9e.png)

    `!`는 `GraphQl`한테 이 필드들이 null이면 안된다는 것을 알려준다. 무엇이 `required`이고 아닌지 구체화 할수 있다.
    `[Tweet!]!`은 `[]!` 과 같으며 항상 list 여야 한다는 뜻이다.

### GraphQl Resolver

1. Query Resolver 만들기

    위 작업은 `GraphQl`의 스키마 정의였다. resolver는 하나의 로직이며 객체같은것이다. 이 객체는 타입과 정의를 거쳐야한다. 

    만약 Apollo에 누군가 query의 tweet을 요청하는것을 본다면, Apollo는 resolver의 query로 갈 것이고 resolver에서 함수(아래의 tweet())를 실행시킬 것이다. 

    ![스크린샷 2022-12-17 20-49-50](https://user-images.githubusercontent.com/71261997/208240390-16151dbe-1e73-455d-9696-08afc58cb934.png)

    그리고 return 되는것은 위 부분에서 해당 필드에(type Query의 tweet필드)에 실행될것이다. 

    **test 1**

    ![스크린샷 2022-12-17 20-51-23](https://user-images.githubusercontent.com/71261997/208240452-0ecff8b2-462f-41e1-998b-673233b7f442.png)

    **test 2**

    ![스크린샷 2022-12-17 20-52-30](https://user-images.githubusercontent.com/71261997/208240494-c013c96b-96d4-4e27-a78d-6a9d89fb7185.png)

    
    
    나중엔 데이터베이스에 접근해서 어떤 SQL 코드를 실행 시킬수도 있다.

    **test 1**
    ![스크린샷 2022-12-17 20-57-45](https://user-images.githubusercontent.com/71261997/208240718-61cd56c2-c33b-45de-b9f6-ccee9788f2fa.png)

    사실 위에 `root`를 무시하기위해 `_`를 쓰기도 한다. 만약 아무것도 넣지않는다면 `root`로 부터 `id`를 가져오게 된다. 왜냐하면 이것은 첫번째 `argument`이기 때문이다. 첫번째 것은 무시해야한다. 

    **test 2**
    ![스크린샷 2022-12-17 20-59-39](https://user-images.githubusercontent.com/71261997/208240787-2987d3a9-b1ea-478b-8a04-f6345f58cc98.png)
    null이 나온다.

    **test 3**
    ![스크린샷 2022-12-17 21-00-43](https://user-images.githubusercontent.com/71261997/208240836-9a12fbfc-e81a-4a3e-8175-8741d32fc471.png)
    위에 test1에 원하고자 하는 결과를 제대로 출력! 

2. 유저들과 반응하는 resolver 만들기

    ![스크린샷 2022-12-17 21-09-43](https://user-images.githubusercontent.com/71261997/208241155-16766e71-aa3b-4f25-8dd8-e1e70c228ef2.png)

    본 로직은 굳이 외울 필요없다. 자바스크립트로 db를 구현한것 뿐이다. 

    ![스크린샷 2022-12-17 21-11-16](https://user-images.githubusercontent.com/71261997/208241200-345d24ad-5b80-4f9c-a9e0-bde79f311666.png)

    본 로직 구현 테스팅결과

    delete 로직

    ![스크린샷 2022-12-17 21-14-28](https://user-images.githubusercontent.com/71261997/208241385-1f38ea70-2f60-4ce1-9376-37b759d401b5.png)

    **test 1**
    ![스크린샷 2022-12-17 21-15-03](https://user-images.githubusercontent.com/71261997/208241408-475afd11-3caf-4991-824b-1b6cb249d307.png)

    **test 2**
    ![스크린샷 2022-12-17 21-16-44](https://user-images.githubusercontent.com/71261997/208241440-112c2a6a-bdd9-4a45-a1ca-458cab5f0ab2.png)

    **test 3**
    ![스크린샷 2022-12-17 21-17-19](https://user-images.githubusercontent.com/71261997/208241471-8cb7089b-d61e-48ac-bd9d-7f9c2ca527b7.png)

    이렇듯 resolver를 비즈니스로직 처럼 활용하여 직접적으로 db를 수정하거나 불러올 수 도 있다.

### GraphQl Relationships

    `tweets`들과 `user`들을 연결하려 할때, 각각 임의로 필드에 `userId` 를 넣어준뒤 1은 2를 , 2는 1로 값을 서로 넣어준다. (Join) 
    SQL database가 저장하는 방식이랑 비슷하다. 외래키를 넣고 조인하는것. 

    resolver에서 로직 구현 
    
    ![스크린샷 2022-12-17 21-28-46](https://user-images.githubusercontent.com/71261997/208241863-5323eb03-bca4-4dbe-a30b-178b29ffe566.png)