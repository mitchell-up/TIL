# 리액트는 얕은 비교를 한다.
- 리액트에서는 Props, Deps(의존성 배열) 등의 비교를 통해서 재 렌더링를 할지 말지 결정한다.
- 리액트에서는 `shallowEqual`이라는 함수를 통해서 두 객체 간 얕은 비교를 한다.
  
```tsx
function shallowEqual(a?: Object, b?: Object): boolean {
    // 1. Object.is로 두 인자를 비교
    // 2. Object.is로 비교할 수 없으면 객체간 얕은 비교 실행
}
```

> 얕은 비교: 객체의 첫번째 깊이의 값만 비교하는 것.

따라서 컴포넌트의 Props나 useMemo, useEffect, useCallback 등의 의존성 배열 안에 <b>깊이가 깊은 객체</b>를 전달하여 사용하면 의도치 않게 컴포넌트가 렌더링 되거나 아예 변경사항을 추적할 수 없을 것이다.

