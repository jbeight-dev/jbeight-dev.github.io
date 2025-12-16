---
layout: single
title: "AI 애플리케이션 구축 - Azure OpenAI 리소스 어디에 만들까"
date: 2025-08-04 20:21:02 +0900
categories: Cloud Azure AI OpenAI
share: true
---

## Azure OPEN AI & Region

제일 많이 듣는 질문 중에 하나

> Q. Azure Open AI 리소스도 KoreaCentral 리전(Region)에 위치해 있나요?  
> A. (내가 구축했던 어플리케이션 기준) No

Korea Central 리전에서도 GPT-4o 모델을 사용할 수 있지만,  
GPT-4.1 시리즈는 지원하지 않고 있다.

그리고 데이터가 해외에서 처리 된다고 한다. (오잉)
Korea Central에는 API Endpoint만 있고, 실제 추론(inference)은 해외 리전에서 수행될 수 있다고 하네.
애플에서 디자인한 아이폰이 중국애서 생산되는 뭐 그런 느낌인데,  
나의 질문이 한국을 떠났다 해외에서 처리되어 답변으로 돌아온다고 보면 된다.

Sweden Central 리전을 사용하는 경우와 비교해보면,
스웨덴을 이용하면 GPT-4.1 시리즈 사용 가능은 물론 풀 서포트를 지원한다고 한다.
(\*풀 서포트: Azure OpenAI 서비스에서 제공하는 모든 기능과 옵션을 최대한 활용하는 것을 의미)

어플리케이션 마다 상황이 다르겠지만,  
네트워크 지연(한국->스웨덴)이 발생하더라도  
성능/처리량(SLA, PTU)이 보장되는 Sweden Central 리전을 우선으로 검토하게 되었다.

![Azure OpenAI](/assets/images/2025-08-03-post-img1.png)

### 참고 URL

- https://learn.microsoft.com/ko-kr/azure/ai-foundry/foundry-models/concepts/models
