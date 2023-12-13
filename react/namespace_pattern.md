# Namespace Pattern
연관 있는 리액트 컴포넌트들을 하나의 네임스페이스로 정리하는 패턴이다.
보통 다음과 같은 모양을 하고 있다.

```tsx
const SomeComp = () => (
    <Modal>
        <Modal.Header/>
        <Modal.Contents/>
        <Modal.Footer/>
    </Modal>
)
```

`Modal` 컴포넌트 내부를 살펴보면 다음과 같이 정리되어 있을 것이다.
```tsx
const Modal = ({ children }) => (
    <div>
        {children}
    </div>
)

Modal.Header = () => (
    <header></header>
)
Modal.Contents = () => (
    <section></section>
)
Modal.Footer = () => (
    <footer></footer>
)
```
<br/>

### 어떻게 이런 표현이 가능한가?
JavaScript에서 함수는 호출가능한 객체이기 때문이다. 따라서 함수자체를 호출할 수도 있으면서도, 객체처럼 함수에  프로퍼티를 추가할 수 있다.<br/>
따라서 리액트 컴포넌트의 네임스페이스 패턴은 네임스페이스 역할을 하는 함수컴포넌트에 property를 추가하고 다른 함수컴포넌트를 등록하여 객체처럼 접근 할 수 있게 된다.