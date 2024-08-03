---
layout: post
title: "Javascript module system (CommonJS, ES Modules)"
created: 2023-12-20
edited: 2024-08-03
category: [기술]
tags: [commonjs:rgba(227 226 224 0.5):rgb(50 48 44),es modules:rgb(238 224 218):rgb(68 42 30),cjs:rgb(245 224 233):rgb(76 35 55),mjs:rgb(238 224 218):rgb(68 42 30),ESM:rgb(255 226 221):rgb(93 23 21)]
---


<div class="callout" style="display:flex;width:100%;border-radius:4px;background:rgb(241,241,239);padding: 16px 16px 16px 12px;">
<div style="display:flex;align-items:center;justify-content:center;height:24px;width:24px;border-radius:0.25em;flex-shrink:0;">💡</div>
<div style="white-space:pre-wrap;word-break:break-word;caret-color:rgb(55, 53, 47);margin-left:8px;padding-left:2px;padding-right:2px;">"CJS"와 "MJS"는 각각 CommonJS와 ECMAScript Modules의 약어입니다. 이 두 모듈 시스템은 JavaScript에서 모듈을 정의하고 가져오는 방식을 다르게 다룹니다.</div>
</div>


## 목차


## **CommonJS (CJS):**

- **동기적 로딩:** CommonJS는 동기적인 방식으로 모듈을 로딩합니다. 즉, 모듈이 필요할 때 해당 모듈이 로딩되기를 기다리고, 로딩이 완료된 후에 다음 코드를 실행합니다.
- **서버 측 JavaScript (Node.js):** CommonJS는 주로 서버 측 JavaScript 환경에서 사용되며, Node.js에서 모듈 시스템의 기본이었습니다.
- **require() 함수:** 모듈을 가져오는 데에 `require()` 함수를 사용합니다.
- **단일 인스턴스:** 모듈은 한 번만 로딩되며, 이후에는 캐싱된 인스턴스가 반환됩니다.

예시 (CommonJS):


```javascript
// 모듈 정의
module.exports = {
  key: 'value',
  func: function() {
    // 코드
  }
};

// 모듈 사용
const myModule = require('./myModule');
console.log(myModule.key);
myModule.func();

```


## **ECMAScript Modules (MJS):**

- **비동기적 로딩:** ECMAScript Modules는 비동기적으로 모듈을 로딩합니다. 모듈이 필요할 때 비동기적으로 로딩되며, 이를 위해 `import` 문을 사용합니다.
- **클라이언트 측 및 서버 측 JavaScript:** MJS는 브라우저 환경에서도 사용 가능하며, 모던 브라우저들은 MJS를 지원합니다. Node.js에서도 MJS를 사용할 수 있습니다.
- **import/export 구문:** 모듈을 가져오는 데에 `import` 문을 사용하며, 다른 모듈에서 사용할 수 있도록 변수, 함수, 클래스 등을 `export` 키워드로 내보냅니다.

예시 (ECMAScript Modules):


```javascript
// 모듈 정의
export const key = 'value';
export function func() {
  // 코드
}

// 모듈 사용
import { key, func } from './myModule';
console.log(key);
func();

```


요약하면, CommonJS는 주로 서버 측 JavaScript 환경에서 사용되고 동기적으로 모듈을 로딩하는 반면, ECMAScript Modules는 브라우저와 서버 모두에서 사용 가능하며 비동기적으로 모듈을 로딩합니다. 최신 JavaScript 프로젝트에서는 보통 ECMAScript Modules를 선호하는 추세가 있습니다.


## CommonJS 와 ES Module 비교 표


| 특성                             | CommonJS               | ES Module                 |
| ------------------------------ | ---------------------- | ------------------------- |
| 모듈 정의                          | `module.exports = ...` | `export` 및 `import` 구문 사용 |
| 모듈 로딩                          | `require` 함수 사용        | `import` 구문 사용            |
| 비동기 로딩 지원                      | 지원하지만 주로 동기적 사용        | 네이티브 비동기 지원               |
| 동적 모듈 로딩                       | 지원                     | 지원                        |
| 실행 시점 평가
(Run-time Evaluation) | 가능                     | 불가능 (정적으로 해석됨)            |
| 브라우저 지원                        | 일반적으로 사용 불가능           | 최신 브라우저에서 지원              |
| 활용 분야                          | 주로 서버 측 JavaScript 개발  | 주로 클라이언트 측 및 브라우저 환경      |
| 사용 예시                          | Node.js                | 브라우저 및 Node.js (최신 버전)    |

