---
layout: post
title: "Proxy와 Reflect"
created: 2024-03-19
edited: 2024-08-03
category: [기술]
tags: [ES6:rgb(219 237 219):rgb(28 56 41),Javascript:rgb(232 222 238):rgb(65 36 84)]
---


## Proxy


### 정의


`Proxy` 객체는 다른 객체에 대한 대리자 역할을 하며, 목표 객체의 동작을 가로채어 필요 시 수정할 수 있는 패턴의 객체이다.


### 구성


```javascript
// 기본 생성자 이용
new Proxy(target, handler);

// revoke(취소) 가능한 Proxy 객체
new Proxy.revocable(target, handler);
```


`Proxy` : 대상 객체와 핸들러 객체를 파라미터로 받으며 핸들러를 통해 대상 객체를 제어


`Target` : 프록시가 감싸는 실제 객체


`Handler` : 프록시의 동작을 정의하는 객체, 프록시에 적용할 트랩(trap) 함수를 포함


> [Proxy.revocable 자세히 보기](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy/revocable)


### Handler에서 사용되는 주요 Trap 함수

- **`get`** : 프록시가 대상 객체의 속성을 읽을 때 호출
- **`set`** : 프록시가 대상 객체의 속성에 값을 할당할 때 호출
- **`apply`** : 프록시가 대상 객체의 함수를 호출할 때 호출
- **`construct`** : 프록시가 대상 객체의 생성자를 호출할 때 호출
- **`deleteProperty`** : 프록시가 대상 객체의 속성을 삭제할 때 호출
- **`enumerate`** **or** **`ownKeys`** **:** 프록시가 대상 객체의 속성을 열거할 때 호출

### 예시


```javascript
const docCookies = new Proxy(docCookies, {
  // 쿠키 값을 가져올 때 호출됨
  get(target, key) {
    return target[key] || target.getItem(key) || undefined;
  },
  // 쿠키 값을 설정할 때 호출됨
  set(target, key, value) {
    if (key in target) {
      return false;
    }
    return target.setItem(key, value);
  },
  // 쿠키 속성 삭제를 가로챔
  deleteProperty(target, key) {
    if (!(key in target)) {
      return false;
    }
    // 쿠키 삭제
    return target.removeItem(key);
  },
  // 쿠키 속성을 열거함
  ownKeys(target) {
    return target.keys();
  },
  // 특정 키가 쿠키에 존재하는지 확인
  has(target, key) {
    return key in target || target.hasItem(key);
  },
  // 속성을 정의함
  defineProperty(target, key, descriptor) {
    if (descriptor && "value" in descriptor) {
      target.setItem(key, descriptor.value);
    }
    return target;
  },
  // 속성의 속성 서술자를 가져옴
  getOwnPropertyDescriptor(target, key) {
    const value = target.getItem(key);
    return value
      ? {
          value,
          writable: true,
          enumerable: true,
          configurable: false,
        }
      : undefined;
  },
});

/* 사용 테스트 */
console.log((docCookies.myCookie1 = "First value")); // 쿠키 설정
console.log(docCookies.getItem("myCookie1")); // 쿠키 가져오기

docCookies.setItem("myCookie1", "Changed value"); // 쿠키 값 변경
console.log(docCookies.myCookie1); // 변경된 쿠키 값 가져오기
```


### ⚠️주의

- `get` trap 호출 시 this 는 `Proxy` 객체를 나타낸다.

## Reflect


### 정의


JavaScript에서 메타프로그래밍을 위한 내장 객체로, 다양한 기본 동작들을 수행할 수 있는 메서드들을 제공


### 주요 Method


```javascript
Reflect.apply(target, thisArgument, argumentsList)
// target 함수를 this 값(thisArgument)과 인수(argumentsList) 목록으로 호출
Reflect.construct(target, argumentsList, newTarget?)
// 생성자 `target` 를 인수(`argumentsList`) 목록을 사용하여 호출하여 새로운 인스턴스를 생성, 선택적으로 생성자(`newTarget`)의 변경된 버전을 사용 가능
Reflect.get(target, propertyKey, receiver?)
// target 의 propertyKey 값을 가져옵니다. 선택적으로 receiver 객체를 제공하여 this 설정 가능
    
Reflect.set(target, propertyKey, value, receiver?)
// target.propertyKey 에 값을 설정, 선택적으로 receiver 객체를 제공하여 this 설정 가능
    
Reflect.has(target, propertyKey)
// propertyKey 속성이 target에 존재하는지 확인
    
Reflect.deleteProperty(target, propertyKey)
// target 에서 propertyKey 속성을 삭제

Reflect.getOwnPropertyDescriptor(target, propertyKey)
// target.propertyKey 에 대한 설명자(descriptor)를 반환

Reflect.defineProperty(target, propertyKey, attributes)
// target.propertyKey 에 대한 새로운 설명자(descriptor)를 설정

Reflect.getPrototypeOf(target)
// target 의 프로토타입을 반환

Reflect.setPrototypeOf(target, prototype)
// target 의 프로토타입을 설정

Reflect.isExtensible(target)
// target 이 확장 가능한지 여부를 반환

Reflect.preventExtensions(target)
// target 이 확장되지 않도록 설정

Reflect.ownKeys(target)
// target 의 모든 속성 키 배열을 반환
```


## REFS.


[https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Proxy#명세](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Proxy#%EB%AA%85%EC%84%B8)

