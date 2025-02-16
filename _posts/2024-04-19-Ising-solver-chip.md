---
layout: post
title: "Ising solver chip"
created: 2024-04-19
edited: 2025-02-15
category: [취미]
tags: []
---


[s41928-023-01021-y_240419_152231_240419_155522.pdf](https://prod-files-secure.s3.us-west-2.amazonaws.com/8daffe33-d95b-4c96-91e6-1b899bcdb2d7/6a760c73-e074-4be2-b702-7decc823199c/s41928-023-01021-y_240419_152231_240419_155522.pdf?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YQRUMW25%2F20250216%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250216T141853Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDQaCXVzLXdlc3QtMiJGMEQCIB5D6g%2By8JFUOZWyN5%2BY%2FSr09NRWj5zMCEkPHNzDcF3GAiA%2BDjvUHgW6Kr9IQuwV0rCFRXXluq0mjSjfe9XKvK46kir%2FAwhdEAAaDDYzNzQyMzE4MzgwNSIMfs2Goa%2Fq2id8Gc4oKtwDNGar1oF2I6UXy1m9VXaCfMsJeaw4HcXUYxXN8IRiC%2Bmr1vUB9DqdE9DjSqDOLBy52KGwMZIDTuIB%2FnOSvu3iKpEiLDSwcn4bKV0e5rfrouZu8RwF7iQVyK7dY8jr75w%2BwsC%2BUIt%2BIbFMPQ6mlkSLdtHYI9K1rnjO79y1KHEzOxaZxoeLKwQ%2FRCd%2BjtMYeobhRgvPQ8%2FsGxglHJlVBSgUpLnIFSDnnuPdFP4vDbxmby%2BfCjgF29b9g2pNTjkMZ6Ll1qhtJ6g64If1qG5mAFqFhQ3wIHK9F48ZkhYz8MAOGBe16blgGpEqjskoRmSLcItK8XF%2BrPWhRz03PD357pmzng82DkK5%2BpUVcLj2CI7tdNKcKnF6wLbXbiGcVc5ExZSxdcXn%2B3dQgAaQC%2BKvFcztQtDjFy4V950%2FPnR6RwPQNuWNeDZL3olD%2Ft1jfWZfkJv0UOKXyqyt3QHPvmqyuVUZUaf5H1sLy0kN6svUUMIHQE4VU0nGTYPsq2RNFcQbi%2F7M2vMkM%2B2Nmx9KGLHJuGM4LqZokisr7iRbb%2B7BXFUOec92aaoJk99qSW8AxweoOjkDRjwgi868miQRTh8UMnxzrjNSIsyI5ItIwOxgp%2Fv1G%2BJJjHQm%2BPIGu%2B%2Ft%2F7kw6J3HvQY6pgGsBGlNUg%2FQrNa2jsBZW0CthV%2F7nv5gO3k8zo6zCIujUVx9bBy3BjxBOqYXqzR5LlcYxNTfb6tlfZCF2TX0OTANvonEqC3on8YsOUv6LxYO7Fg5rr9q%2FqscSyoIOLzXc%2Ft0hiOeRSrHqQB5y2b7STawdoAsa9HkghyxnLee%2Bs6Bs6HsGt4uMauyZASUv772WS%2Fhs5gbQAI7GUP67UKCjf7Cdv4GRBT7&X-Amz-Signature=fb96dc93a09d547f74f46c6cb9b58880ab1f2c5c8eda4ff322343c269258b18d&X-Amz-SignedHeaders=host&x-id=GetObject)


# 48노드 전체 연결 어레이 아키텍처를 갖춘 결합 링 진동자를 기반으로 하는 Ising 솔버 칩 


**Received: 19 December 2022
Accepted: 27 July 2023**
Published online: 31 August 2023


**Hao Lo, William Moy   , Hanzhao Yu, Sachin Sapatnekar & Chris H. Kim**


---


양자에서 영감을 얻은 컴퓨팅 시스템을 사용하여 조합 최적화 문제를 효율적으로 해결할 수 있습니다.


이러한 시스템을 개발할 때 핵심 과제는 임의의 문제 그래프를 하드웨어에 직접 매핑할 수 있는 전체 노드 연결을 갖춘 대규모 하드웨어 토폴로지를 생성하는 것입니다.


여기에서는 표준 1.2V, 65nm 상보형 금속-산화물-반도체 기술로 제작된 물리학 기반 Ising 솔버 칩에 대해 작성했습니다.


이 칩은 48개의 스핀을 갖춘 all-to-all 아키텍처와 -14에서 +14까지의 정수 가중치를 갖는 매우 균일한 결합 회로를 특징으로 합니다. all-to-all 아키텍처는 수평 진동자(oscillator)와 수직 진동자(oscillator)를 강력하게 결합하여 각 수평-수직 진동자 쌍이 교차스타일(corssbar-style) 배열의 다른 모든 쌍과 교차하고 최대 48개의 노드가 있는 그래프를 직접 매핑할 수 있도록 합니다. 하드웨어. 우리는 Ising 솔버 칩을 사용하여 다양한 문제 크기, 그래프 밀도, 작동 온도 및 문제 인스턴스에 대한 통계 측정을 수행합니다.


Ising 솔버는 제약 없는 2차 이진법에 대한 솔루션을 찾을 수 있습니다. 최적화(QUBO) 문제1 진동자 의해 여기로 전송됨) 바닥 상태로 이완됩니다. 관찰 가능한 자연스러운 상호 작용에서 영감을 받은 Ising 솔버는 복잡한 알고리즘이 사용되는 시뮬레이션 어닐링과 같은 소프트웨어 접근 방식과 다릅니다. 회전 상태를 교란시키고 목적 함수의 변화를 추적하는 데 사용됩니다. 물리 기반 Ising 솔버는 바닥 상태로 완화되는 자연의 능력을 활용하여 복잡한 알고리즘 없이도 QUBO 문제에 대한 경쟁력 있는 솔루션을 찾을 수 있으며, 이로 인해 훨씬 더 나은 [time-to-solution](/393f664b73e942238e8d31b34c2b7bd8#30123aafe8984010998810809ce45a9d) 및 [energy-to-solution](/393f664b73e942238e8d31b34c2b7bd8#30123aafe8984010998810809ce45a9d) 결과를 얻을 수 있습니다.


Ising 솔버의 단순화된 계산 흐름은 [_[그림 1]_](/393f664b73e942238e8d31b34c2b7bd8#bb0da212349645be8ed3e07925c3cb1b)에 나와 있습니다.


첫째, 대규모 조합 최적화 문제는 스핀 상태를 나타내는 꼭지점과 결합 가중치를 나타내는 모서리를 갖는 무방향 그래픽 표현을 사용하여 공식화됩니다. 스핀 si(si = ±1)의 상태는 Ising Hamiltonian 함수에 의해 제공되는 최소화할 목적 함수를 결정합니다.


$$
H = - \sum_{i,j} J_{ij} s_i s_j - \sum_i h_i s_i

$$


결합 강도 $J_{ij}$는 스핀 $i$와 $j$ 쌍 사이의 친화력을 모델링하는 반면, $h_i$는 스핀 $i$에 작용하는 로컬 필드 매개변수를 나타냅니다. 직관적으로 두 개의 스핀이 양의 결합 가중치 $J_{ij}$와 결합되면 에너지가 최소화되므로 해당 상태가 동일한 위상으로 해결되는 경향이 있습니다. 마찬가지로, 음으로 결합된 스핀은 자연스럽게 반대 위상으로 수렴하여 에너지를 최소화합니다. 일반화된 시나리오에서 스핀은 다양한 극성과 결합 강도를 갖는 여러 결합 스핀에 의해 작용할 수 있으며, 집합적으로 스핀은 $H(s)$를 최소화하는 최적의 균형을 찾습니다.


공식화된 조합 최적화 문제는 일반적으로 원래의 QUBO 문제를 여러 하위 QUBO 문제로 분해한 후 하드웨어에 의해 반복적으로 해결되는 소프트웨어 알고리즘이 필요한 단일 하드웨어 인스턴스에 적합하지 않습니다(이 인스턴스는 [_[그림 1a]_](/393f664b73e942238e8d31b34c2b7bd8#bb0da212349645be8ed3e07925c3cb1b)에서 생략됨).
예를 들어 실제 문제는 수천 개 이상의 스핀으로 구성될 수 있지만 단일 Ising 솔버의 스핀 수는 하드웨어 기술의 물리적 한계로 인해 100개 미만으로 제한될 수 있습니다. 문제 스핀 수가 솔버 스핀 보다 클 때 양자 어닐러를 사용하여 얼마나 큰 QUBO 문제를 해결할 수 있는지 보여줍니다.


<div class="callout" style="display:flex;width:100%;border-radius:4px;background:rgb(241,241,239);padding: 16px 16px 16px 12px;">
<div style="display:flex;align-items:center;justify-content:center;height:24px;width:24px;border-radius:0.25em;flex-shrink:0;">💡</div>
<div style="white-space:pre-wrap;word-break:break-word;caret-color:rgb(55, 53, 47);margin-left:8px;padding-left:2px;padding-right:2px;"><strong>time-to-solution</strong> : 문제를 해결하는 데 소요되는 시간  <br /><strong>energy-to-solution</strong> : 문제를 해결하는 데 필요한 에너지 양  </div>
</div>


<div class="callout" style="display:flex;width:100%;border-radius:4px;background:rgb(241,241,239);padding: 16px 16px 16px 12px;">
<div style="display:flex;align-items:center;justify-content:center;height:24px;width:24px;border-radius:0.25em;flex-shrink:0;">💡</div>
<div style="white-space:pre-wrap;word-break:break-word;caret-color:rgb(55, 53, 47);margin-left:8px;padding-left:2px;padding-right:2px;"><strong>QUBO</strong> ;Quantum Approximated Optimization Algorithm : 양자 근사 최적화 알고리즘</div>
</div>


**[그림 1].** Ising 솔버 계산 흐름, 연결성 비교 테이블 및 마이너 임베딩 예시


![0](/assets/img/2024-04-19-Ising-solver-chip.md/0.png)_Untitled.png_


**[그림 1a].** Ising 솔버의 단순화된 컴퓨팅 흐름.

	1. 첫째, 최적화 문제는 Ising 가중치 $J_{ij}$와 $h_i$에 매핑됩니다.
	2. 둘째, 전체 그래프는 Ising 솔버 칩에 맞을 수 있는 하위 그래프로 분류됩니다.
	3. 셋째, 가중치가 솔버에 프로그래밍되고 스핀이 바닥 상태로 완화됩니다.
	4. 마지막으로 진동자 위상을 읽고 조합 최적화 문제에 대한 해결책을 찾습니다.

**[그림 1b].** 클래식 및 양자 최신 어닐링 프로세서의 연결성. 연결성은 직접 연결되어 있는 노드의 수입니다.


**[그림 1c].** 로컬로 연결된 Chimera 그래프와 King의 그래프 아키텍처에 대한 6-node fully connected 그래프의 마이너 임베딩 예.


<div class="callout" style="display:flex;width:100%;border-radius:4px;background:rgb(241,241,239);padding: 16px 16px 16px 12px;">
<div style="display:flex;align-items:center;justify-content:center;height:24px;width:24px;border-radius:0.25em;flex-shrink:0;">💡</div>
<div style="white-space:pre-wrap;word-break:break-word;caret-color:rgb(55, 53, 47);margin-left:8px;padding-left:2px;padding-right:2px;"><strong>마이너 임베딩 (Minor embedding)<br /><br /></strong>필요한 큐비트 수가 부족한 경우에 사용되는 기술.<br />문제를 해결하기 위해 필요한 큐비트를 양자 컴퓨터의 큐비트로 매핑하는 과정을 말하며,<br />이를 통해 큐비트 수가 부족한 문제를 효율적으로 해결할 수 있다.</div>
</div>


---


물리 기반 Ising 솔버의 성능 평가 지표는 연결성(connectivity)과 가중치 해상도(weight resolution)부터 솔루션 시간(time-to-solution)과 솔루션 에너지(energy-to-solution) 데이터까지 다양하게 나타납니다. 연결성(connectivity)은 주어진 스핀과 상호 작용하는 스핀의 수를 나타냅니다. 예를 들어, 각 스핀이 주변의 스핀과 카디널 방향으로 상호 작용할 수 있는 배열 아키텍처의 경우, 연결성은 4입니다. King의 그래프 토폴로지는 연결성이 8이며, D-Wave의 양자 어닐러의 연결성은 6에서 20까지 다양합니다. [_**[그림 1b]**_](/393f664b73e942238e8d31b34c2b7bd8#bb0da212349645be8ed3e07925c3cb1b)


고연결성이 높은 그래프 토폴로지는 임의의 그래프를 하드웨어에 임베딩하는 것을 더 쉽게 만듭니다. 그러나 고연결성을 갖는 임의의 그래프를 연결성이 제한된 하드웨어에 임베딩하는 것은 계산 비용이 많이 듭니다. 그래프 임베딩은 스핀을 복제하여 Ising 하드웨어가 제한된 연결성을 초과하는 연결성을 증가시킵니다. 6개 노드의 완전 연결 그래프에 대한 두 가지 임베딩 예제가 [_**[그림 1c]**_](/393f664b73e942238e8d31b34c2b7bd8#bb0da212349645be8ed3e07925c3cb1b) 에 표시되어 있습니다. 고유한 토폴로지 때문에 예제 그래프를 임베딩하는 데 필요한 물리적 스핀의 수는 Chimera 그래프 아키텍처의 경우 18개이고, King 그래프 아키텍처의 경우 30개입니다. 제한된 연결성 하드웨어에 대한 임베딩 가능한 동형 그래프를 생성하려면 상당한 계산이 필요하며, 이는 고효율의 물리 기반 솔버를 사용하는 목적을 방해합니다. all-to-all 연결성을 갖지 않는 Ising 솔버의 또 다른 단점은 복제된 스핀이 항상 동일한 상태로 해결되지 않을 수 있기 때문에 솔루션 정확도가 손실될 수 있다는 것입니다.


이 논문에서는 all-to-all연결된 Ising 솔버 유닛에 대해 작성했습니다. 이 솔버는 하이브리드 컴퓨팅 알고리즘에서 사용될 수 있으며, 기존 아키텍처의 연결성 제한을 극복할 수 있습니다. 접근 방식을 검증하기 위해, 우리는 65나노미터의 CMOS 기술을 사용하여 통합 회로 칩을 제작하고, 다양한 문제 크기와 그래프 밀도에 대한 성공 확률, 솔루션 시간, 전력 소비 및 진동 주기와 같은 지표를 측정합니다.


## all-to-all 연결된 결합된 링 진동자(Oscillator) 아키텍처


**[그림 2].** 제안된 all-to-all 배열 아키텍처 및 칩 레이아웃.


![1](/assets/img/2024-04-19-Ising-solver-chip.md/1.png)_Untitled.png_


**[그림 2a].** all-to-all 연결된 배열과 단위 타일 회로.
**[그림 2b].** 제안된 배열에 5개의 스핀(참조 스핀 1개 포함) 완전 그래프의 매핑을 보여주는 간략한 예시.
**[그림 2c].** 65나노미터 테스트 칩의 전체 칩 레이아웃 및 단일 유닛 타일 레이아웃.


[_**[그림 2a](왼쪽)**_](/393f664b73e942238e8d31b34c2b7bd8#bb0da212349645be8ed3e07925c3cb1b)은 우리의 완전 연결된 아키텍처를 보여줍니다. 이 아키텍처는 n+1개의 수평 진동자, n+1개의 수직 진동자, 그리고 각 교차 위치마다 $J_{ij}$ 를 위한 $W$와 $h_i$를 위한 $L$로 표시된 결합 회로로 구성됩니다. 각 수평 진동자는 대각 셀에서 해당하는 수직 진동자와 점 기호로 연결되어, 같은 위상 정보를 가지는 단일 물리적 진동자를 형성합니다. $s_{LF}$(로컬 필드 스핀)로 표시된 페어 진동자는 위상 동기화되어 전체 배열에 대한 타이밍 참조 역할을 합니다. $s_{LF}$의 스핀 값은 +1로 고정되어 있으며, $s_i$의 스핀 값은 $s_{LF}$와의 위상이 같은지 다른지에 따라 +1 또는 -1입니다. sLF가 고정된 참조(+1)로 작용하기 때문에, $s_{LF}$와 다른 스핀 $s_i$ 간의 결합 항은 Ising Hamiltonian 방정식에서 로컬 필드 항 $h_i$가 됩니다. 배열의 하단과 오른쪽 가장자리에 있는 결합 회로는 '$L$'로 표시되어 $h_i$ 가중치를 구현합니다. 한편, $s_i$와 $s_j$ 사이의 교차점은 '$W$'로 표시되어 결합 가중치 $j_{ij}$를 구현합니다. 배열 내의 두 위치(행 i, 열 j)와 (행 j, 열 i)는 그래프가 무방향인지라 동일한 가중치 $j_{ij}$를 프로그래밍하는 데 사용될 수 있습니다. 진동자 $i$와 진동자 $j$ 사이의 결합 가중치는 두 대각 위치 중 어느 것에서든 구현될 수 있기 때문에, 최대 달성 가능한 가중치 해상도는 단일 결합 위치의 최대 가중치의 두 배입니다. 각 결합 셀이 {+7, +6, ..., -6, -7}과 같은 정수 가중치 값을 제공할 수 있다면, 두 대각 셀을 결합하여 {+14, +13, ..., -13, -14}과 같은 가중치 값을 제공할 수 있습니다.


강하게 결합된 링 오실레이터 쌍은 전체 배열에 걸쳐 같은 위상 정보를 운반하며, 다른 모든 링 오실레이터 쌍과 교차하여 all-to-all 연결성을 가능하게 합니다. 상세한 위상 샘플링 회로 도식은 [_**[그림 2a(우측 하단)]**_](/393f664b73e942238e8d31b34c2b7bd8#bb0da212349645be8ed3e07925c3cb1b) 에 표시되어 있습니다. 일련의 표준 플립플롭을 기반으로 한 위상 샘플링 회로는 수직 및 수평 진동자 신호 사이의 상대적인 위상 차이를 비교합니다. 베타에 나타낸 XNOR(배타적 NOR) 게이트는 $RO_{HOR}$와 $RO_{VER}$진동자 신호의 위상을 비교합니다. XNOR 게이트의 듀티 사이클은 위상 차이가 작아짐에 따라 증가합니다. `SAMPLE_TRIG` 신호는 지연 체인을 통해 전파되는 XNOR 출력을 샘플링합니다. 지연 체인의 총 지연은 전체 진동 주기와 대략적으로 일치하여, 전체 주기에 고르게 분포된 여덟 개의 샘플 비트를 제공합니다.


<div class="callout" style="display:flex;width:100%;border-radius:4px;background:rgb(241,241,239);padding: 16px 16px 16px 12px;">
<div style="display:flex;align-items:center;justify-content:center;height:24px;width:24px;border-radius:0.25em;flex-shrink:0;">💡</div>
<div style="white-space:pre-wrap;word-break:break-word;caret-color:rgb(55, 53, 47);margin-left:8px;padding-left:2px;padding-right:2px;"><strong>Duty cycle</strong> : 신호의 한 주기에서 신호가 켜져있는 시간의 비율을 백분율로 나타낸 수치</div>
</div>


[[그림 2b]](/393f664b73e942238e8d31b34c2b7bd8#bb0da212349645be8ed3e07925c3cb1b) 는 4개 변수의 Ising Hamiltonian이 all-to-all 배열에 매핑된 예를 보여줍니다. 로컬 필드 참조를 위해 별도의 오실레이터 쌍이 필요하기 때문에 전체 오실레이터 쌍의 총 수는 다섯입니다. 로컬 필드 가중치 $h_i$는 배열의 하단과 오른쪽 가장자리에 프로그래밍되고, $J_{ij}$ 가중치는 배열 전체에 프로그래밍됩니다. [_**[그림 2b(오른쪽)]**_](/393f664b73e942238e8d31b34c2b7bd8#b04ae4e2de494ddc83ae6c161e7af408) 은 동일한 색상을 사용하여 강조된 대각선을 벗어난 $J_{ij}$ 및 $J_{ji}$ 셀을 보여줍니다.


<div class="callout" style="display:flex;width:100%;border-radius:4px;background:rgb(241,241,239);padding: 16px 16px 16px 12px;">
<div style="display:flex;align-items:center;justify-content:center;height:24px;width:24px;border-radius:0.25em;flex-shrink:0;">💡</div>
<div style="white-space:pre-wrap;word-break:break-word;caret-color:rgb(55, 53, 47);margin-left:8px;padding-left:2px;padding-right:2px;">Hamiltonian<i>(에르미트)</i> : 양자 시스템의 에너지를 나타내는 연산자</div>
</div>


[_**[그림 2c]**_](/393f664b73e942238e8d31b34c2b7bd8#bb0da212349645be8ed3e07925c3cb1b) 에는 전체 칩 레이아웃과 단일 결합 회로의 레이아웃이 나와 있습니다. 1.8mm × 1.0mm 칩은 표준 65나노미터 CMOS 기술로 제작되었으며 명목 공급 전압은 1.2V입니다. 각 결합 셀은 13.0μm × 8.5μm의 면적을 차지하며, 6개의 정적 랜덤 액세스 메모리 셀, 14개의 인버터 및 14개의 트랜스미션 게이트가 포함되어 있습니다. 이 칩은 50개의 스핀 모두 연결된 4개의 모든-모든 배열로 구성되어 있으며, 가장자리 결합 셀을 통해 100개의 스핀의 슈퍼 배열을 형성할 수 있습니다. 가장자리 결합 셀은 일반 결합 셀과 동일하며, 최대 결합 강도는 +7입니다. 초기 테스트 결과, 최대 결합 강도가 +7임에도 불구하고 사분면을 걸쳐 연결된 스핀들이 완전히 잠겨 있지 않아 완전한 100개 스핀 배열의 솔루션 정확도가 낮아졌습니다. 따라서 우리는 먼저 50개 스핀 사분면의 테스트에 중점을 두었습니다.


$J_{ij}$ 및 $h_i$ 가중치 회로의 도식과 짧은 대각 셀이 [_**[그림 2a(오른쪽)]**_](/393f664b73e942238e8d31b34c2b7bd8#bb0da212349645be8ed3e07925c3cb1b)에 제공됩니다. '$W$'와 '$L$' 셀에 대한 결합 회로는 동일하며, 이는

1. 회로 레이아웃이 모든 배열 크기에 확장 가능
2. 공정-전압-온도 변동에 대해 결합 가중치 및 로컬 필드 편향이 좋은 일치 속성을 갖도록 보장

저희 가중치 회로는 전송 게이트(즉, 병렬로 연결된 p형 및 n형 트랜지스터)를 사용하여 대칭 저항성 결합을 제공합니다. 전송 게이트를 사용하여 두 개의 오실레이터 신호를 연결하면 저항성 결합 상호작용을 통해 두 신호를 같은 위상에 잠그게 됩니다. 두 개의 인버터와 두 개의 전송 게이트로 구성된 단위 결합 단계([_**[그림 2a]**_](/393f664b73e942238e8d31b34c2b7bd8#bb0da212349645be8ed3e07925c3cb1b), 빨강 및 파랑)가 연속적으로 연결되어 다중 비트 결합 강도를 제공합니다. 단위 결합 단계는 +1, 0 또는 -1의 효과적인 결합 가중치를 제공합니다. 예를 들어, 파란색(빨간색) 전송 게이트를 켜면 두 신호를 같은(다른) 위상에 잠그게 되어 +1(-1)의 결합 강도를 제공합니다. 두 전송 게이트를 모두 끄면 결합 강도가 0이 됩니다. 가중치 수준의 수는 결합 회로의 딜레이 단계 수에 비례합니다. 저희의 칩 구현에서는, 양방향 경로마다 7개의 전송 게이트를 통한 양수 결합 및 음수 결합에 연결된 수평/수직 경로 당 7개의 인버터 단계를 갖는 결합 셀을 사용했습니다. 이는 각 유닛 타일당 15개의 구별되는 가중치 수준(-7부터 +7까지)을 제공합니다. 여섯 개의 정적 랜덤 액세스 메모리 셀은 가중치 제어 비트를 저장하며, 양수 결합 제어와 음수 결합 제어를 위한 세 개의 비트가 있습니다. 대각선 결합 위치(행 $i$, 열 $j$) 및 (행 $j$, 열 $i$)를 활용하여 각 결합에 대해 -14부터 +14까지의 정수 가중치 값을 프로그래밍할 수 있습니다. 제안된 연속 가중치 회로의 주요 장점은 단순히 더 많은(또는 더 적은) 단계를 연속하여 가중치 해상도를 증가시킬(또는 감소시킬) 수 있다는 것입니다. 이 특성은 고급 CMOS 기술에서 변동 효과를 완화하기 위해 더 균일하고 일정한 단계 크기를 제공할 수 있어 중요합니다.


**[그림 3]. 제안된 멀티비트 결합 가중치 회로와 가중치 구성**


![2](/assets/img/2024-04-19-Ising-solver-chip.md/2.png)_Untitled.png_


**[그림 3a].** 각 결합 단계는 인버터 쌍, 양수 결합을 위한 전송 게이트(파란색으로 강조됨), 음수 결합을 위한 전송 게이트(빨간색으로 강조됨)로 구성됩니다. 유닛 단계는 다중 비트 양수 또는 음수 결합을 가능하게 하기 위해 연속적으로 연결됩니다.


**[그림 3b].** 정수 가중치 값(-N부터 +N까지)에 대한 결합 가중치 구성입니다.


[**[그림 3]**](/393f664b73e942238e8d31b34c2b7bd8#5c3df9756acf41eaa07560515944efce)은 제안된 결합 회로의 서로 다른 가중치 구성을 보여줍니다. 양수와 음수 구성 모두 활성화된 전송 게이트의 수에 비례하여 결합 강도가 결정됩니다. 더 강한 결합 강도는 양의 방향이나 음의 방향으로의 잠금 시간이 짧아집니다. 잠금 시간은 활성화된 전송 게이트의 수에 반비례합니다. 그림 3에서 보듯이, 파란색 CMOS 전송 게이트는 두 개의 오실레이터 경로를 병렬 구성으로 연결하여 양의 가중치를 구현하는 데 사용됩니다. 빨간색 결합 장치는 연결을 한 인버터 단계만큼 이동하여 음수 가중치를 구현하는 데 사용됩니다. 이 구성은 두 개의 오실레이터로부터의 반대 위상 신호 간의 결합을 강제합니다.


## all-to-all Ising 솔버 칩 측정 결과

