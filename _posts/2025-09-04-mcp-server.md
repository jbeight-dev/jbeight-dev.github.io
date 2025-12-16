---
layout: single
title: "MCP Server 배포 해주세요."
date: 2025-09-04 01:00:00 +0900
categories: MCP 업무일지
share: true
---

## test.py

" 매니저님 test.py 개발에 배포 해주실 수 있나요?"

test.py 라니 난이도 1레벨 포스의 파일을 전달 받았다.
열어보니 import FastMCP 로 시작하는 파일이였고,
나는 파일명을 바꿨다. rag_mcp_server.py

## 못해요를 못해요.

'못해요' 하면 되는데 배포 해보고 싶었다.
FastMCP QuickStart 예제를 실행해보고, 혼자 파고 들었다.
배포는 쉬웠다.
문제는 client '어떻게 호출해서 쓰는 놈이여..'

또 다시 전달받은 파일 client.py
'아 호출은 이 파일로 하시면 돼요.'
처음에 같이 줬으면 고마웠을텐데 하하하

mcp_server.py를 배포한 Pod 에서는 응답이 잘 나왔다.
회사 내 이런 저런 제약 사항 때문에 테스트 화면을 개발해서 Pod로 배포해서 테스트 하기로 마음 먹었다. flask 기반 웹 화면을 만들어서 AKS에 배포했다. (Vibe Coding 덕분에 이정도 PoC 화면은 금방 뚝딱 만들고 배포할 수 있다.)

![에러를 해결하는 GPT와 나](/assets/images/2025-09-05-mcpserver.png)

에러가 났다. 하나씩 해결했다.
결국 배포와 웹 클라이언트에서 호출 성공했다:) 룰루
MCP Server 의 인증관련 부분은 다시 자료를 봐야할 거 같다.
넘 졸려 흑흑 화이팅!

## 🧪 MCP 서버 배포 및 호출 테스트 요약

### ✅ 1. MCP 서버 세션 ID 오류

- **오류 메시지**:  
  `No session ID received`

- **문제 원인**:  
  `Mcp-Session-Id` 헤더가 응답에 있음에도, 클라이언트에서 인식하지 못함

- **Ingress 설정 해결 방법**:

  ```yaml
  nginx.ingress.kubernetes.io/cors-expose-headers: "Mcp-Session-Id"
  ```

- **주의**:  
  해당 설정은 **서버(Ingress)** 에 적용해야 함

---

### ✅ 2. CORS 및 OPTIONS 요청 404 오류

- **현상**:  
  HTML에서 MCP 서버 호출 시 CORS 차단, `OPTIONS /` 요청이 404 응답

- **Ingress 설정 예시**:

  ```yaml
  nginx.ingress.kubernetes.io/enable-cors: "true"
  nginx.ingress.kubernetes.io/cors-allow-methods: "PUT, GET, POST, OPTIONS"
  nginx.ingress.kubernetes.io/cors-allow-headers: "Authorization, Content-Type, Mcp-Session-Id"
  nginx.ingress.kubernetes.io/cors-allow-origin: "*"
  ```

---

### ✅ 3. Pod 간 통신 중 도메인 해석 실패 (`ERR_NAME_NOT_RESOLVED`)

- **현상**:  
  동일 네임스페이스 내 Pod에서 MCP 도메인 (`http://rag-mcp`) 호출 시 DNS 오류 발생

- **원인**:  
  Ingress 기반 호출 시 DNS 등록 누락 또는 설정 오류

- **해결**:
  - `Service DNS`: `http://<service>.<namespace>.svc.cluster.local` 형태로 직접 호출
  - 또는 `ClusterIP` 노출로 내부 접근 가능하게 구성
