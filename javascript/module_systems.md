# Module System
크게 CommonJS와 ESModule에 대해서만 알고 있었는데, AMD, UMD라는 시스템도 있어서 영어로된 아티클 하나를 번역하면서 정리하고자 한다.

[출처] https://dev.to/iggredible/what-the-heck-are-cjs-amd-umd-and-esm-ikm

---
# [번역] 도대체 자바스크립의 CJS, AMD, UMD, ESM이 뭐야?
최초에 자바스크립트에는 모듈을 import/export 방법이 없었습니다. 이건 문제가 있었죠. 하나의 파일에서 앱의 모든 로직을 쓴다고 상상해보세요. 완전 악몽입니다!
<br/>
그 후로, 많은 똑똑한 사람들이 자바스크립에 모듈을 붙이려는 시도를 했었습니다. 그 시도들이 바로 CJS, AMD, USM, ESM이고 이것들에 대해 아마 들어보셨을 겁니다. (다른 많은 방식들이 있지만 이것들이 주요 플레이어입니다.)
<br/>
여기에서는 고수준 정보들인 문법, 목적, 기본동작에 대해 다룰것입니다. 저의 목표는 독자들이 현업에서 저 모듈들을 마주했을 때 그것에 대해 무엇인지 알도록 하는 것입니다.
<br/>

## CJS
CJS는 CommonJS의 줄임말입니다. 다음과 같은 모습입니다.
```javascript
//importing 
const doSomething = require('./doSomething.js'); 

//exporting
module.exports = function doSomething(n) {
  // do something
}
```
- 아마 Node로부터 CJS 문법인줄 바로 알았을 겁니다. Node가 CJS 모듈 포맷을 사용하고 있기 때문이죠.
- CJS는 동기적으로 모듈을 import합니다.
- `node_modules`나 로컬경로로부터 라이브러리를 불러올 수 있는데요. `const myLocalModule = require('./some/local/file.js')` 또는 `var React = require('react');`으로 할 수 있습니다.
- CJS가 import할 때, 가져온 객체의 복사본을 불러옵니다.
- CJS는 브라우저에서 작동하지 않습니다. 트랜스파일 및 번들되어야 합니다.
<br/>

## AMD
AMD는 Asynchronous Module Definition의 줄임말입니다. 여기 코드샘플이 있습니다.
```javascript
define(['dep1', 'dep2'], function (dep1, dep2) {
    //Define the module value by returning a value.
    return function () {};
});
```
또는
```javascript
// "simplified CommonJS wrapping" https://requirejs.org/docs/whyamd.html
define(function (require) {
    var dep1 = require('dep1'),
        dep2 = require('dep2');
    return function () {};
});
```
- AMD는 모듈을 비동기적으로 불러옵니다. (이름과 마찬가지로)
- AMD는 프론트엔드를 위해서 만들어졌습니다. (제안되었을 때에는)(반면에 CJS는 백엔드)
- AMD의 문법은 CJS보다는 덜 직관적입니다. 제 생각엔 AMD가 CJS의 성격이 완전 다른 형제 같습니다.
<br/>

## UMD
UMD는 Universal Module Definition의 줄임말입니다. 코드는 다음과 같습니다.
```javascript
(function (root, factory) {
    if (typeof define === "function" && define.amd) {
        define(["jquery", "underscore"], factory);
    } else if (typeof exports === "object") {
        module.exports = factory(require("jquery"), require("underscore"));
    } else {
        root.Requester = factory(root.$, root._);
    }
}(this, function ($, _) {
    // this is where I defined my module implementation

    var Requester = { // ... };

    return Requester;
}));
```
- 프론트와 백에서 모두 사용가능합니다.(Universal 이름 그대로)
- CJS와 AMD와는 다르게, UMD는 여러 모듈 시스템을 정하는 패턴이라고 볼 수 있습니다.
- UMD는 대체로 Rollup이나 Webpack 같은 번들러를 사용할 때 fallback 모듈로 사용됩니다.
<br/>

## ESM
ESM은 ES Modules의 줄임말입니다. 이것은 표준 모듈 시스템을 구현하기 위한 자바스크립트의 제안입니다. 많은 분들이 아래과 같은 걸 보셨을거라 생각합니다.
```javascript
import React from 'react';
```
현업에서는 이렇게도 많이 볼 수 있습니다.
```javascript
import {foo, bar} from './myLib';

...

export default function() {
  // your Function
};
export const function1() {...};
export const function2() {...};
```
- 현대의 많은 브라우저에서 사용가능합니다.
- 이것은 양쪽의 장점을 모두 가지고 있습니다(CJS의 간편한 문법과 AMD의 비동기특성)
- ES6의 정적 모듈 구조 덕분에 Tree-Shaking이 가능합니다.
- ESM은 Rollup과 같은 번들러가 필요없는 코드를 삭제할 수 있게 해주고, 웹사이트가 더 빠른 로딩을 위해 더 적은 코드를 전달하게 합니다.
- HTML안에서도 불러올 수 있으며, 다음과 같이 합니다
  ```html
  <script type="module">
    import {func1} from 'my-lib';

    func1();
  </script>
  ```
  하지만 아직 모든 브라우저에서 100% 동작하진 않습니다.