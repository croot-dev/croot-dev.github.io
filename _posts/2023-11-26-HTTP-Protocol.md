---
layout: post
title: "HTTP Protocol"
created: 2023-11-26
edited: 2024-03-13
category: [기술]
tags: []
---


# HTTP의 정의


> **HTTP**; HyperText Transfer Protocol : 하이퍼텍스트 전송 규약


HTTP는 OSL 7 Layer의 최상위 계층인 **Application 계층**에 위치하며 
데이터 교환을 위한 기본적인 규약을 정의하는데 그 목적을 가지고 있습니다.


**Client-Server 구조**로 이루어져 있으며 보통 Client 요청은 Request, Server 요청은 Response로 말합니다.


HTTP는 Web의 발전에 따라 진화하였으며 이 문서에서는 각 버전 별 특징을 알아보도록 하겠습니다.


# HTTP 버전 별 특징 및 장단점


## HTTP 1.0


### HTTP 1.0 **특징**

1. **단일 연결:** 각 요청마다 새로운 연결을 맺는 방식이었습니다. 각각의 요청은 개별적인 TCP 연결을 통해 이루어졌습니다.
2. **단방향 통신:** 클라이언트가 서버에게 요청을 보내고, 서버가 응답을 반환하는 단방향 통신 방식이었습니다.
3. **상태 저장 불가능:** 서버는 클라이언트의 상태를 유지하지 않았기 때문에 각각의 요청은 독립적이었습니다.
4. **헤더의 부족:** HTTP 1.0의 헤더는 상대적으로 간단하며, 다양한 메타데이터를 전달하기에는 제한적이었습니다.

### HTTP 1.0 **장점**

1. **간단하고 직관적:** HTTP 1.0은 단순하고 직관적인 프로토콜이었기 때문에 구현과 사용이 비교적 쉬웠습니다.
2. **일관성:** 단일 연결을 통한 각 요청이 독립적이기 때문에, 각각의 요청이 일관성 있게 처리되었습니다.

### HTTP 1.0 **단점**

1. **성능 이슈:** 매번 새로운 연결을 맺어야 했기 때문에, 웹 페이지 로딩 시간이 길어지고 성능이 저하되는 문제가 있었습니다.
2. **상태 저장 불가능:** 클라이언트와 서버 간의 상태를 유지하지 않았기 때문에, 특정 세션에서의 정보 공유가 어려웠습니다.
3. **헤더의 부족:** HTTP 1.0의 헤더는 제한적이었기 때문에, 다양한 메타데이터를 전달하기에는 한계가 있었습니다.

<div class="callout" style="display:flex;width:100%;border-radius:4px;background:rgb(241,241,239);padding: 16px 16px 16px 12px;">
<div style="display:flex;align-items:center;justify-content:center;height:24px;width:24px;border-radius:0.25em;flex-shrink:0;">💡</div>
<div style="white-space:pre-wrap;word-break:break-word;caret-color:rgb(55, 53, 47);margin-left:8px;padding-left:2px;padding-right:2px;">HTTP 1.0은 이러한 한계로 인해 웹의 발전과 함께 HTTP 1.1과 같은 업데이트가 이루어지게 되었습니다. HTTP 1.1에서는 Keep-Alive 연결을 통한 지속적인 연결, 파이프라이닝과 같은 기술이 도입되어 성능과 효율성이 향상되었습니다. </div>
</div>


## HTTP 1.1


### HTTP 1.1 **특징**

1. **Keep-Alive 연결 (지속 연결):** HTTP 1.1에서는 Keep-Alive 연결을 통해 단일 TCP 연결을 유지하여 여러 요청과 응답을 하나의 연결에서 처리할 수 있습니다. 이는 연결 지연을 감소시키고 성능을 향상시킵니다.
2. **헤더 압축:** HTTP 1.1에서는 헤더 압축을 지원하여 헤더의 크기를 최소화하고 네트워크 대역폭을 절약합니다.
3. **분할된 요청 처리 (Chunked Transfer Encoding):** 큰 데이터를 여러 개의 작은 조각으로 분할하여 전송하는 Chunked Transfer Encoding을 지원하여 효율적인 데이터 전송이 가능합니다.
4. **파이프라이닝:** 여러 요청을 하나의 TCP 연결로 동시에 보낼 수 있는 파이프라이닝을 지원하여 성능을 향상시킵니다. 하지만 파이프라이닝은 일부 상황에서 지원되지 않을 수 있습니다.
5. **가상 호스팅 지원:** 하나의 웹 서버에서 여러 독립적인 도메인을 호스팅하는 가상 호스팅을 지원합니다.
6. **캐시 지원 향상:** 캐시 헤더를 통해 캐시 제어를 세밀하게 조절하고, 캐시의 효과적인 활용을 가능케 합니다.

### HTTP 1.1 **장점**

1. **성능 향상:** Keep-Alive 연결, 헤더 압축, 파이프라이닝 등의 기능을 통해 HTTP 1.1은 성능을 향상시켰습니다.
2. **효율적인 연결 관리:** Keep-Alive 연결을 통해 여러 요청과 응답을 하나의 연결에서 처리할 수 있어 연결 관리가 효율적입니다.
3. **캐시 향상:** 캐시 지원이 향상되어 콘텐츠의 효과적인 재사용이 가능하며, 네트워크 대역폭을 절약합니다.

### HTTP 1.1 **단점**

1. **파이프라이닝 문제:** 파이프라이닝은 지원되지 않는 경우가 있으며, 일부 상황에서는 요청의 순서가 중요할 수 있습니다.
2. **무거운 헤더:** 헤더의 크기가 크고 불필요한 반복이 있을 수 있습니다. 헤더 압축이나 기타 기술이 필요할 수 있습니다.
3. **보안 부족:** 기본적으로 암호화가 적용되지 않으므로 데이터의 보안이 보장되지 않습니다. 이를 보완하기 위해 HTTPS를 사용하는 것이 권장됩니다.

## HTTP 2.0


### HTTP 2.0 특징

1. **다중 연결 (Multiplexing):** 하나의 TCP 연결을 통해 여러 요청과 응답을 병렬로 처리할 수 있어, 성능과 효율성을 향상시킵니다.
2. **헤더 압축:** 헤더 필드를 압축하여 대역폭을 절약하고 네트워크 성능을 향상시킵니다. 이는 HPACK 압축 알고리즘을 사용합니다.
3. **우선 순위 지정:** 각 요청에 대한 우선 순위를 설정하여 중요한 자원에 대한 빠른 응답을 가능하게 합니다.
4. **서버 푸시 (Server Push):** 서버가 클라이언트의 요청 없이도 필요한 리소스를 미리 보내는 기능으로, 페이지 로딩 속도를 개선하고 불필요한 요청을 감소시킵니다.
5. **세션 상태 유지:** 쿠키 및 기타 메커니즘을 통해 세션 상태를 유지할 수 있어, 상태를 저장할 수 있는 기능을 지원합니다.
6. **보안 강화:** 기본적으로 TLS(Transport Layer Security)를 사용하여 통신을 암호화하고 데이터의 안전성을 보장합니다.

<div class="callout" style="display:flex;width:100%;border-radius:4px;background:rgb(241,241,239);padding: 16px 16px 16px 12px;">
<div style="display:flex;align-items:center;justify-content:center;height:24px;width:24px;border-radius:0.25em;flex-shrink:0;">💡</div>
<div style="white-space:pre-wrap;word-break:break-word;caret-color:rgb(55, 53, 47);margin-left:8px;padding-left:2px;padding-right:2px;">HTTP 2.0은 웹 통신의 성능과 효율성을 향상시키는 많은 혁신적인 기능을 도입하여, 전반적으로 웹 사용자 경험을 향상시켰지만 환경에 따라 호환성 문제에 주의해야 합니다.</div>
</div>


### **HTTP 2.0의 장점**

1. **성능 향상:** 다중 연결 및 헤더 압축을 통해 성능이 향상되어 웹 페이지의 로딩 속도가 빨라집니다.
2. **효율적인 리소스 사용:** 다중 연결과 서버 푸시를 통해 효율적으로 리소스를 관리하고 활용할 수 있습니다.
3. **우선 순위 설정:** 우선 순위를 통해 중요한 자원에 대한 처리를 우선시 함으로써 사용자 경험을 개선합니다.
4. **보안 강화:** TLS 사용을 권장하여 데이터의 안전성을 보장하며, 중요한 정보의 안전한 전송을 지원합니다.

### **HTTP 2.0의 단점**

1. **서버 구현의 복잡성:** HTTP 2.0의 구현이 기존보다 복잡하며, 서버 및 클라이언트가 업그레이드되어야 합니다.
2. **안전하지 않은 환경에서의 사용 어려움:** 보안 강화를 위해 TLS를 권장하지만, 일부 환경에서는 TLS의 설정이 어려울 수 있습니다.
3. **중개 서버의 문제:** 일부 중개 서버나 방화벽에서 HTTP 2.0을 지원하지 않을 수 있어 호환성 문제가 발생할 수 있습니다.

# 버전 별 비교


| **특징**        | **HTTP 1.0**           | **HTTP 1.1**                   | **HTTP 2.0**                  |
| ------------- | ---------------------- | ------------------------------ | ----------------------------- |
| **연결 방식**     | 단일 연결                  | Keep-Alive 연결 (지속 연결)          | 다중 연결 (Multiplexing)          |
| **헤더 처리**     | 헤더의 크기가 크고 불필요한 반복이 있음 | 헤더 압축 및 Keep-Alive로 헤더 재사용 가능  | 헤더 압축 (HPACK 알고리즘 사용)         |
| **캐시 지원**     | 캐시 지원이 미비함             | 캐시 지원이 향상됨                     | 캐시 지원이 향상됨                    |
| **파이프라이닝**    | 지원하지 않음                | 여러 요청을 하나의 TCP 연결로 동시에 보낼 수 있음 | 지원됨 (Multiplexing을 통해 보다 효율적) |
| **분할된 요청 처리** | 지원하지 않음                | 요청 및 응답을 여러 조각으로 분할 가능         | 요청 및 응답을 여러 조각으로 분할 가능        |
| **오류 처리**     | 정의된 몇 가지 오류 코드만 사용     | 더 많은 상태 코드와 세밀한 오류 처리가 가능함     | 더 많은 상태 코드와 세밀한 오류 처리가 가능함    |
| **서버 푸시**     | 지원하지 않음                | 서버 푸시 (Server Push) 지원         | 서버 푸시 (Server Push) 지원        |
| **우선 순위 설정**  | 지원하지 않음                | 요청에 대한 우선 순위 설정 가능             | 요청에 대한 우선 순위 설정 가능            |
| **세션 상태 유지**  | 상태 저장 불가능              | 상태 저장 가능 (쿠키 등 사용)             | 상태 저장 가능 (쿠키 등 사용)            |
| **보안**        | 보안 강화를 위한 추가 표준 필요     | 기본적으로 보안 강화 (TLS 사용 권장)        | 기본적으로 보안 강화 (TLS 사용 권장)       |

