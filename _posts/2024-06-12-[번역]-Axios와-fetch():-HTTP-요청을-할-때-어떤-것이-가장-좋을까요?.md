---
layout: post
title: "[번역] Axios와 fetch(): HTTP 요청을 할 때 어떤 것이 가장 좋을까요?"
created: 2024-06-12
edited: 2024-08-03
category: [기술]
tags: [axios:rgb(219 237 219):rgb(28 56 41),fetch:rgb(250 222 201):rgb(73 41 14),api:rgb(232 222 238):rgb(65 36 84),Frontend:rgb(238 224 218):rgb(68 42 30)]
---


# 서론


최근 포스팅 "[Axios로 전문가처럼 HTTP 요청하는 방법](https://blog.logrocket.com/how-to-make-http-requests-like-a-pro-with-axios/#new_tab)"에서 Axios 라이브러리 사용의 장점에 대해 설명했습니다. 하지만 Axios가 항상 이상적인 솔루션은 아니며 때로는 더 나은 HTTP 요청 옵션이 있다는 점을 인정하는 것이 중요합니다.


의심할 여지 없이 일부 개발자는 사용 편의성 때문에 기본 제공 API보다 Axios를 선호합니다. 그러나 많은 사람들이 그러한 라이브러리의 필요성을 과대평가합니다. fetch() API는 Axios의 주요 기능을 완벽하게 재현할 수 있으며, 모든 최신 브라우저에서 쉽게 사용할 수 있다는 추가적인 장점이 있습니다.


이 글에서는 fetch()와 Axios를 비교하여 서로 다른 작업을 수행하는 데 어떻게 사용할 수 있는지 살펴보겠습니다. 이 글이 끝날 때쯤이면 두 API에 대해 더 잘 이해하실 수 있기를 바랍니다.


# 기본 Syntax


Axios의 고급 기능을 살펴보기 전에 기본 구문을 fetch()와 비교해 보겠습니다.


다음은 Axios를 사용하여 사용자 정의 헤더가 포함된 POST 요청을 URL로 보내는 방법입니다. Axios는 데이터를 JSON으로 자동 변환하므로 사용자가 직접 변환할 필요가 없습니다:


```javascript
// Using Axios

const url = 'https://jsonplaceholder.typicode.com/posts'
const data = {
  a: 10,
  b: 20,
};
axios
  .post(url, data, {
    headers: {
      Accept: "application/json",
      "Content-Type": "application/json;charset=UTF-8",
    },
  })
  .then(({data}) => {
    console.log(data);
});
```


```javascript
// Using Fetch API

const url = "https://jsonplaceholder.typicode.com/todos";
const options = {
  method: "POST",
  headers: {
    Accept: "application/json",
    "Content-Type": "application/json;charset=UTF-8",
  },
  body: JSON.stringify({
    a: 10,
    b: 20,
  }),
};
fetch(url, options)
  .then((response) => response.json())
  .then((data) => {
    console.log(data);
  });
```


<div class="callout" style="display:flex;width:100%;border-radius:4px;background:rgb(241,241,239);padding: 16px 16px 16px 12px;">
<div style="display:flex;align-items:center;justify-content:center;height:24px;width:24px;border-radius:0.25em;flex-shrink:0;">💡</div>
<div style="white-space:pre-wrap;word-break:break-word;caret-color:rgb(55, 53, 47);margin-left:8px;padding-left:2px;padding-right:2px;"><strong>Notice:</strong></div>
</div>


# 이전 버전과의 호환성


Axios의 주요 포인트 중 하나는 광범위한 브라우저 지원입니다. IE11과 같은 구형 브라우저에서도 문제 없이 Axios를 실행할 수 있습니다. 이는 내부적으로 `XMLHttpRequest`를 사용하기 때문입니다.


반면 `Fetch()`는 Chrome 42+, Firefox 39+, Edge 14+, Safari 10.3+만 지원합니다(전체 호환성 표는 [CanIUse.com](https://caniuse.com/)에서 확인할 수 있습니다).


Axios를 사용하는 유일한 이유가 이전 버전과의 호환성 때문이라면 HTTP 라이브러리가 필요하지 않습니다. 대신 fetch()를 지원하지 않는 웹 브라우저에서 이와 같은 폴리필과 함께 fetch()를 사용하여 유사한 기능을 구현할 수 있습니다.


`fetch()` 폴리필을 사용하려면 다음과 같이 npm 명령을 통해 설치합니다:


```bash
npm install whatwg-fetch --save
```


그런 다음 다음과 같이 요청할 수 있습니다:


```javascript
import 'whatwg-fetch'
window.fetch(...)
```


일부 구형 브라우저에서는 프로미스 폴리필이 필요할 수도 있다는 점에 유의하세요.


# 응답 시간 초과


Axios에서 시간 제한을 설정하는 것이 간단하다는 점이 일부 개발자가 `fetch()` 대신 이 방법을 선호하는 이유 중 하나입니다. Axios에서는 선택적으로 구성 객체의 `timeout` 속성을 사용하여 요청이 중단되기 전의 시간(밀리초)을 설정할 수 있습니다.


예시:


```javascript
// Using Axios

axios({
  method: 'post',
  url: '/login',
  timeout: 4000,    // 4 seconds timeout
  data: {
    firstName: 'David',
    lastName: 'Pollock'
  }
})
.then(response => {/* handle the response */})
.catch(error => console.error('timeout exceeded'))
```


`Fetch()`는 `AbortController` 인터페이스를 통해 유사한 기능을 제공합니다. 하지만 Axios 버전만큼 간단하지는 않습니다:


```javascript
// Using Fetch API

const controller = new AbortController();
const options = {
  method: 'POST',
  signal: controller.signal,
  body: JSON.stringify({
    firstName: 'David',
    lastName: 'Pollock'
  })
};
const promise = fetch('/login', options);
const timeoutId = setTimeout(() => controller.abort(), 4000);

promise
  .then(response => {/* handle the response */})
  .catch(error => console.error('timeout exceeded'));
```


# 자동 JSON 데이터 변환


앞서 살펴본 것처럼 Axios는 요청을 보낼 때 데이터를 자동으로 문자열화합니다(기본 동작을 재정의하고 다른 변환 메커니즘을 정의할 수 있지만). 그러나 fetch()를 사용할 때는 수동으로 수행해야 합니다.


아래 두 가지를 비교해 보세요:


```javascript
// Using Axios

axios.get('https://api.github.com/orgs/axios')
  .then(response => {
    console.log(response.data);
  }, error => {
    console.log(error);
  });
```


```javascript
// Using Fetch API

fetch('https://api.github.com/orgs/axios')
  .then(response => response.json())    // one extra step
  .then(data => {
    console.log(data)
  })
  .catch(error => console.error(error));
```


데이터의 자동 변환은 좋은 기능이지만, 다시 말하지만 `fetch()`로 할 수 없는 것은 아닙니다.


# HTTP 인터셉터


Axios의 주요 기능 중 하나는 HTTP 요청을 가로채는 기능입니다. HTTP 인터셉터는 애플리케이션에서 서버로 또는 그 반대로 HTTP 요청을 검사하거나 변경해야 할 때 유용합니다(예: 로깅, 인증 또는 실패한 HTTP 요청 재시도).


인터셉터를 사용하면 각 HTTP 요청에 대해 별도의 코드를 작성할 필요가 없습니다. HTTP 인터셉터는 요청과 응답을 처리하는 방법에 대한 글로벌 전략을 설정하고자 할 때 유용합니다.


Axios에서 요청 인터셉터를 선언하는 방법은 다음과 같습니다:


```javascript
// Using Axios

axios.interceptors.request.use(config => {
  // log a message before any HTTP request is sent
  console.log('Request was sent');

  return config;
});

// sent a GET request
axios.get('https://api.github.com/users/sideshowbarker')
  .then(response => {
    console.log(response.data);
  });
```


이 코드에서 `axios.interceptors.request.use()` 메서드는 HTTP 요청이 전송되기 전에 실행할 코드를 정의하는 데 사용됩니다. 또한 `axios.interceptors.response.use()`를 사용하여 서버의 응답을 가로챌 수도 있습니다. 네트워크 오류가 발생했다고 가정할 때 응답 인터셉터를 사용하면 동일한 요청을 인터셉터를 사용하여 다시 시도할 수 있습니다.


기본적으로 `fetch()`는 요청을 가로채는 방법을 제공하지 않지만 해결 방법을 찾는 것은 어렵지 않습니다. 다음과 같이 글로벌 `fetch()` 메서드를 덮어쓰고 인터셉터를 정의할 수 있습니다:


```javascript
// Using Fetch API

fetch = (originalFetch => {
  return (...arguments) => {
    const result = originalFetch.apply(this, arguments);
      return result.then(console.log('Request was sent'));
  };
})(fetch);

fetch('https://api.github.com/orgs/axios')
  .then(response => response.json())
  .then(data => {
    console.log(data) 
  });
```


# 다운로드 진행상황


진행률 표시기는 대용량 자산을 로드할 때, 특히 인터넷 속도가 느린 사용자에게 매우 유용합니다. 이전에는 자바스크립트 프로그래머가 진행률 표시기를 구현하기 위해 `XMLHttpRequest.onprogress` 콜백 핸들러를 사용했습니다.


`Fetch API`에는 진행 중 핸들러가 없습니다. 대신 응답 객체의 본문 속성을 통해 `ReadableStream` 인스턴스를 제공합니다.


다음 예시는 이미지 다운로드 중에 사용자에게 즉각적인 피드백을 제공하기 위해 `ReadableStream`을 사용하는 방법을 보여줍니다:


```html
<!-- index.html -->

<!-- Wherever you html is -->
  <div id="progress" src="">progress</div>
  <img id="img">
```


```javascript
// script.js

'use strict'
const element = document.getElementById('progress');
fetch('https://fetch-progress.anthum.com/30kbps/images/sunrise-baseline.jpg')
  .then(response => {
    if (!response.ok) {
      throw Error(response.status+' '+response.statusText)
    }
    // ensure ReadableStream is supported
    if (!response.body) {
      throw Error('ReadableStream not yet supported in this browser.')
    }
    // store the size of the entity-body, in bytes
    const contentLength = response.headers.get('content-length');
    // ensure contentLength is available
    if (!contentLength) {
      throw Error('Content-Length response header unavailable');
    }
    // parse the integer into a base-10 number
    const total = parseInt(contentLength, 10);
    let loaded = 0;
    return new Response(
      // create and return a readable stream
      new ReadableStream({
        start(controller) {
          const reader = response.body.getReader();
          read();
          function read() {
            reader.read().then(({done, value}) => {
              if (done) {
                controller.close();
                return;
              }
              loaded += value.byteLength;
              progress({loaded, total})
              controller.enqueue(value);
              read();
            }).catch(error => {
              console.error(error);
              controller.error(error)
            })
          }
        }
      })
    );
  })
  .then(response =>
    // construct a blob from the data
    response.blob()
  )
  .then(data => {
    // insert the downloaded image into the page
    document.getElementById('img').src = URL.createObjectURL(data);
  })
  .catch(error => {
    console.error(error);
  })
function progress({loaded, total}) {
  element.innerHTML = Math.round(loaded/total*100)+'%';
}
```


Axios에서 진행률 표시기를 구현하는 것은 더 간단합니다. 특히 Axios 진행률 표시기 모듈을 사용하는 경우 더욱 그렇습니다. 먼저 다음 스타일과 스크립트를 포함해야 합니다:


```html
<!-- the head of your HTML -->
<link rel="stylesheet" type="text/css"
      href="https://cdn.rawgit.com/rikmms/progress-bar-4-axios/0a3acf92/dist/nprogress.css" />

<!-- the body of your HTML -->
<img id="img" />
<button onclick="downloadFile()">Get Resource</button>
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>
<script src="https://cdn.rawgit.com/rikmms/progress-bar-4-axios/0a3acf92/dist/index.js"></script>

// add the following to customize the style

<style>
#nprogress .bar {
    background: red !important;
}
#nprogress .peg {
    box-shadow: 0 0 10px red, 0 0 5px red !important;
}
#nprogress .spinner-icon {
    border-top-color: red !important;
    border-left-color: red !important;
}
</style>
```


그런 다음 진행률 표시줄을 다음과 같이 구현할 수 있습니다:


```html
<script type="text/javascript">
loadProgressBar();

function downloadFile() {
  getRequest(
    "https://fetch-progress.anthum.com/30kbps/images/sunrise-baseline.jpg"
  );
}

function getRequest(url) {
  axios
    .get(url, { responseType: "blob" })
    .then(function (response) {
      const reader = new window.FileReader();
      reader.readAsDataURL(response.data);
      reader.onload = () => {
        document.getElementById("img").setAttribute("src", reader.result);
      };
    })
    .catch(function (error) {
      console.log(error);
    });
}
</script>
```


이 코드는 `FileReader` API를 사용하여 다운로드한 이미지를 비동기적으로 읽습니다. `readAsDataURL` 메서드는 이미지의 데이터를 Base64로 인코딩된 문자열로 반환한 다음 `img` 태그의 `src` 속성에 삽입하여 이미지를 표시합니다.


# 동시 요청


여러 요청을 동시에 처리하기 위해 Axios는 `axios.all()` 메서드를 제공합니다. 이 메서드에 요청 배열을 전달한 다음 `axios.spread()`를 사용하여 응답 배열의 속성을 별도의 변수에 할당하기만 하면 됩니다:


```javascript
// Using Axios

axios.all([
  axios.get('https://api.github.com/users/iliakan'),
  axios.get('https://api.github.com/users/taylorotwell')
])
.then(axios.spread((obj1, obj2) => {
  // Both requests are now complete
  console.log(obj1.data.login + ' has ' + obj1.data.public_repos + ' public repos on GitHub');
  console.log(obj2.data.login + ' has ' + obj2.data.public_repos + ' public repos on GitHub');
}));
```


기본 제공 Promise.all() 메서드를 사용하여 동일한 결과를 얻을 수 있습니다. 모든 가져오기 요청을 Promise.all()에 배열로 전달합니다. 그런 다음 다음과 같이 비동기 함수를 사용하여 응답을 처리합니다:


```javascript
// Using Fetch API

Promise.all([
  fetch('https://api.github.com/users/iliakan'),
  fetch('https://api.github.com/users/taylorotwell')
])
.then(async([res1, res2]) => {
  const a = await res1.json();
  const b = await res2.json();
  console.log(a.login + ' has ' + a.public_repos + ' public repos on GitHub');
  console.log(b.login + ' has ' + b.public_repos + ' public repos on GitHub');
})
.catch(error => {
  console.log(error);
});
```


# CORS 설정 방법


CORS(교차 출처 리소스 공유)는 서버가 자신 이외의 출처에서 리소스를 로드할 수 있도록 허용하기 위해 HTTP에서 사용할 수 있는 메커니즘입니다. 예를 들어 공개되거나 권한이 부여된 외부 API에서 데이터를 가져오려는 경우 CORS가 필요합니다.


서버에서 CORS 메커니즘이 제대로 활성화되어 있지 않으면 다른 서버로부터의 모든 요청은 Axios 또는 `fetch()`를 사용했는지 여부에 관계없이 `No Access-Control-Header-Present` 오류를 받게 됩니다.


CORS를 올바르게 처리하기 위한 첫 번째 단계는 환경/서버에 따라 서버를 구성하는 것입니다. 서버가 올바르게 구성되면 모든 요청에 대한 응답으로 `Access-Control-Allow-Origin` 헤더가 자동으로 포함됩니다(자세한 내용은 문서 참조).


Axios와 `fetch()` 모두에서 흔히 발생하는 오류는 요청에 `Access-Control-Allow-Origin`을 추가하는 것인데, 이는 응답 매개변수이며 서버에서 원본에 허용된 액세스 제어를 지정하는 데 사용됩니다.


Axios 요청에 헤더를 추가할 때 주의해야 할 또 다른 측면은 요청이 다르게 처리된다는 것입니다. 브라우저는 실제 요청 전에 프리플라이트 요청을 수행하며 이 프리플라이트 요청은 CORS가 준수되는지, 실제 요청을 보내도 안전한지 확인하는 `OPTIONS` 요청입니다.


# 효과적인 응답 처리


응답 관리는 API를 호출하는 모든 애플리케이션에서 매우 중요한 부분입니다. 이 섹션에서는 오류 코드 가져오기와 응답 데이터 조작이라는 두 가지 측면에 대해 간략하게 살펴보겠습니다.


오류 관리는 Axios와 `fetch()`에서 다릅니다. 특히 `fetch()`는 HTTP 404 또는 500 상태 코드와 같은 서버 측 오류가 발생할 경우 자동으로 `promise`를 거부하지 않습니다. 즉, 일반적으로 이러한 응답이 예외로 간주되는 Axios와 달리 이러한 오류는 `.catch()` 블록을 트리거하지 않습니다.


대신 `fetch()`는 응답의 ok 상태가 `false`로 설정된 상태에서 정상적으로 `promise`를 해결합니다. `fetch()` 호출은 네트워크 장애 또는 요청이 완료되지 못한 경우에만 실패합니다.


다음 코드에서는 `fetch()`에서 오류를 처리하는 방법을 확인할 수 있습니다:


```javascript
// Using Fetch API

try {
  const res = await fetch('...');

  if (!res.ok) {
    // Error on the response (5xx, 4xx)
    switch (res.status) {
      case 400: /* Handle */ break;
      case 401: /* Handle */ break;
      case 404: /* Handle */ break;
      case 500: /* Handle */ break;
    }
  }

  // Here the response can be properly handled
} catch (err) {
    // Error on the request (Network error)
}
```


한편, Axios에서는 다음 예시와 같이 적절한 캐치 블록에서 모든 오류를 구분할 수 있습니다:


```javascript
// Using Axios

try {
  let res = await axios.get('...');
  // Here the response can be properly handled
} catch (err) {
  if (err.response) {
    // Error on the response (5xx, 4xx)
  } else if (err.request) {
    // Error on the request (Network error)
  }
}
```


요청에 오류 없이 적절한 응답이 제공되면 두 가지 다른 메커니즘을 사용하여 액세스 가능한 응답 페이로드를 처리할 수 있습니다.


`fetch()`에서 요청/응답 페이로드는 본문 필드에서 액세스할 수 있으며 문자열화되어야 하는 반면, Axios에서는 데이터 필드에 적절한 JavaScript 객체로 존재합니다. 이 차이점은 다음 두 가지 예제에서 확인할 수 있습니다:


```javascript
 // Using Fetch API
 
fetch('...')
  .then(response => response.json())
  .then(data => console.log(data))
  .catch(error => console.error('Error:', error)); 
```


```javascript
// Using Axios

axios.get('...')
  .then(response => console.log(response.data))
  .catch(error => console.error('Error:', error));
```


`fetch()`의 주요 차이점은 `.json()` 메서드를 사용한다는 점입니다. 이름과 달리 이 메서드는 JSON을 생성하는 것이 아니라 JSON을 입력으로 받아 파싱하여 JavaScript 객체를 생성합니다.


# 결론


Axios는 대부분의 HTTP 통신 요구 사항을 위한 사용하기 쉬운 API를 컴팩트한 패키지로 제공합니다. 하지만 기본 API를 선호하는 경우에도 Axios 기능을 구현하는 데 아무런 제약이 없습니다.


이 글에서 설명한 것처럼 웹 브라우저에서 제공하는 `fetch()` 메서드를 사용하여 Axios 라이브러리의 주요 기능을 완벽하게 재현할 수 있습니다. 클라이언트 HTTP API를 로드할 가치가 있는지 여부는 기본 제공 API로 작업하는 데 익숙한지 여부에 따라 달라집니다.


# Refs.


[https://blog.logrocket.com/axios-vs-fetch-best-http-requests/](https://blog.logrocket.com/axios-vs-fetch-best-http-requests/)

