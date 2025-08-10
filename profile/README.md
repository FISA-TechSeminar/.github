# WeMakeIT – 금융권 트래픽 관리 실증 실험 프로젝트

**“Redis 캐싱으로 금융 서비스 안정성 강화”**  

---

## 📢 프로젝트 소개
[![발표 영상](https://img.youtube.com/vi/mDGNjxhtFtU/0.jpg)](https://www.youtube.com/watch?v=mDGNjxhtFtU)

🎥 [발표 영상 보러가기](https://www.youtube.com/watch?v=mDGNjxhtFtU)

- 금융 서비스는 **안정성과 신뢰성**이 필수입니다.

- 전통적인 3-Tier 구조 시스템은 트래픽 폭증 시 DB와 API 서버에 병목이 쉽게 발생합니다.

- 우리는 **개발자 관점**에서 실질적인 성능 개선 방법을 탐구하고, 캐싱 전략을 적용해 병목 현상을 완화했습니다.

- 이를 실험으로 검증해 성능 개선 효과를 수치로 확인했습니다.

---

## 🚩 핵심 문제의식
<img width="700" alt="image" src="https://github.com/user-attachments/assets/30ba04d1-cd53-49af-9d3b-1078d077d186" />
<img width="700" alt="image" src="https://github.com/user-attachments/assets/d8e308d7-adaa-41e8-99da-cc8231eca2a6" />

👉🏻 단순 기능 구현이 아닌, **트래픽 관리와 시스템 안정성**이 핵심 과제

---

## 🎯 실험 목표

1. **실제 금융권과 유사한 3-Tier 구조**에서
2. **캐시 미적용/Cache Update/Cache Evict** 전략을 비교
3. **k6 + Grafana + Prometheus**로 대규모 트래픽 시뮬레이션
4. **DB 부하, 응답속도, 에러율** 등 정량적 지표 기반 성능 분석

---

## 🛠️ 기술 스택

- **Spring Boot** – API 서버 및 비즈니스 로직
- **Redis** – 캐싱 및 분산락 구현
- **Oracle** – RDBMS
- **k6** – 부하 테스트
- **Prometheus, Grafana** – 모니터링 및 시각화

---

## 📜 실험 시나리오

| 전략           | 설명                 | 기대 효과                |
| ------------ | ------------------ | -------------------- |
| No Cache     | 모든 요청을 DB 직접 조회    | 병목·응답 지연·장애 위험       |
| Cache Update | 값 변경 시 캐시 갱신       | 빠른 응답, DB 부하 완화      |
| Cache Evict  | 값 변경 시 캐시 삭제       | 정합성↑, hit/miss 비교 가능 |


> **API**  
> - 계좌 이자 조회, 송금, 상세조회 등 실제 금융 서비스 패턴 반영
---
## 🧪 테스트 조건
- 👥 동시 사용자: 100명

- ⏱ 지속 시간: 10분
---

## 📊 측정 핵심 지표

- **처리량:** 초당 요청 수(RPS), 총 처리 요청 수
- **응답속도:** 평균, 최대, p95 등 분포
- **에러율:** 실패 요청 비율 (성공률)
- **시스템 리소스:** CPU, 메모리 등 (Prometheus 활용)

---
## 🏁 성능 테스트 결과 요약
| 전략           | 총 처리 건수 | 평균 RPS | 
| ------------ | ------- | ------ |
| No Cache     | 23,715  | 39.0   | 
| Cache Update | 59,001  | 99.2   | 
| Cache Evict  | 59,254  | 96.7   |

- **캐시 미적용:** DB 과부하, 응답 지연, 장애 발생 가능성 높음
- **캐시 Update/Evict:** 모두 성능 개선 효과
  - 테스트 환경에서는 약 2.5% 성능 차이  
    (트래픽·캐시 미스 폭증 시 차이 더 커질 수 있음)
- **100% 성공률, 평균 39 RPS, 최대 53.5 RPS**  
  → 실제 금융 서비스 요구치에 근접하는 안정성 확인

💡 Cache Update가 RPS는 높은데 Cache Evict보다 총 처리 건수가 적은 이유
→ Cache Evict 전략은 값 변경 시 캐시를 삭제하므로, 이후 해당 키 조회 시 캐시 미스가 발생.
이 미스 요청이 DB를 거쳐 처리되지만, 테스트 집계에서는 이것도 총 처리 건수에 포함되기 때문에 건수가 더 높게 나타남.

--- 

## 💡 분석 결과 & 기술적 인사이트
**📊 분석 결과**

- Cache Update – 성능 최고(99.2 req/s), 그러나 구현 복잡·동시성 제어 부담

- Cache Evict – 2.5% 느리지만 완벽한 데이터 정합성·유지보수성 우수

금융권 특성상 정합성과 신뢰성 > 미미한 성능 차이 → 최종 채택: Cache Evict

**🛠 기술적 인사이트**

- 트래픽 관리·캐싱 전략 설계는 실무 경쟁력을 좌우하는 핵심 역량

- 단순 기능 구현보다 시스템 구조·운영 관점에서의 문제 해결 능력이 중요

- 실험, 분석, 협업 과정을 통해 팀 모두가 성장  
  → **함께 고민하고 직접 검증한 경험**이 가장 값짐


---

## 🤝 팀 & 역할 소개
- **WeMakeIT** 팀은 기술 토론, 실험, 결과 공유까지 전원 적극 참여
- 각자 역할(백엔드, 인프라, 테스트, 시각화 등)을 분담해  
  **함께 문제를 정의·해결·성장**하는 문화를 경험
<img width="700" src="https://github.com/user-attachments/assets/63badbd0-fdfc-4843-980d-cb77983d5aeb">

---

## 📸 대시보드/실험 화면

- 3개 전략 비교 표  
  <img width="700" height="200" alt="스크린샷 2025-08-10 오후 1 26 26" src="https://github.com/user-attachments/assets/7220472a-e543-4042-ae00-04e003aa4d46" />
- Cache Update Grafana 대시보드  
  <img width="650" height="300" alt="image" src="https://github.com/user-attachments/assets/ac0bb36a-b68a-4a6a-9154-87ad8aacfd45" />
- Cache Evict Grafana 대시보드  
  <img width="650" height="300" alt="image" src="https://github.com/user-attachments/assets/6619a54d-d8dd-40e7-a4a6-59652a464bd2" />


---
> 이번 프로젝트는 개발자로서 금융권 IT의 안정성과 신뢰성에 기여할 수 있는 의미 있는 경험이었습니다.
> 은행 시스템에서 발생할 수 있는 병목 현상을 직접 다뤄보고, 이를 해결해보는 과정에서 많은 것을 배우고 성장할 수 있었습니다.
<br>

**💬 문의·협업·피드백은 PR/이슈/디스커션으로 언제든 환영합니다!**

