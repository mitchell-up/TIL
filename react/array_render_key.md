# React 배열 렌더링 Key
```tsx
function SomeComponent({ data }) {
    return (
        <div>
            {data.map(dataValue => {
                <span key={dataValue.id}>
                    {dataValue.value}
                </span>
            })}
        </div>
    )
}
```
React에서 배열의 각 요소를 렌더링할 때 반드시 `key`를 prop으로 제공해주어야 React가 어떤 데이터가 변경, 추가, 삭제 되었는지 식별 할 수 있다.
#### 그러니까 배열의 Index를 Key로 사용하지 마라.
Array의 index는 Array안에서 Unique하지만 그 값이 고정적이지 않다. 예시는 다음과 같다.
**1. 배열이 재정렬될 때**: 재정렬 되기 전과 같은 데이터가 다른 index를 key로 가지게 되므로 리액트는 불필요한 DOM 업데이트를 수행할 수도 있다.
**2. 배열에 추가, 삭제될 때**: 추가, 삭제도 마찬가지로 같은 데이터가 다른 index를 key로 받을 수 있으므로 불필요한 DOM 업데이트를 수행할 수도 있다.
**3. 컴포넌트의 State 유지**: index 변경으로 인해 key가 바뀐다면 새로운 컴포넌트를 렌더링하기 때문에 예상치 못하게 기존 컴포넌트의 상태를 유지할 수 없게 된다.