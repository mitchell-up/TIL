# React.FC를 사용 제고하기

아래와 같은 문제가 존재하므로 사용을 제고해야한다.

### 1. 암시적 children
> React 18에서 암시적 children은 삭제되었습니다.
>
자식노드가 필요 없는 경우에도 에러를 잡아내지 못한다.
```tsx
interface Props {
    name: string
}

const Example: React.FC<Props> = ({ name }) => {
    return (<div>{name}</div>)
}

funtion App() {
    return (
        <div>
            <Example name={'예시'}>
                {'자식노드를 받지 않아도 에러가 나지 않습니다.'}
            </Example>
        </div>
    )
}
```
<br/>

### 2. 함수 타입을 적용하는 것은 어렵다.
아래의 함수타입은 익명함수를 담는 변수에 적용할 수 밖에 없으며 코드 가독성이 떨어집니다.
```tsx
const Greeting:FC<GreetingProps> = function({ name }) {
  return <h1>Hello {name}</h1>
}
```

아래와 같이 함수표현식을 사용하면 가독성 뿐 아니라, Props에 제네릭을 전달하기 간단합니다.
```tsx
// ✅
function Greeting({ name }: GreetingProps<SomeType>) {
  return <h1>Hello {name}</h1>
}
```
<br/>

### 3. defaultProps를 파괴합니다.
```tsx
export const Greeting:FC<GreetingProps> = ({ name }) => {
  return <h1>Hello {name}</h1>
};

// 적용되지 않을 것입니다.
Greeting.defaultProps = {
  name: "World"
};

const App = () => <>
  {/* Big boom 💥*/}
  <Greeting />
</>
```
