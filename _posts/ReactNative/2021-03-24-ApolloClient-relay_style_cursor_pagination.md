---
title: Apollo Client - relay-style cursor pagination
categories: [ReactNative]
comments: true
---

# Apollo Client - relay-style cursor pagination

`ContestBox` 프로젝트를 진행하던 도중, 데이터 로딩 시간을 단축시키기 위해 ScrollView에서 페이지네이션 기능을 제공했다.

`GraphQL` 의 `Apollo Client` 를 사용했으며, `Cursor` , `Offset`, `Relay Style` 기반의 세 가지 `Pagination` 방식 중 Relay style Pagination을 선택했다.

Pagination 개념 요약

[Infinite View Pagination](https://www.notion.so/haena3230/Infinite-View-Pagination-1016e5d3f2b74b38bae506e21f229cb8)

# Relay Style Pagination 적용 방식

## 캐싱 규칙 적용

### fetch policing

서버에서 가져온 데이터를 업데이트 할 때 `network` 에서 가져올지, `cache` 에서 가져올지 등을 정해야 한다. `cache-and-network` 옵션을 통해 서버와 캐시에서 동시에 데이터를 가져와서 비교한 후 캐시와 내용이 다를 경우 업데이트하여 서버와 캐시의 데이터를 일관되게 유지하는 옵션을 사용했다.

```jsx
const { loading, error, data,fetchMore,refetch } = useQuery(GET_LISTS,{
        variables:{
            after:pageInfo.endCursor,
            first:10,
						...

        },
        fetchPolicy: "cache-and-network"
    });
```

docs

[Queries](https://www.apollographql.com/docs/react/data/queries/#cache-and-network)

또한 어떻게 `merge` 할지, `read` 할지 등의 Option을 정의해야 한다.

relay style pagination을 사용했기 때문에, relay에서 정한 규칙을 사용했다.

```jsx
import { relayStylePagination } from "@apollo/client/utilities";

const client = new ApolloClient({
  uri: "endpoint 주소",
  cache: new InMemoryCache({
    typePolicies: {
      Query: {
        fields: {
          contests: relayStylePagination(),
        },
      },
    },
  }),
});
```

## query 작성

relay 규칙에 맞게 pageInfo의 endCursor와 hasNextPage를 사용했으며, edges의 node를 통해 객체를 구현하고, 연결했습니다.

```jsx
export const GET_LISTS = gql`
  query ($first:Int,$after:ID,$categories:[ID!],$search:String,$sort:ContestsSortType,$conditions:[ID!],$types:[ID!],$place:LatLngBoxInput){
    contests(first:$first,after:$after,categories:$categories,search:$search,sort:$sort,conditions:$conditions,types:$types,place:$place) {
      pageInfo{
        endCursor
        hasNextPage
      }
      edges{
        node{
          ...
        }
      }
    }
  }
`;
```

## fetchMore

useQuery의 `fetchMore`함수를 사용해서 맨 아래로 드래그 했을때 `hasNextPage` 의 값이 true일 경우 loading 컴포넌트와 함께 다음 데이터를 불러오도록 적용했다.

# Update

1. Polling
2. refetching

### Polling

`pollInterval` 을 통해서 정해진 시간이 지나면 update 하도록 지정할 수 있다.

### Refetching

또한 useQuery의 `refetch` 함수와 react native 컴포넌트인 `ScrollView` 의 `refreshControl` 옵션을 사용하여 맨 위로 스크롤을 드래그 했을 때 로딩 컴포넌트와 함께 새로운 데이터를 업데이트 할 수 있도록 설정했다.

```jsx
interface pageInfoProps{
    endCursor:string,
    hasNextPage:boolean
}

const CategoryListPage=(props:CategoryListPageProps)=>{

    ...

    // list data

    let listData=``;
    let pageInfo:pageInfoProps={
        endCursor:null,
        hasNextPage:null
    }
    const { loading, error, data,fetchMore,refetch } = useQuery(GET_LISTS,{
        variables:{
            after:pageInfo.endCursor,
            first:10,
            ...
        },
				fetchPolicy: "cache-and-network"
    });
		// refetch
    const [refreshing,setRefreshing]=useState(false);
    const onRefresh=()=>{
        console.log('refetch')
        setRefreshing(true);
        try{
            refetch()
        } catch(e){
            console.log('refetch err')
        }
        setTimeout(()=>{
            setRefreshing(false);
        },2000);
    }
    if (loading) return <Loading />;
    if (error){
        console.log(error.graphQLErrors)
    }

    if(data&&data.contests.edges){
        listData=data.contests.edges.map((data)=>

            ...

        )
        pageInfo=data.contests.pageInfo;
    }
    // pagination
    const onEndReached=()=>{
        if(pageInfo.hasNextPage===true)
            {
                fetchMore({
                    variables:{
                        after:pageInfo.endCursor,
                        first:10,
                        ...
                    }
                })
            }

        else{
            console.log('마지막노드입니다.')
        }
    }
    return(
        <Container>
						<ScrollView
                    ref={scrollRef}
                    onScroll={(e)=>{
                        if (e.nativeEvent.contentOffset.y + e.nativeEvent.layoutMeasurement.height >= e.nativeEvent.contentSize.height){
                            onEndReached()
                        }
                    }}
                    refreshControl={
                        <RefreshControl
                            refreshing={refreshing}
                            onRefresh={onRefresh}
                            colors={[Color.p_color]}

                            />
                    }
                    onScrollBeginDrag={()=>setTotop(true)}
                    >
						...

                <View style={{marginBottom:10}}>
                    {listData}
                </View>

						...
           </ScrollView >
        </Container>
    )
}
```

docs

[Cursor-based pagination](https://www.apollographql.com/docs/react/pagination/cursor-based/#relay-style-cursor-pagination)

[github repository](https://github.com/contestbox/android)
