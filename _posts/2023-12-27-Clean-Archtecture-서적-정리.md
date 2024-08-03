---
layout: post
title: "Clean Archtecture 서적 정리"
created: 2023-12-27
edited: 2024-08-03
category: [취미]
tags: [Architecture:rgb(232 222 238):rgb(65 36 84)]
---


## 소개


### 설계와 아키텍처의 차이


설계(Design): 저수준의 구조 또는 결정사항 등을 의미


아키텍처(Architecture): 저수준의 세부사항과는 분리된 고수준의 무언가를 가리킬 때 사용하는 단어


### 아키텍처 목표


필요한 시스템을 만들고 유지보수하는 데 투입되는 인력을 최소화하기 위함.


## 가치


모든 소프트웨어 시스템은 이해관계자에게 `행위(behavior)` 와 `구조(structure)`를 제공한다.


소프트웨어는 유연하여야 한다. _(비복비복변순무)_


변경사항 적용 어려움은 범위(scope)에 비례 해야하며 형태(shape)와는 관련이 없어야한다.


즉, 아키텍처는 형태에 독립적 이여야한다.


### 기능과 아키텍처의 관계


시스템은 완전성과 변경용이성이 모두 중요하다. `아이젠하워 매트릭스`에 따라 판단이 중요하다.


![0](/assets/img/2023-12-27-Clean-Archtecture-서적-정리.md/0.png)_Untitled.png_


<div class="callout" style="display:flex;width:100%;border-radius:4px;background:rgb(241,241,239);padding: 16px 16px 16px 12px;">
<div style="display:flex;align-items:center;justify-content:center;height:24px;width:24px;border-radius:0.25em;flex-shrink:0;">💡</div>
<div style="white-space:pre-wrap;word-break:break-word;caret-color:rgb(55, 53, 47);margin-left:8px;padding-left:2px;padding-right:2px;">현업은 주로 3번<i>(긴급하지만 중요하지 않은 일)</i>을 1번<i>(긴급하고 중요한 일)</i>으로 착각하는 실수를 저지른다. </div>
</div>


## 패러다임과 아키텍처 관심사


패러다임: 구조적, 객체지향, 함수형


관심사: 함수, 컴포넌트 분리, 데이터 관리


### 구조적 프로그래밍


제어흐름의 직접적인 전환에 적용되는 규칙


기능적 분해 최고의 실천법


반증 가능한 단위를 만들 수 있는 방법론


### 객체지향 프로그래밍 


제어흐름의 간접적인 전환에 따르는 규칙


SOLID 설계 원칙을 기반함.


특징: `캡슐화`, `추상화`, `다형성`, `정보은닉`, `상속`


다형성을 이용하여 코드 `의존성 역전`할 수 있다.


### 함수형 프로그래밍


변수 할당에 관련된 규칙


람다(lambda) 계산법을 기반함.


가변성과 불변성의 접점, 관리


<div class="callout" style="display:flex;width:100%;border-radius:4px;background:rgb(241,241,239);padding: 16px 16px 16px 12px;">
<div style="display:flex;align-items:center;justify-content:center;height:24px;width:24px;border-radius:0.25em;flex-shrink:0;">💡</div>
<div style="white-space:pre-wrap;word-break:break-word;caret-color:rgb(55, 53, 47);margin-left:8px;padding-left:2px;padding-right:2px;">소프트웨어는 순차, 분기, 반복, 참조 로 구성되며 이 핵심은 변하지 않는다.</div>
</div>


## SOLID 설계 원칙


### 단일 책임 원칙 (SRP; Single Reponsibility Principle)


콘웨이 법칙을 따름


메서드와 클래스 수준의 원칙


### 개방 폐쇄 원칙 (OCP; Open-Closed Principle)


확장에는 개방적, 수정에는 폐쇄적


고수준 컴포넌트를 보호하는 형태의 의존성 계층 구조가 필요


### 리스코프 치환 원칙 (LSP; Liskov Substitution Principle)


구성요소는 서로 치환이 가능해야한다.


아키텍처 수준까지 확장해야하며 위배 시 큰일(?)난다 ㅎㅎ


### 인터페이스 분리 원칙 (ISP; Interface Segregation Principle)


사용하지 않는 것에 의존하지 않는다.


### 의존성 역전 원칙 (DIP; Dependency Inversion Principle)


고수준 정책 구현 코드는 저수준 구현 코드에 의존해서는 안된다.


클래스 단위 정책


## 컴포넌트 응집도


### 재사용/릴리즈 동등 원칙 (REP; Reuse/Release Equivalence Principle)


재사용 단위는 릴리즈 단위와 같다


### 공통 폐쇄 원칙 (CCP; Common Closure Principle)


동일 시점에 동일 이유로 변경되는 것들은 묶고, 아닌 것들은 분리 할 것.


컴포넌트 수준의 SRP


### 공통 재사용 원칙 (CRP; Common Reuse Principle)


필요하지 않은 것에 의존하지 말 것.


ISP의 포괄적인 버전


### 균형 다이어그램


세 가지 원칙은 서로 상충 관계이며 **`REP, CCP 는 포함 원칙`**이며 **`CRP는 배재 원칙`**이다.


![1](/assets/img/2023-12-27-Clean-Archtecture-서적-정리.md/1.png)_Untitled.png_


<div class="callout" style="display:flex;width:100%;border-radius:4px;background:rgb(241,241,239);padding: 16px 16px 16px 12px;">
<div style="display:flex;align-items:center;justify-content:center;height:24px;width:24px;border-radius:0.25em;flex-shrink:0;">💡</div>
<div style="white-space:pre-wrap;word-break:break-word;caret-color:rgb(55, 53, 47);margin-left:8px;padding-left:2px;padding-right:2px;">프로젝트는 일반적으로 삼각형 오른쪽(CCP)에서 시작해서 다른 프로젝트가 시작되면 왼쪽(REP)으로 이동해간다.</div>
</div>


## 컴포넌트 결합


### 의존성 비순환 원칙 (ADP; Acyclic Dependencies Principle)


순환 구조 시 **`숙취 증후군`** 문제 발생하며, **`Weekly Build`**와 **`순환 의존성 제거`** 를 통해 해결

- **`Weekly Build`** : 통합 일정을 잡아 진행, 규모가 커질수록 비효율적
- **`순환 의존성 제거`** : 의존 관계가 **`DAG(Direct Access Graph)`**이 되도록 하는 방법.
	- 의존성 역전 원칙(DIP) 적용하여 해결
	- 의존하는 새로운 컴포넌트 생성
	- 컴포넌트 구조 변경(Jitter)을 항상 관찰

> **숙취 증후군(morning after syndrome) :** 의존하던 무언가를 누군가 수정하여 동작되지 않는 현상


### 안정된 의존성 원칙 (SDP; Stable Dependencies Principle)


더 안정된 방향으로 의존하라

- **안정성 지표**

	$$
	I = FanOut ÷ (FanIn + FanOut)
	$$


	> FanIn: 들어오는 의존성  
	> FanOut : 나가는 의존성


	계산 예시


	![2](/assets/img/2023-12-27-Clean-Archtecture-서적-정리.md/2.png)_Untitled.png_


### 안정된 추상화 원칙 (SAP; Stable Abstractions Principle)


컴포넌트는 안정된 정도만큼만 추상화 되어야한다.


안정성과 추상화 정도 사이의 관계를 정의

- **추상화 정도**

	$$
	A = Na ÷ Nc
	$$


	> Na : 컴포넌트의 추상클래스와 인터페이스 개수  
	> Nc : 컴포넌트의 클래스 개수

- **A/I 그래프**

	![3](/assets/img/2023-12-27-Clean-Archtecture-서적-정리.md/3.png)_Untitled.png_


	> **배재구역**(Zone of Exclusion)**의 종류**  
	> - **Zone of Pain :** 안정적이며 구체적인 컴포넌트, 변동성이 있는 컴포넌트는 변경해야 한다.  
	> - **Zone of Uselessness :** 추상적이지만 의존이 없는 컴포넌트.


	<div class="callout" style="display:flex;width:100%;border-radius:4px;background:rgb(241,241,239);padding: 16px 16px 16px 12px;">
	<div style="display:flex;align-items:center;justify-content:center;height:24px;width:24px;border-radius:0.25em;flex-shrink:0;">💡</div>
	<div style="white-space:pre-wrap;word-break:break-word;caret-color:rgb(55, 53, 47);margin-left:8px;padding-left:2px;padding-right:2px;">주계열(Main Sequence)에 가깝게 위치하는 컴포넌트가 가장 이상적이며,<br />주계열과의 거리를 계산하여 편차가 높은 컴포넌트는 재검토 및 재구성이 필요하다.</div>
	</div>


## 아키텍처


### 독립성


서로 다른 업무 규칙은 분리하여 독립적으로 변경 되도록 해야한다.


좋은 아키텍처는 다음 네 가지 주요항목을 지원한다.

- **UseCase** :  시스템의 수평적 계층을 가로지르는 수직적으로 분리
- **운영** : 요구사항 변경에 대응에 유연하다.
- 개발 : 컨웨이 법칙

	<div class="callout" style="display:flex;width:100%;border-radius:4px;background:rgb(241,241,239);padding: 16px 16px 16px 12px;">
	<div style="display:flex;align-items:center;justify-content:center;height:24px;width:24px;border-radius:0.25em;flex-shrink:0;">💡</div>
	<div style="white-space:pre-wrap;word-break:break-word;caret-color:rgb(55, 53, 47);margin-left:8px;padding-left:2px;padding-right:2px;">Conway’s law : 프로젝트 조직은 조직의 의사소통 구조와 동일한 시스템 설계를 만든다.</div>
	</div>

- 배포 : 즉시배포가 목표이며 UseCase를 유연하게 교체, 변경 할 수 있어야한다.

결합 분리 수준 유형은 `소스 수준`, `배포 수준`, `서비스 수준` 으로 나뉠 수 있으며, 


결합 분리는 선택사항으로 남기고 배포 규모에 따라 선택 사용 할 수 있게 해야한다.


### 경계(Boundary)


컴포넌트 분리 후 핵심 업무 규칙과 플러그인으로 구분


경계전략

