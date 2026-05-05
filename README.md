Minecraft 거래소 데이터 수집 & API 시스템 설계

---

1. 전체 구조

- Minecraft 클라이언트: 데이터 수집
- 서버(Node.js): 데이터 처리 및 API 제공
- DB(Supabase): 데이터 저장
- 사용자: 웹 / 디스코드 / 알림 시스템

---

2. 클라이언트 설계 (Fabric 모드)

역할

- 서버 접속 유지
- 거래소 GUI 감지
- 아이템 데이터 추출
- 서버로 전송

기능

- 자동 재접속
- GUI 탐지 및 반복 접근
- 슬롯 데이터 파싱
- JSON 변환 후 전송

데이터 구조

{
  "item": "string",
  "price": number,
  "seller": "string",
  "timestamp": number
}

---

3. 서버 설계 (Node.js)

구성

- Express: API 서버
- WebSocket: 실시간 데이터 전달
- Supabase: 데이터베이스

역할

- 데이터 수신
- 데이터 정제
- 시세 계산
- API 제공

---

4. 데이터베이스 구조

items

- id
- name
- category

listings

- id
- item_id
- price
- seller
- timestamp
- source_bot

price_cache

- item_id
- avg_price
- min_price
- max_price
- updated_at

---

5. 데이터 처리 로직

정제

- 중복 데이터 제거
- 동일 아이템 그룹화

시세 계산

- 평균값 계산
- 중앙값 기반 보정

이상치 제거

- 평균 대비 ±50% 초과 데이터 제외

---

6. 수집 전략

- GUI 이벤트 발생 시 데이터 수집
- 주기적 전체 스캔 (30~60초)

---

7. 안정성 설계

- 자동 재접속
- 실패 데이터 재전송 큐
- 로그 저장
- Rate Limit 대응

---

8. 확장 구조

- 다중 클라이언트(봇) 운영
- 서버 역할 분리 (수집 / API / 알림)
- 디스코드 알림 시스템 연동
- 가격 히스토리 및 그래프 제공

---

9. 전체 흐름

[Minecraft Client]
        ↓
   JSON 데이터 전송
        ↓
[Node.js Server]
 ├─ 데이터 정제
 ├─ 시세 계산
 ├─ DB 저장
 └─ API 제공
        ↓
[User Services]
 ├─ 웹사이트
 ├─ 디스코드 봇
 └─ 알림 시스템
