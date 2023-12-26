# Tagged Template Literals
주로 Styled-Components와 Emotion에서 Styled 함수를 이용할 때 사용하던 문법이다.

```javascript
const styles = Styled.div`
    background-color: ${color.red};
`
```
위 처럼 주로 사용하곤 했는데 이와 같이 함수이름와 함께 `Template Literals`을 사용해서 호출하는 방식을 `Tagged Template Literals`라고 한다.
<br/>
아래와 같은 방식으로 표현될 수 있다. 첫번째 인자로 변수를 구분점으로 갖는 string 배열이 전달되고, 두번째 인자로는 변수들이 전달된다.
```javascript
const red = '빨간색';
const blue = '파란색';

function favoriteColors(texts, ...values) {
  console.log(texts);
  console.log(values);
}

favoriteColors`제가 좋아하는 색은 ${red}과 ${blue}입니다.`
```
