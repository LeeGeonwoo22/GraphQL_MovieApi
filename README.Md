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



