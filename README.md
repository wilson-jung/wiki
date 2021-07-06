## Redux를 사용하는데 있어서 api fetching에 드는 중복작업에 대한 불만이 많아 대안을 찾아보다 RTK-Query를 알게 되었다.

## RTK-QUERY 가 해결하려는 점

* * *

Remote Data는 무엇이 달라서 새로운 방법론이 대두 되었는지 알아보기 위해 [rtk-query](https://redux-toolkit.js.org/rtk-query/overview) 문서를 보다

> Over the last couple years, the React community has come to realize that "data fetching and caching" is really a different set of concerns than "state management".
> While you can use a state management library like Redux to cache data, the use cases are different enough that it's worth using tools that are purpose-built for the data fetching use case.

이런 문장이 나오는데 기존 리덕스의 상태관리와 "data fetching and caching" 이 무엇이 다른지 알아보기위해 RTK-Query의 motivation 중 하나인 react-query에 대해 조사해보니 다음과 같았다.

우선 어플리케이션의 상태를 다음과 같이 둘로 나눴을 때 의 특성이다.

\- Sever State

\- Client State

|     |     |
| --- | --- |
| Client State 특징 | Server State 특징 |
| Non-Persistent | Remotely Persisted |
| Synchronous | Asynchronous |
| Client-Owned | Shared Ownership |
| Reliably Up-To-Date | Potentially Out-Of-Date |

여기서 Server State 가 해결해야 할 문제

\- caching

\- deduping requests

\- background update

\- outdated requests

\- mutations

\- Pagination

\- Garbage Colletion

특징과 해결해야 할 문제를 맵핑

|     |     |
| --- | --- |
| Remotely Persisted | caching |
|     | deduping requests |
| Asynchronous | background update |
|     | outdated requests |
| Shared Ownership | mutations |
| Potentially Out-Of-Date | Garbage Colletion, Pagination |



### RTK-Query에서 해결하려는 문제가 '드라이버 피드백'에서 해결해야 할 문제와 일치하는 점이 많아 프로젝트에 적용해보았고, 지금까지 사용해본 결과 내가 생각하는 장단점은 다음과 같다.
---
\# 장점

- loading flag, fething flag 등 meta 데이터를 직접 만들지 않아도 된다.
- 탭이나 달력이동에 대해 이미 진입했던 페이지는 로딩이 없었으면 좋겠다는 요건이 있었는데 caching으로 쉽게 해결이 되었다.
- 특정 시간 이후 저장되었던 data를 설정으로 쉽게 지울 수 있다.
- redux devtoolkit으로 loging이 가능하다.
- api fetching에서 코드가 엄청나게 간소화 되었다.

\# 단점

- saga를 사용했을 때 triger -> result 까지 플로우의 캡슐화가 잘 이뤄졌는데, hook을 사용하다 보니 로직의 파편화가 심하다는 느낌을 받았다.
- meta data를 보면 isLoading, isFetching, isUninitialized 등 많은데 redux store에 저장된 값을 보면 해당 메타데이터는 저장하지 않고 있어서 불만이 있다.
