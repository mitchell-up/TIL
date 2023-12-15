# 디바이스 스크린 모드에 따른 반응형 CSS
모바일 기기에서는 스크린 모드를 `Portrait` 또는 `Landscape`로 변경할 수 있다.
CSS에서는 스크린 모드를 인식해서 반응형 스타일을 적용할 수 있는 미디어 쿼리 조건을 제공하고 있다.
```html
<style>
    @media screen and (orientation: portrait) {
      /* Portrait 모드일 때 적용할 스타일 */
    }
    
    @media screen and (orientation: landscape) {
      /* Landscape 모드일 때 적용할 스타일 */
    }
</style>
```