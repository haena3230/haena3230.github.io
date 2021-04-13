---
title: Apollo Client - makeVar
categories: [problem]
comments: true
---

## The Problem

`contestbox` project를 진행하면서, `graphQL` 을 활용한 API를 사용했기 때문에 react native 에서 지원하는 `apollo client` 를 이용했다.

react의 hook을 활용하여 api call을 하기 위해 `useQuery` 와 `varibles` 를 활용했는데, 지도의 위치정보 변수를 업데이트하여 다시 call을 하는 과정에서 무한반복 업데이트의 오류가 발생했다.

## The Explanation

해당 오류를 만나기 전에는, React의 상태관리 Hook인 `useState` 를 활용하여 업데이트를 진행했다. 하지만 해당 오류를 만나고 다시 `Apollo Docs` (공식 문서)를 살펴 해결 방안을 찾았다.

참고 - variables 공식문서

[Reactive variables](https://www.apollographql.com/docs/react/local-state/reactive-variables/)

## The solution

- makeVar method 활용

```jsx
import { useQuery,makeVar } from '@apollo/client';
// type 지정
interface MapApiProps{
    place:{
        leftBottom:{
            lat:number,
            lng:number
        },
        rightTop:{
            lat:number,
            lng:number
        }
    }
}
type MapApi=MapApiProps[];

...

// 변수 생성 및 초기화
const initLocation:MapApi=[{
        place:{
            leftBottom:{
                lat:location.latitude-location.latitudeDelta,
                lng:location.longitude-location.longitudeDelta
            },
            rightTop:{
                lat:location.latitude+location.latitudeDelta,
                lng:location.longitude+location.longitudeDelta
            }
        }
    }]
const mapVar = makeVar<MapApi>(initLocation);

```

```jsx
// api call
const { loading, error, data } = useQuery(GET_LISTS,{
        variables:{
            ...
            place:mapVar
      }
  });
```

```jsx
// update
<MapView
  ...
  onRegionChangeComplete={(region)=>{
      mapVar([{
          place:{
              leftBottom:{
                  lat:region.latitude-region.latitudeDelta,
                  lng:region.longitude-region.longitudeDelta
              },
              rightTop:{
                  lat:region.latitude+region.latitudeDelta,
                  lng:region.longitude+region.longitudeDelta
              }
          }
      }])
      console.log(location);
  }}
>
```
