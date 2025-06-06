# RAG Model Manager System (Private Project Summary)

> 🏢 **Company:** DB Inc.  
> 📅 **Period:** 2025.02 ~ 현재  
> 🧑‍💻 **Role:** Java Backend Developer (Model Manager)  
> 🛠 **Tech Stack:** Java, Spring Boot, Spring Data JPA, MySQL, Gradle, Swagger, REST API, MSA, JPQL

---

## 🧩 프로젝트 개요

RAG 기반 AI 챗봇 · 지식 · 사전 전처리 시스템에서 사용하는 다양한 AI 모델(OpenAI, Triton 등)을 하나의 시스템에서 통합 관리하기 위해 모델 매니저 시스템을 설계 및 구현했습니다. 본 시스템은 모델 상태 모니터링, 설정값 관리, API 연동, 사용 이력 통계 등 운영 전반을 책임지는 백오피스 솔루션입니다.

> **Note**: 본 프로젝트는 회사의 Private Repository에 포함되어 있어 실제 소스는 비공개입니다.

---

## 🔧 주요 기능

### 1. 모델 등록 / 수정 / 삭제
- Triton 서버 연동 또는 OpenAI/Azure 기반 API 모델 등록 기능
- 사용 중인 모델은 삭제 불가, 논리 삭제 처리로 안전성 확보

### 2. 모델 상태 / 버전 / 설정값 관리
- 상태 흐름: 등록 → 활성 → 사용 → 오류
- JSON 기반 설정값 버전 관리 및 변경 가능 구조
- 모델 사용 이력과 연동되는 상태/버전 관리

### 3. 시스템 연동용 REST API
- Spring Boot + Swagger 기반 REST API 설계 및 문서화
- OpenAI/Azure API Key, Endpoint, Provider 관리 기능 포함
- Triton 모델 상태 사전 검증 API 제공

### 4. 사용 이력 및 통계 조회
- 모델별 사용 이력 및 상태 기반 통계 API 제공
- 챗봇 · 지식 · 사전 시스템 연동

### 5. 모델 상태 검증 및 예외 처리 설계
- GPU 1장 환경에서 동시에 모델 다중 로드 시 오류 방지 로직 설계
- Triton 서버의 실시간 상태를 사용 시점에 검증 (On-demand 동기화)
- 실시간 모니터링 불가능 환경에서의 안정성 확보

### 6. Triton 모델 모니터링 및 배치 기반 상태 동기화
- 17시~09시 사이에만 모델을 '활성' 상태로 유지 (야간 운영 로직)
- Polling 기반(2분 간격) Triton 상태 체크 및 DB 상태 동기화
- Triton 미존재 → '오류' / Triton 존재 & DB '오류' → '회복'
- 모델 활성화 요청 시 최대 5분간 Triton 상태 모니터링 후 실패 시 '오류' 처리

---

## 💡 문제 해결 사례

### 문제 1: GPU 자원 부족으로 인한 모델 충돌
- **문제:** GPU 1장으로는 다수 모델 활성화 불가. 미활성 모델 사용 시 장애 발생
- **해결:** Triton 서버에 모델이 실제 활성화되었는지 '사용 시점'에 동기화 검증. 비활성 시 '오류' 전환 및 알림 제공

### 문제 2: DTO 파생 필드 기반 정렬 불가
- **문제:** `status` 필드는 DB 컬럼이 아니어서 Pageable 정렬 불가. UX 혼선
- **해결:** 전체 조회 후 DTO 변환 → Java에서 status + id 기준 정렬 → PageImpl로 재구성

### 문제 3: 모델 로드 중 충돌 문제
- **문제:** Triton 모델 로딩 중 다른 모델 활성화 시 충돌 발생
- **해결:** 시간대 기반 활성화 배치 및 Polling 도입. 충돌 방지 및 지속적인 상태 동기화로 안정성 확보

---

## ✍️ 요약 문구

> Triton 기반 모델과 OpenAI API 모델을 통합 관리하는 시스템을 설계 · 구현하고, GPU 제약 환경에서도 예외 없는 모델 사용을 보장하기 위한 상태 기반 동기화 로직을 개발했습니다.

---

## 📌 Resume/README용 간단 요약

- 다양한 AI 모델(Triton, OpenAI)을 하나의 시스템에서 통합 관리하는 백엔드 시스템 설계 및 구현  
- GPU 제약 환경에서도 안정적으로 모델을 동기화하기 위한 Polling 기반 상태 검증 및 배치 로직 개발  
- REST API 기반 OpenAI 연동, 모델 설정 버전 관리, 사용 이력 통계 기능 포함

---

> 📝 이 문서는 실제 Private Repository 기반 프로젝트의 요약 문서입니다.  
> 🔒 코드 접근은 불가하지만, 상세 설계/구현 경험은 면접 시 설명 가능합니다.
