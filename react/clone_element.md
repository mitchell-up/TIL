# cloneElement
`cloneElement`는 시작지점 같은 새로운 리액트 엘리먼트를 만들어줍니다.
```tsx
const clonedElement = cloneElement(element, props, ...children)
```
<br/>

### 예시
- `children`에 props를 부모 컴포넌트에서 지정하고 싶을 때 사용할 수 있습니다.
- 전달되는 props는 override 기존 props를 override 합니다.
```tsx
// Children.map으로 children을 ReactNode Array로 만들어준 후 적용하기
const Parent = ({ children }) => {
    return (
        <div>
            {Children.map(children, (child, index) =>
                cloneElement(child, {
                    className: 'clone',
                })
            )}
        </div>
    )
}

// 단일 children에 적용하기
const Parent2 = ({ children }) => {
    return (
        <div>
            {cloneElement(child, { className: 'clone' })}
        </div>
    )
}
```