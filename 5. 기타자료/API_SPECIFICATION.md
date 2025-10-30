# 🧾 API 상세 명세서

## 1. 개요

-   본 API 상세 명세서는 루티 시스템 내부에서 사용하기 위한 API 사용 방법을 정리한 문서입니다.
-   **통신 프로토콜:** HTTP 기반으로 `GET`, `POST` 방식으로 데이터를 주고받습니다.
-   **사용 라이브러리:** axios

---

## 2. 공통

### 2.1 응답 구조

-   `resultCode`: HTTP 상태 코드
-   `resultMsg`: 응답 상태를 설명하는 메시지 (대문자 스네이크 케이스)
-   `resultTime`: 서버 응답 시각

#### 성공 응답 예시

```json
{
  "resultCode": 200,
  "resultMsg": "SUCCESS",
  "resultTime": "2025-10-01 09:00:00",
  "data": {
    ...
  }
}
```

-   `resultCode`: 200 (고정)
-   `resultMsg`: "SUCCESS" (고정)

#### 에러 응답 예시

```json
{
    "resultCode": 400,
    "resultMsg": "BAD_REQUEST",
    "resultTime": "2025-10-01 09:00:00"
}
```

-   `resultCode`: 아래 에러 코드표 참고
-   `resultMsg`: 아래 에러 코드표 참고

##### 에러 코드표

| 결과 코드 (`resultCode`) | 결과 메시지 (`resultMsg`) | 설명                  |
| ------------------------ | ------------------------- | --------------------- |
| 200                      | SUCCESS                   | 요청 성공             |
| 400                      | BAD_REQUEST               | 요청 구문 오류        |
| 401                      | UNAUTHORIZED              | 인증 실패             |
| 403                      | FORBIDDEN                 | 접근 권한 없음        |
| 404                      | NOT_FOUND                 | 리소스를 찾을 수 없음 |
| 500                      | INTERNAL_SERVER_ERROR     | 서버 내부 오류        |

---

## 3. 내부 API

### 3.1 회원 API

#### 회원 목록 조회 — [POST] `/member/all_list`

##### Request Body

```json
{
    "type": "",
    "keyword": "",
    "page": 1,
    "limit": 20
}
```

| 필드      | 타입   | 최대 길이 | 필수 여부 | 설명                       |
| --------- | ------ | --------- | --------- | -------------------------- |
| `type`    | String | 100       | 선택      | 검색 조건 (필터링 시 필수) |
| `keyword` | String | 100       | 선택      | 검색어 (필터링 시 필수)    |
| `page`    | Int    | 100       | 선택      | 페이지 번호                |
| `limit`   | Int    | 50        | 선택      | 페이지당 목록 수           |

##### Response Example

```json
{
    "resultCode": 200,
    "resultMsg": "SUCCESS",
    "resultTime": "2025-10-01 09:00:00",
    "data": [{}, {}]
}
```

| 필드   | 타입  | 설명                  |
| ------ | ----- | --------------------- |
| `data` | Array | 조건에 맞는 회원 목록 |

---

#### 회원 정보 조회 — [GET] `/member/info/{memId}`

| Path Variable | 타입   | 설명         |
| ------------- | ------ | ------------ |
| `memId`       | String | 회원 고유 ID |

> 특정 회원의 상세 정보를 조회합니다.

---

### 3.2 상품 API

> (작성 예정)

---

### 3.3 주문 API

> (작성 예정)
