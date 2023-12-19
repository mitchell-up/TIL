# T extends T ? keyof T : never
여러 오브젝트를 `intersection`한 타입에 대해서 각 오브젝트의 키를 모두 포함하는 유니언 타입을 생성하기 위해 아래의 방법을 사용하였다.

```typescript
type UnionKeys<T> = T extends T ? keyof T : never
```
### 해석
1. T는 제네릭 매개변수이다.
2. `T extends T ? A : B` 는 조건부 타입 정의로 여기에서는 `keyof T`로 평가된다.
3. `keyof T` 때문에 T의 각 key에 대해서 2번의 조건부 타입을 평가한다.
4. 결과적으로 T의 모든 key를 유니언으로 갖는 타입이 생성된다.