---
title: GraphQL Server (빠르게) 만들기-1
date: 2019-02-20 10:02:42
category: development
---

> 블로그를 엄청 오랜만에 쓰게되어 설렙니다.<br>
> 🙈🙈🙈🙈🙊🙊🙊🙊🙈🙈🙈🙈<br>


> 최근 사이드 프로젝트의 api를 GraphQL로 작성했는데, 이를 공유하고자 합니다.<br>
> 이 포스트에서는 Docker + Postgresql + GraphQL + Prisma + Nexus + Typescript를 이용해 인증처리를 포함한 API Server를 만듭니다.
> 내용은 [Repository](https://github.com/sisobus/graphql-typescript-server)에 있습니다.

* **GraphQL Server (빠르게) 만들기-1**
* GraphQL Server (빠르게) 만들기-2

## GraphQL ?

GraphQL은 페이스북이 2012년부터 개발하기 시작해서 2015년에 공개한 Query Language이다.<sup>[[1]]</sup> 주로 단일 Endpoint(/graphql)를 가지고, 문자열로 이루어진 Query문을 정의한 스키마에 따라 응답의 구조가 바뀐다.

REST랑 비교해서 GraphQL을 알아보자. 기존의 REST한 API에 쿼리를 날리면 작성한 API에 해당하는 반환값이 전부 날라온다.

예를 들어, 글을 작성한 user정보를 가져올 때에는 name, birthdate, enterdate가 필요하고, 댓글을 작성한 user정보를 가져올 때에는 name만 필요하다고 가정하자.

만약 server에서 한 endpoint(/users/<:id>)로 처리한다고 하면 댓글을 작성한 user정보를 가져올 때 필요없는 정보(birthdate, enterdate)도 가져오게 된다. 보통 Endpoint를 새로 더 파던지, 요청할 field를 Argument로 명시해주어 해결할 수 있지만 번거롭다.

GraphQL은 다음과 같은 Query문으로 이 문제를 해결할 수 있다.

```graphql
# Reqeust
query {
  user(id: 4802170) {
    id
    name
    isViewerFriend
    profilePicture(size: 50)  {
      uri
      width
      height
    }
    friendConnection(first: 2) {
      totalCount
      friends {
        id
        name
      }
    }
  }
}
```

```json
# Response
{
  "data": {
    "user": {
      "id": "4802170",
      "name": "Lee Byron",
      "isViewerFriend": true,
      "profilePicture": {
        "uri": "cdn://pic/4802170/50",
        "width": 50,
        "height": 50
      },
      "friendConnection": {
        "totalCount": 14,
        "friends": [
          {
            "id": "305249",
            "name": "Stephen Schwink"
          },
          {
            "id": "3108935",
            "name": "Nathaniel Roman"
          },
       ]
      }
    }
  }
}
```

필요한 데이터만 요청해서 받을 수 있다는 점에서 위에서 언급한 번거로움을 해결할 수 있고, 이는 생산성을 향상시켜준다.

그 외에도 페이스북에서 GraphQL의 이점에 대해 나열한게 몇가지 있다.<sup>[[1]]</sup>

1. Request와 Response의 **형태**가 **일치**한다.
>상당히 직관적이다.
2. **그래프** 및 **계층적 데이터**를 요청하는데 **효율적**이다.<br>
>한번의 호출로 다양한 깊이의 많은 Resource를 가져올 수 있다.<br>
>HTTP(S) request수가 적다.
3. **Strongly typed**
>Query전에 타입체크, Runtime Error가 적어진다.
>Typescript, Swift등 강타입 언어와 잘맞는다.
4. Storage가 아닌 **Protocol**이기 때문에 기존 서비스에 적용이 쉽고 함께 쓸 수 있다.
>GraphQL의 단점이 있는 API는 REST로 작성하고 REST의 단점이 있는 API는 GraphQL로 작성할 수 있다.
5. GraphiQL같은 웹기반 Query test interface가 존재한다.
6. Client의 Query에 의해 Resource의 형태가 결정되므로, 서버에서는 이전버전을 유지하면서 새 기능추가를 할 수 있게된다.

페이스북에서 단점은 따로 고지하지 않았기 때문에 나의 주관적인 생각으로 나열해보았다.

1. 단일 endpoint를 사용하기에 HTTP, HTTP(S)에 의한 **캐싱**이 어렵다.<sup>[[2]]</sup>
2. **러닝커브**가 높다.
3. Javascript/Typescript 계열의 언어가 아니면 개발하기 쉽지않다.
>원래 Python Sanic, graphene, ...을 이용해 하려했으나 직접 손봐야되는 부분이 많아서 포기함. 
>그 과정에서 [Sanic-starter](https://github.com/sisobus/sanic-starter)를 정리해봤습니다.
4. Query **N + 1** 문제
>[dataloader](https://github.com/facebook/dataloader)라는 대안책이 있다.
5. Client 캐싱에도 문제가 있을 것 같다. (추측)

이번 사이드 프로젝트의 API를 GraphQL로 선택한 이유는 동료직원의 추천으로 한번 써볼 생각으로 시작했다. 써본 결과 써드파티 라이브러리가 잘 되어있어서 상당히 편하다는 느낌이 들었지만, "이게 왜 좋지?"에 대한 생각을 오랫동안 지울 수는 없었다. 실제로 페이스북의 오픈되어있는 Graph API는 RESTful API이고, Daniel Schafer는 페이스북에서 사용중인 GraphQL API를 공개하지는 않는다는 것으로 보아<sup>[[3]]</sup> 페이스북에서도 적절히 잘 섞어 쓰고 있을거라 추측할 수 있다. 

**결론**: GraphQL을 적용해서 "잘" 쓸 수 있다면 추천 할만 한 것 같다.
>이거 뭔가 RDB랑 NoSQL느낌이 ..

원래 한 포스트로 다 끝내려고 했는데, 생각보다 길어져서 실제 만드는 것은 다음 포스트에서 하겠습니다.😭😭 최종 결과물은 다음과 같습니다.

```swift
      4000:80
    *-----------------------------Docker---------------------------*
    |                                                              |
    |    80/443      *---------*                                   |
    |  *---------*   | graphql >---V                               |
    |  |         >---> Server1 |   |                               |
    |  |         |   *---------*   |                    5432       |
--->*-->         >--->---------*   |     *--------*   *----------* |
    |  | Server  |   | graphql >--->--^--> prisma |   |  (RDB)   | |
    |  | (nginx) |   | Server2 |   |     | server >---> postgres | |
    |  |         |   *---------*   |     *--------*   *-----V----* |
    |  |         >--->---------*   |       4466             |      |
    |  |         |   | graphql >---^                    *---V----* |
    |  *---------*   | Server3 |                        |  data  | |
    |                *---------*                        | Volume | |
    |                                                   *--------* |
    |                                                              |
    *--------------------------------------------------------------*
```



## Reference

[1]: https://code.fb.com/core-data/graphql-a-data-query-language/
[2]: https://blog.apollographql.com/graphql-at-facebook-by-dan-schafer-38d65ef075af
[3]: https://stackoverflow.com/a/44254658
\[1\]: [https://code.fb.com/core-data/graphql-a-data-query-language/](https://code.fb.com/core-data/graphql-a-data-query-language/)<br>
\[2\]: [https://blog.apollographql.com/graphql-at-facebook-by-dan-schafer-38d65ef075af](https://blog.apollographql.com/graphql-at-facebook-by-dan-schafer-38d65ef075af)<br>
\[3\]: [https://stackoverflow.com/a/44254658](https://stackoverflow.com/a/44254658)
