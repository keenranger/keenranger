---
title: "restrospective 2022"
date: 2022-12-24T18:38:54+09:00
slug: "restrospective-2022"
draft: true
---

## 2022년을 돌아보며…

![커밋이력](https://user-images.githubusercontent.com/18392918/209499014-711432ee-30d7-4985-b397-54a54fea393c.png)  
커밋 이력을 돌아보니 누가 제초제라도 들이 부은 양 잔디밭이 황폐하다. 중간에 논문 쓴다는 핑계로 오랫동안 쉬었다… 그래도 올 한 해 해본 것들을 분야별로 모아 정리했다.

---

## Infrastructure

### Get Started with Infrastructure

CTS 출시 당일 새벽 개발이 완료되어 안도했으나, 배포 과정은 만만치 않았다… 당일 아침 Tony에게 전화걸어 이번주 배포가 힘들것 같다며 (마음속으로) 울던 기억이 아직도 생생하다.
![뭐가뭔지…](https://user-images.githubusercontent.com/18392918/209498400-51295002-2249-413b-8bb7-81c0c0324aa1.png)  
지금봐도 복잡한 AWS 콘솔은 첫 배포 당시 막막하기만 하였다. GCP에서 하던 것 처럼 간단하게 배포가 가능할 줄 알았으나, '제대로' 배포하는 과정은 공부할 것도 많고 어려웠다. 공유기 설정 정도는 해봤으나 네트워킹 관련한 경험이 없었기 떄문에 첫 AWS 배포는 씁쓸한 기억으로 남아있다.

### 좀 더 쉽게 할 순 없을까

inner 출시를 준비하며, 이전처럼 고생하기는 싫어 다양한 방법을 물색해보며 CTS 배포를 되돌아보았다. 당시 내가 잘못 판단했던 것은

- 컴파일이 사용한 Go를 사용하면서도 대안을 모른체 컨테이너 기반으로 배포한 점(사실 아는게 컨테이너 뿐이었다)
- 간단한 작업만이 요구됨에도 좀 더 가벼운 product를 사용하지 않은 점

이 있었고, inner 또한 무거운 제품이 아니었기에 이번에는 서버리스의 형태로 배포하기로 다짐하였다.

inner 서비스를 위해 AWS Lambda를 사용하였고, 이를 배포하기 위해서 AWS SAM(Serverless Application Model)을 처음 사용해보았다. 기존에 행해오던 콘솔 기반의 배포에서 조금 벗어나,
**IaC**(Infrastructure as a Code)를 통해 빠르고 효율적으로 배포하는 법을 배웠다.

[<img src="https://user-images.githubusercontent.com/18392918/209504182-74373dc3-c72c-40e8-8b2c-8309c6169d79.png" height="600">](https://user-images.githubusercontent.com/18392918/209504182-74373dc3-c72c-40e8-8b2c-8309c6169d79.png)

### 그렇다면 지금은?

SAM을 이용해 배포하면서 IaC의 편리함을 느끼고, 현재 olympus와 dunkirk는 인프라 구축부터 대표적인 IaC 오픈소스 프로그램인 [Terraform](https://www.terraform.io/)을 이용해서 작업하고있다.  
[<img src="https://user-images.githubusercontent.com/18392918/209506618-1f92c7da-fad7-4a21-aa75-cda06208aa8e.png" height="600">](https://user-images.githubusercontent.com/18392918/209506618-1f92c7da-fad7-4a21-aa75-cda06208aa8e.png)  
[<img src="https://user-images.githubusercontent.com/18392918/209508825-6f28b5fd-94fe-415e-9d3c-4664899e2e52.svg" height="600">](https://user-images.githubusercontent.com/18392918/209508825-6f28b5fd-94fe-415e-9d3c-4664899e2e52.svg)  
아주 편하다 :)

---

## Devlopment

인프라에 밀려 개발을 등한시했던것 같지만 나름 정리해보면

### Auth 관련

각종 소셜로그인들을 구현하며 이제서야 제대로 OAuth 2.0에 대해 제대로 이해하였다. 내년에는 [FIDO2](https://fidoalliance.org/fido2/)와 같이 새로운 인증도 구현해보고 싶다.

### Documentation

빠른 개발을 위해 문서화 작업을 나중으로 미뤄오며 작업해왔는데, olympus 구현을 시점으로 개발하며 문서화를 빼놓지 않기로 결심했다.  
 _프론트엔드 개발자분들도 조금 편해지지 않았을까?_  
![swagger](https://user-images.githubusercontent.com/18392918/209507793-0626c98a-b649-4263-be39-c3f285e05e3f.png)

---

## Productivity

AI의 발달로 그 어느때보다도 편리한 소프트웨어가 나온 한해인것 같다. 애용하고 있는 것들을 소개하면

### Github Copilot

![Github Copilot](https://user-images.githubusercontent.com/18392918/209507251-352592d3-cfb8-4816-9254-613f14333003.gif)

사용 후 단순 반복 작업 효율이 매우 높아졌다. 이제 없으면 개발 못해요 :(

### OpenAI Chat

![OpenAI Chat0](https://user-images.githubusercontent.com/18392918/209504494-e163cf1b-6cb6-4d69-b23d-f0820123cadc.png)
![OpenAI Chat1](https://user-images.githubusercontent.com/18392918/209504393-2b4fa72e-8b73-4283-88e8-460075b87e15.png)

개발내외적으로 많은 도움을 받으며 사용하고 있다. 혹시 아직 안써보신 분은 한번 써보시는 것을 추천드려요.

---

## 내년에는

'그곳'과의 대규모 서비스가 예정되어 있는 시점에서, [kubernetes](https://kubernetes.io/)와 같이 대규모 분산 시스템을 이용한 엔진 구현이 가장 먼저일것 같다.

---

## 정리하면

- 졸업 :)
- IaC
- Serverless
- OAuth(Social login)

정도가 올해 하고 배운 것이 아닐까...

돌아보고나니 생각보다 한게 많지 않아 아쉬움도 남는다. 내년에는 더 열심히 파이팅해보는 걸로 :)
