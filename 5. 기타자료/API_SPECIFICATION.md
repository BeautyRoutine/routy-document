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

### 운영자 API

### 3.2.1 상품 목록 조회 — [GET] `/api/admin/products`

설명: 관리자 페이지에서 사용할 상품 목록 전체를 필터링하여 조회합니다.

### Response Example

```json
{
    "resultCode": 200,
    "resultMsg": "SUCCESS",
    "resultTime": "2025-10-01 09:00:00",
     "data": {
        "products": [
            {
              "prdNo":123,
              "prdName":"샘플",
              "prdPrice":25000,
              "prdVolume":200,
              "prdCompany": "제조사",
              "prdStock":100,
              "prdMainCate":"스킨/케어",
              "prdSubCate":"크림",
              "prdImg":"image/src/blabla/123.jpg",
              "prdDesc":"제품설명입니다",
              "prdUpdate": "2025-09-20 09:00:00",
              "prdRegdate":"2025-09-25 09:00:00"
            },
            { ... }
        ],
        "pagination": {
            "page": 1,
            "limit": 20,
            "totalCount": 153,
            "totalPages": 8
        }
    }
}

```

| 필드              | 타입   | 설명                       |
| ----------------- | ------ | -------------------------- |
| `data`            | Object | 응답 데이터 객체           |
| `data.products`   | Array  | 상품의 모든 정보 포함 목록 |
| `data.pagination` | Object | 페이징 정보                |

---

### 3.2.2. 상품 등록 — [POST] `/api/admin/products`

### Request Body

```json
{
  "prdName": "샘플",
  "prdPrice": 25000,
  "prdVolume": 200,
  "prdCompany": "제조사",
  "prdStock": 100,
  "prdMainCate": "스킨/케어",
  "prdSubCate": "크림",
  "prdImg": "image/src/blabla/123.jpg",
  "prdDesc": "제품설명입니다"
}
```

| 필드          | 타입   | 최대 길이 | 필수 여부 | 설명             |
| ------------- | ------ | --------- | --------- | ---------------- |
| `prdName`     | String | 100       | 필수      | 상품명           |
| `prdPrice`    | Int    | 9         | 필수      | 상품가격         |
| `prdVolume`   | Int    | 5         | 필수      | 용량(ml)         |
| `prdCompany`  | String | 100       | 선택      | 제조사           |
| `prdStock`    | Int    | 5         | 필수      | 재고 수량        |
| `prdMainCate` | String | 2         | 필수      | 상품 대분류      |
| `prdSubCate`  | String | 5         | 필수      | 상품 소분류      |
| `prdImg`      | String | 255       | 선택      | 상품 이미지 경로 |
| `prdDesc`     | String | 255       | 선택      | 상품 상세 설명   |

### Response Example

```json
{
  "resultCode": 200,
  "resultMsg": "SUCCESS",
  "resultTime": "2025-10-01 09:00:00",
  "data": {
    "prdNo": 124
  }
}
```

| 필드   | 타입   | 설명                                    |
| ------ | ------ | --------------------------------------- |
| `data` | Object | 생성이 완료된 상품 정보(prdno만 반환함) |

---

### 3.2.3. 상품 수정 — [PUT] `/api/admin/products/{prdNo}`

| Path Variable | 타입 | 설명         |
| ------------- | ---- | ------------ |
| `prdNo`       | Int  | 제품 고유 ID |

### Request Body

```json
{
  "prdName": "수정 상품",
  "prdPrice": 50000,
  "prdVolume": 150,
  "prdCompany": "수정 제조사",
  "prdStock": 200,
  "prdMainCate": "스킨/케어",
  "prdSubCate": "로션",
  "prdImg": "image/src/update.jpg",
  "prdDesc": "수정이 완료된 상품 설명입니다."
}
```

| 필드          | 타입   | 최대 길이 | 필수 여부 | 설명             |
| ------------- | ------ | --------- | --------- | ---------------- |
| `prdName`     | String | 100       | 필수      | 상품명           |
| `prdPrice`    | Int    | 9         | 필수      | 상품가격         |
| `prdVolume`   | Int    | 5         | 필수      | 용량(ml)         |
| `prdCompany`  | String | 100       | 선택      | 제조사           |
| `prdStock`    | Int    | 5         | 필수      | 재고 수량        |
| `prdMainCate` | String | 2         | 필수      | 상품 대분류      |
| `prdSubCate`  | String | 5         | 필수      | 상품 소분류      |
| `prdImg`      | String | 255       | 선택      | 상품 이미지 경로 |
| `prdDesc`     | String | 255       | 선택      | 상품 상세 설명   |
|               |        |           |           |                  |

### Response Example

```json
{
  "resultCode": 200,
  "resultMsg": "SUCCESS",
  "resultTime": "2025-10-01 09:00:00",
  "data": {
    "prdNo": 123,
    "prdName": "샘플",
    "prdPrice": 25000,
    "prdVolume": 200,
    "prdCompany": "제조사",
    "prdStock": 100,
    "prdMainCate": "스킨/케어",
    "prdSubCate": "크림",
    "prdImg": "image/src/blabla/123.jpg",
    "prdDesc": "제품설명입니다",
    "prdUpdate": "2025-09-20 09:00:00",
    "prdRegdate": "2025-09-25 09:00:00"
  }
}
```

| 필드          | 타입   | 설명                |
| ------------- | ------ | ------------------- |
| `data`        | Object | 수정 완료 제품 정보 |
| `prdNo`       | Int    | 상품 고유 id        |
| `prdName`     | String | 상품명              |
| `prdPrice`    | Int    | 상품가격            |
| `prdVolume`   | Int    | 용량(ml)            |
| `prdCompany`  | String | 제조사              |
| `prdStock`    | Int    | 재고 수량           |
| `prdMainCate` | String | 상품 대분류         |
| `prdSubCate`  | String | 상품 소분류         |
| `prdImg`      | String | 상품 이미지 경로    |
| `prdDesc`     | String | 상품 상세 설명      |
| `prdUpdate`   | String | 최종 수정일         |
| `prdRegdate`  | String | 최초 등록일         |

---

3.2.4. 상품 삭제 — [DELETE] `/api/admin/products/{prdNo}`

| Path Variable | 타입 | 설명         |
| ------------- | ---- | ------------ |
| `prdNo`       | Int  | 제품 고유 ID |

### Response Example

```json
{
  "resultCode": 200,
  "resultMsg": "SUCCESS",
  "resultTime": "2025-10-02 11:35:00"
}
```

---

### 사용자 API

### 3.2.5 상품 목록 조회 — [GET] `/api/products`

설명: 사용자가 상품 목록을 필터링하여 조회합니다.

### Response Example

```json
{
    "resultCode": 200,
    "resultMsg": "SUCCESS",
    "resultTime": "2025-10-01 09:00:00",
     "data": {
        "products": [
            {
                "prdName": "샘플",
                "prdPrice": 25000,
                "prdImg": "image/src/product/123.jpg"
            },
            { ... }
        ],
        "pagination": {
            "page": 1,
            "limit": 20,
            "totalCount": 153,
            "totalPages": 8
        }
    }
}

```

| 필드              | 타입   | 설명              |
| ----------------- | ------ | ----------------- |
| `data`            | Object | 응답 데이터 객체  |
| `data.products`   | Array  | 상품 일부 정 목록 |
| `data.pagination` | Object | 페이징 정보       |

---

### 3.2.6 상품 정보 조회 — [GET] `/api/products/{prdNo}`

| Path Variable | 타입 | 설명         |
| ------------- | ---- | ------------ |
| `prdNo`       | Int  | 제품 고유 ID |

### Response Example

```json
{
  "resultCode": 200,
  "resultMsg": "SUCCESS",
  "resultTime": "2025-10-01 09:00:00",
  "data": {
    "prdName": "샘플",
    "prdPrice": 25000,
    "prdVolume": 200,
    "prdCompany": "제조사",
    "prdMainCate": "스킨/케어",
    "prdSubCate": "크림",
    "prdImg": "image/src/blabla/123.jpg",
    "prdDesc": "제품설명입니다"
  }
}
```

| 필드          | 타입   | 설명                  |
| ------------- | ------ | --------------------- |
| `data`        | Object | 사용자 제공 상세 정보 |
| `prdName`     | String | 상품명                |
| `prdPrice`    | Int    | 상품가격              |
| `prdVolume`   | Int    | 용량(ml)              |
| `prdCompany`  | String | 제조사                |
| `prdMainCate` | String | 상품 대분류           |
| `prdSubCate`  | String | 상품 소분류           |
| `prdImg`      | String | 상품 이미지 경로      |
| `prdDesc`     | String | 상품 상세 설명        |

> 특정 제품의 상세 정보를 조회합니다.

---

### 3.3 주문 API

> (작성 예정)
