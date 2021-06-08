---
title: npm - IllegalStateException
categories: [etc]
comments: true
---

## The Problem

IllegalStateException - The specified child already has a parent. You must call removeView() on the child's parent first

`contestbox` project를 진행하던 중 `map component`위에 또다른 `component` 를 배치해야 했다. 원래는 위치를 `absolute` 로 지정하고 `z-index` 를 조절하면 된다. 하지만 API 데이터를 연결하는 과정에서 `map component` 라이브러리 코드 구조를 적합하게 만들지 못해 문제가 발생했다.

## The Explanation

- npm library - `react-native-maps`
- `MapView` 내부에 `Marker` Component 배치
- `Marker` Component는 map 함수로 `api data` 연결
- `MapInfoMenu` Component는 `MapView` 외부에 배치해야함

→ 라이브러리 구조상 MapView 컴포넌트 내부에 반복함수가 필요하지만, 해당 데이터를 연결할 MapInfoMenu 컴포넌트는 외부에 존재해야한다.

## The Solution

→ 데이터를 전달하기 위해 `useState` hook 활용

```jsx
import MapView,{Marker} from 'react-native-maps';

interface InfoProps{
    id:string;
    title:string;
    hits:number;
    tags:Array<{
        id:string;
        label:string;
    }>
    status:string;
    deadline:string;
}

...

const[info,setInfo]=useState<InfoProps>();

...

<MapContainer>
      <MapView ...>
					<Marker .../>
      </MapView>
      {menu?
          <MapInfoMenu
              id={info.id}
              title={info.title}
              hits={info.hits}
              tags={info.tags}
              status={info.status}
              deadline={info.deadline}
          />
      :(null)}
  </MapContainer>

...
```
