# typeof Function
함수에 대해서 `typeof` 키워드를 사용하면 어떤 결과을 얻을지 애매해서 정리함.

```ts
function add(a: number, b: number): number {
    return a + b;
}
```

위 함수에 대해서 `typeof`를 사용하면 함수 타입을 추론하여 반환함.
```ts
type AddFunction = typeof add;

// AddFunction은 다음과 같아지게 됩니다.
type AddFunction = (a: number, b: number) => number
```

리액트 함수컴포넌트에 대해서는 다음과 같음
```ts
type ComponentType = typeof MyComponent

// ComponentType은 다음과 같아지게 됩니다.
type ComponentType = React.FC<MyComponentProps>
```