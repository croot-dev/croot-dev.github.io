---
layout: post
title: "JSON Query Libraries"
created: 2023-12-29
edited: 2024-01-02
category: [기술]
tags: [JSON:rgb(245 224 233):rgb(76 35 55),jmespath:rgba(227 226 224 0.5):rgb(50 48 44),jsonpath-plus:rgb(211 229 239):rgb(24 51 71),json-query:rgb(238 224 218):rgb(68 42 30),jsonpath:rgb(255 226 221):rgb(93 23 21),JSONStream:rgb(232 222 238):rgb(65 36 84),oboe:rgb(253 236 200):rgb(64 44 27)]
---


개인 프로젝트 중 JSON 데이터 쿼리 조회가 필요해서 찾아보았다.


## 라이브러리 비교


### npm 다운로드 비교


[https://npmtrends.com/JSONPath-vs-jmespath-vs-jq-vs-json-path-vs-json-query-vs-jsonpath-vs-jsonpath-plus](https://npmtrends.com/JSONPath-vs-jmespath-vs-jq-vs-json-path-vs-json-query-vs-jsonpath-vs-jsonpath-plus)


![0](/assets/img/2023-12-29-JSON-Query-Libraries.md/0.png)_Untitled.png_


### 성능 비교 검증


쿼리 라이브러리 성능을 비교한 프로젝트를 찾을 수 있었다. [**Json Querying Performance Testing**](https://github.com/andykais/json-querying-performance-testing)


하지만 4년 전에 작성한 것으로 각 라이브러리 버전이 낮아 최신버전으로 업데이트 후 비교해보았다.


<div class="callout" style="display:flex;width:100%;border-radius:4px;background:rgb(241,241,239);padding: 16px 16px 16px 12px;">
<div style="display:flex;align-items:center;justify-content:center;height:24px;width:24px;border-radius:0.25em;flex-shrink:0;">💡</div>
<div style="white-space:pre-wrap;word-break:break-word;caret-color:rgb(55, 53, 47);margin-left:8px;padding-left:2px;padding-right:2px;">전체 성능 비교 코드는 아래 Github에서 확인 가능합니다.</div>
</div>


> 성능 테스트는 각 라이브러리에 대하여 아래와 같이 세가지 쿼리를 실행합니다.

	- `shallow` returns an object inside an array
	- `deep` returns a string from the object inside the array
	- `conditional` filters the results based on which lots have an 'UNKNOWN' street name, then returns an array of coordinates from inside the object

```markdown
# summary

## smallCityLots
┌───────────────────┬───────────────────┬───────────────────┬───────────────────┐
│      (index)      │      shallow      │       deep        │    conditional    │
├───────────────────┼───────────────────┼───────────────────┼───────────────────┤
│     jmespath      │      0.0445       │      0.0299       │      0.0375       │
│    json-query     │      0.0196       │      0.0301       │      0.0186       │
│   jsonpath-plus   │      0.4228       │      0.3999       │      0.1292       │
│     jsonpath      │      1.4185       │      10.9563      │       0.028       │
│ map-filter-reduce │ 'not implemented' │ 'not implemented' │ 'not implemented' │
└───────────────────┴───────────────────┴───────────────────┴───────────────────┘
## mediumCityLots
┌───────────────────┬───────────────────┬───────────────────┬───────────────────┐
│      (index)      │      shallow      │       deep        │    conditional    │
├───────────────────┼───────────────────┼───────────────────┼───────────────────┤
│     jmespath      │      0.0375       │      0.0192       │      0.0263       │
│    json-query     │      0.0505       │      0.0584       │      0.0469       │
│   jsonpath-plus   │      1.1017       │      0.8726       │      0.2504       │
│     jsonpath      │      3.2803       │      42.4674      │       0.051       │
│ map-filter-reduce │ 'not implemented' │ 'not implemented' │ 'not implemented' │
└───────────────────┴───────────────────┴───────────────────┴───────────────────┘
## largeCityLots
┌───────────────────┬───────────────────┬───────────────────┬───────────────────┐
│      (index)      │      shallow      │       deep        │    conditional    │
├───────────────────┼───────────────────┼───────────────────┼───────────────────┤
│     jmespath      │      0.0178       │       0.031       │      0.0474       │
│    json-query     │     'failed'      │     'failed'      │     'failed'      │
│   jsonpath-plus   │      2.2324       │      2.3612       │      0.4997       │
│     jsonpath      │      9.1876       │     172.0668      │      0.1204       │
│ map-filter-reduce │ 'not implemented' │ 'not implemented' │ 'not implemented' │
└───────────────────┴───────────────────┴───────────────────┴───────────────────┘
```


결론적으로 `jmespath` 가 가장 성능이 좋은 결과가 나왔다. 


## jmespath 


파이썬에서 주로 사용하는 JSON 쿼리 라이브러리이다.


### 공식페이지


[https://jmespath.org/](https://jmespath.org/)


### 문법

1. **프로젝션 (Projection):**
	- 문법: `field`
	- 예시: `name`은 JSON 문서에서 `name` 필드의 값을 가져옵니다.
2. **다중 수준 프로젝션:**
	- 문법: `field.subfield`
	- 예시: `person.name`은 `person` 객체의 `name` 필드의 값을 가져옵니다.
3. **와일드카드 (Wildcards):**
	- 문법:
	- 예시: `people[*].name`은 `people` 배열의 모든 요소의 `name` 필드 값을 가져옵니다.
4. **필터링 (Filtering):**
	- 문법: `[?expression]`
	- 예시: `people[?age > 21].name`은 `people` 배열에서 `age`가 21보다 큰 요소의 `name` 값을 가져옵니다.
5. **내장 함수 활용:**
	- 문법: `function(arguments)`
	- 예시: `length(people)`는 `people` 배열의 길이를 반환합니다.
6. **테이블 슬라이싱:**
	- 문법: `[start:end]`
	- 예시: `people[1:3]`는 `people` 배열에서 두 번째부터 세 번째까지의 요소를 가져옵니다.
7. **정렬 및 리버스:**
	- 문법: `sort_by(@, &field)` 또는 `reverse(@)`
	- 예시: `sort_by(people, &age)`는 `age` 필드를 기준으로 `people` 배열을 정렬합니다.

## jsonpath-plus


### 공식페이지


[https://jsonpath-plus.github.io/JSONPath/docs/ts/index.html](https://jsonpath-plus.github.io/JSONPath/docs/ts/index.html)


### 문법

1. **프로젝션 (Projection):**
	- 문법: `$.field`
	- 예시: `$.name`은 JSON 문서에서 최상위 수준의 `name` 필드의 값을 가져옵니다.
2. **다중 수준 프로젝션:**
	- 문법: `$.field.subfield`
	- 예시: `$.person.name`은 `person` 객체의 `name` 필드의 값을 가져옵니다.
3. **와일드카드 (Wildcards):**
	- 문법: `$.*` 또는 `$[*]`
	- 예시: `$.*.name`은 모든 객체의 `name` 필드 값을 가져옵니다.
4. **필터링 (Filtering):**
	- 문법: `$.field[?expression]`
	- 예시: `$.people[?(@.age > 21)].name`은 `people` 배열에서 `age`가 21보다 큰 요소의 `name` 값을 가져옵니다.
5. **내장 함수 활용:**
	- 문법: `$.function(arguments)`
	- 예시: `$.length($.people)`는 `people` 배열의 길이를 반환합니다.
6. **테이블 슬라이싱:**
	- 문법: `$.field[start:end]`
	- 예시: `$.people[1:3]`는 `people` 배열에서 두 번째부터 세 번째까지의 요소를 가져옵니다.
7. **정렬 및 리버스:**
	- 문법: `$..field | sort` 또는 `$..field | reverse`
	- 예시: `$..age | sort`는 `age` 필드를 기준으로 배열을 정렬합니다.
