# API 상세 명세 복붙용

# 0. 목차

# 📑 API 상세 명세

## 📌 목차

-   [1. 개요](#1-개요)
-   [2. 공통](#2-공통)

    -   [2.1 응답 구조](#21-응답-구조)
        -   [2.1.1 성공 응답 예시](#211-성공-응답-예시)
        -   [2.1.2 에러 응답 예시](#212-에러-응답-예시)

-   [3. 사용자 API - 8080](#3-사용자-api---8080)

    -   [3.1 회원](#31-회원)
    -   [3.2 상품](#32-상품)
    -   [3.3 장바구니](#33-장바구니)
    -   [3.4 관심상품](#34-관심-상품)
    -   [3.5 성분](#35-성분)
    -   [3.6 리뷰](#36-리뷰)

-   [4. 관리자 API - 8085](#4-관리자-api---8085)
    -   [4.1 회원](#41-회원)
    -   [4.2 상품](#42-상품)
    -   [4.7 주문](#47-주문)

# 1. 개요

---

-   본 API 상세 명세서는 루티 시스템 내부에서 사용하기 위한 API 사용 방법을 정리한 문서입니다.
-   **통신 프로토콜:** HTTP 기반으로 `GET`, `POST` 방식으로 데이터를 주고받습니다.
-   **사용 라이브러리:** axios

# 2. 공통

---

## 2.1 응답 구조

-   `resultCode`: HTTP 상태 코드
-   `resultMsg`: 응답 상태를 설명하는 메시지 (대문자 스네이크 케이스)
-   `resultTime`: 서버 응답 시각

### 2.1.1 성공 응답 예시 ✅

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

### 2.1.2 에러 응답 예시 ⛔

```json
{
    "resultCode": 400,
    "resultMsg": "BAD_REQUEST",
    "resultTime": "2025-10-01 09:00:00"
}
```

-   `resultCode`: 아래 에러 코드표 참고
-   `resultMsg`: 아래 에러 코드표 참고
    | 결과 코드 (`resultCode`) | 결과 메시지 (`resultMsg`) | 설명 |
    | ------------------------ | ------------------------- | --------------------- |
    | 200 | SUCCESS | 요청 성공 |
    | 400 | BAD_REQUEST | 요청 구문 오류 |
    | 401 | UNAUTHORIZED | 인증 실패 |
    | 403 | FORBIDDEN | 접근 권한 없음 |
    | 404 | NOT_FOUND | 리소스를 찾을 수 없음 |
    | 500 | INTERNAL_SERVER_ERROR | 서버 내부 오류 |

# 3. 사용자 API - 8080

---

## 3.1. 회원

### [POST] `/member/all_list` : 회원 목록 조회

> 회원 목록을 조회하는 api.
> 검색 기능을 제공합니다.

### 🛎️ Request Body

```json
{
    "type": "",
    "keyword": "",
    "page": 1,
    "limit": 20
}
```

| 필드      | 타입   | 최대 길이 | 필수 / 선택 | 설명                       |
| --------- | ------ | --------- | ----------- | -------------------------- |
| `type`    | String | 100       | 선택        | 검색 조건 (필터링 시 필수) |
| `keyword` | String | 100       | 선택        | 검색어 (필터링 시 필수)    |
| `page`    | Int    | 100       | 선택        | 페이지 번호                |
| `limit`   | Int    | 50        | 선택        | 페이지당 목록 수           |

### 💬 Response Example

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

### [GET] `/member/info/{memId}` : 회원 정보 조회

> `{memId}` : 회원 고유 ID
> 특정 회원의 상세 정보를 조회합니다.

### 🛎️ Request Body

```json

```

| 필드     | 타입   | 최대 길이 | 필수 / 선택 | 설명 |
| -------- | ------ | --------- | ----------- | ---- |
| `필드명` | String | 100       | 선택        | 설명 |
|          |        |           |             |      |

### 💬 Response Example

```json

```

| 필드     | 타입   | 최대 길이 | 설명 |
| -------- | ------ | --------- | ---- |
| `필드명` | String | 100       | 설명 |
|          |        |           |      |

---

### **[GET] `/api/users/me/notifications/count` : 사용자 알림 개수 조회**

> 로그인한 사용자의 알림 개수를 반환합니다.

> 헤더에서 로그인 후 알림 배지 초기화 시 호출됩니다.

---

### **💬 Response Example**

```json
{
    "count": 23
}
```

| **필드** | **타입** | **최대 길이** | **설명**                           |
| -------- | -------- | ------------- | ---------------------------------- |
| count    | Number   | -             | 현재 사용자에게 존재하는 알림 개수 |

---

### **[GET] `/api/cart/count` : 장바구니 상품 개수 조회**

> 로그인한 사용자의 장바구니 내 상품 개수를 반환합니다.

> 헤더에서 로그인 후 장바구니 배지 초기화 시 호출됩니다.

---

### **💬 Response Example**

```json
{
    "count": 3
}
```

| **필드** | **타입** | **최대 길이** | **설명**                       |
| -------- | -------- | ------------- | ------------------------------ |
| count    | Number   | -             | 사용자의 장바구니 내 상품 개수 |

---

## 3.2. 상품

### [GET] `/api/products` : 상품 목록 조회

> 사용자가 상품 목록을 필터링하여 조회합니다.

### 🛎️ Request Body

```json

```

| 필드     | 타입   | 최대 길이 | 필수 / 선택 | 설명 |
| -------- | ------ | --------- | ----------- | ---- |
| `필드명` | String | 100       | 선택        | 설명 |
|          |        |           |             |      |

### 💬 Response Example

```json
{
    "resultCode": 200,
    "resultMsg": "SUCCESS",
    "resultTime": "2025-10-01 09:00:00",
     "data": {
        "products": [
            {
		            "prdNo":"101",
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

| 필드              | 타입   | 최대 길이 | 설명              |
| ----------------- | ------ | --------- | ----------------- |
| `data`            | Object |           | 응답 데이터 객체  |
| `data.products`   | Array  |           | 상품 일부 정 목록 |
| `data.pagination` | Object |           | 페이징 정보       |

---

### [GET] `/api/products/{prdNo}` : 상품 정보 조회

> `{prdNo}` : 제품 고유 ID
> 특정 제품의 상세 정보를 조회합니다.

### 🛎️ Request Body

```json

```

| 필드     | 타입   | 최대 길이 | 필수 / 선택 | 설명 |
| -------- | ------ | --------- | ----------- | ---- |
| `필드명` | String | 100       | 선택        | 설명 |
|          |        |           |             |      |

### 💬 Response Example

```json
{
    "resultCode": 200,
    "resultMsg": "SUCCESS",
    "resultTime": "2025-10-01 09:00:00",
    "data": {
        "prdNo": "101",
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

| 필드          | 타입   | 최대 길이 | 설명                  |
| ------------- | ------ | --------- | --------------------- |
| `data`        | Object |           | 사용자 제공 상세 정보 |
| `prdName`     | String |           | 상품명                |
| `prdPrice`    | Int    |           | 상품가격              |
| `prdVolume`   | Int    |           | 용량(ml)              |
| `prdCompany`  | String |           | 제조사                |
| `prdMainCate` | String |           | 상품 대분류           |
| `prdSubCate`  | String |           | 상품 소분류           |
| `prdImg`      | String |           | 상품 이미지 경로      |
| `prdDesc`     | String |           | 상품 상세 설명        |
| `prdNo`       | int    |           | 상품 번호             |

---

### **[GET] `/api/categories/top` : 상위 카테고리 조회**

> 메인 헤더에 표시할
>
> **상위 카테고리 목록**

> 사용자의 첫 진입 시 자동 호출되어 카테고리 탭을 구성합니다.

---

### **💬 Response Example**

```
[
  "스킨케어",
  "메이크업",
  "바디",
  "헤어",
  "향수"
]
```

| **필드**   | **타입** | **최대 길이** | **설명**           |
| ---------- | -------- | ------------- | ------------------ |
| 카테고리명 | String   | 100           | 상위 카테고리 이름 |

---

### **[GET] `/api/categories/tree` : 카테고리 트리 조회**

> 상위/하위 구조로 된 전체 카테고리 트리 데이터를 반환합니다.

> 헤더의 드롭다운 메뉴 및 전체 카테고리 탐색 시 사용됩니다.

---

### **💬 Response Example**

```
[
  { "title": "스킨케어", "items": ["토너", "로션", "에센스"] },
  { "title": "메이크업", "items": ["파운데이션", "립", "아이섀도우"] }
]
```

| **필드** | **타입**      | **최대 길이** | **설명**           |
| -------- | ------------- | ------------- | ------------------ |
| title    | String        | 100           | 상위 카테고리 이름 |
| items    | Array[String] | -             | 하위 카테고리 목록 |

---

### **[GET] `/api/search/recent` : 최근 검색어 조회**

> 사용자가 최근 입력한 검색어 목록을 반환합니다.

> 검색창 포커스 시 자동 호출됩니다.

---

### **💬 Response Example**

```
[
  "덤패치",
  "수분크림",
  "세럼"
]
```

| **필드** | **타입** | **최대 길이** | **설명**    |
| -------- | -------- | ------------- | ----------- |
| keyword  | String   | 50            | 최근 검색어 |

---

### **[GET] `/api/search/similar-skin` : 유사 피부 타입 인기 검색어 조회**

> 비슷한 피부 타입의 사용자들이 많이 검색한 키워드 목록을 제공합니다.

> 검색창 포커스 시 함께 호출됩니다.

---

### **💬 Response Example**

```
[
  { "keyword": "라스트핑", "trend": "up" },
  { "keyword": "시카크림", "trend": "steady" }
]
```

| **필드** | **타입** | **최대 길이** | **설명**                             |
| -------- | -------- | ------------- | ------------------------------------ |
| keyword  | String   | 50            | 검색 키워드                          |
| trend    | String   | 10            | 트렌드 상태 (“up”, “down”, “steady”) |

---

### **[GET] `/api/search/suggestions?q={keyword}`: 자동완성 검색어 추천**

> 사용자가 검색창에 입력할 때 250ms 디바운스로 호출됩니다.

> 입력된 키워드와 관련된 추천 검색어를 반환합니다.

---

### **💬 Response Example**

```
[
  "시카크림",
  "시카앰플",
  "시카패드"
]
```

| **필드**   | **타입** | **최대 길이** | **설명**    |
| ---------- | -------- | ------------- | ----------- |
| suggestion | String   | 50            | 추천 검색어 |

---

## 3.3. 장바구니

### [GET] `/api/cart` : 장바구니 목록 조회

> 현재 유저의 장바구니 목록 전체를 조회합니다.

### 🛎️ Request Body

```json

```

| 필드     | 타입   | 최대 길이 | 필수 / 선택 | 설명 |
| -------- | ------ | --------- | ----------- | ---- |
| `필드명` | String | 100       | 필수        | 설명 |
|          |        |           |             |      |

### 💬 Response Example

```json
{
    "resultCode": 200,
    "resultMsg": "SUCCESS",
    "resultTime": "2025-11-04 16:30:00",
    "data": {
        "summary": {
            "totalProductAmount": 98000,
            "deliveryFee": 0,
            "finalPaymentAmount": 98000
        },
        "items": [
            {
                "cartItemId": 101,
                "productId": 1,
                "name": "하이드레이팅 세럼",
                "brand": "글로우랩",
                "price": 28000,
                "quantity": 1,
                "imageUrl": "https://.../img.jpg",
                "selected": true,
                "skinAlert": { "type": "recommend", "message": "..." }
            },
            {
                "cartItemId": 102,
                "productId": 2,
                "name": "비타민 C 토너",
                "brand": "글로우랩",
                "price": 28000,
                "quantity": 2,
                "imageUrl": "https://.../img2.jpg",
                "selected": false,
                "skinAlert": null
            }
        ]
    }
}
```

| 필드     | 타입   | 최대 길이 | 설명         |
| -------- | ------ | --------- | ------------ |
| `필드명` | String | 100       | 회원 고유 ID |

---

### [POST] `/api/cart/items` : 장바구니 상품 추가

> 장바구니에 새 상품을 추가하거나, 이미 있는 상품의 수량을 더합니다.

### 🛎️ Request Body

```json
{
    "productId": 3,
    "quantity": 2
}
```

| 필드     | 타입   | 최대 길이 | 필수 / 선택 | 설명 |
| -------- | ------ | --------- | ----------- | ---- |
| `필드명` | String | 100       | 필수        | 설명 |
|          |        |           |             |      |

### 💬 Response Example

```json
{
    "resultCode": 200,
    "resultMsg": "SUCCESS",
    "resultTime": "2025-11-04 16:30:00",
    "data": {
        "summary": {
            "totalProductAmount": 98000,
            "deliveryFee": 0,
            "finalPaymentAmount": 98000
        },
        "items": [
            {
                "cartItemId": 101,
                "productId": 1,
                "name": "하이드레이팅 세럼",
                "brand": "글로우랩",
                "price": 28000,
                "quantity": 1,
                "imageUrl": "https://.../img.jpg",
                "selected": true,
                "skinAlert": { "type": "recommend", "message": "..." }
            },
            {
                "cartItemId": 102,
                "productId": 2,
                "name": "비타민 C 토너",
                "brand": "글로우랩",
                "price": 28000,
                "quantity": 2,
                "imageUrl": "https://.../img2.jpg",
                "selected": false,
                "skinAlert": null
            }
        ]
    }
}
```

| 필드     | 타입   | 최대 길이 | 설명         |
| -------- | ------ | --------- | ------------ |
| `필드명` | String | 100       | 회원 고유 ID |
|          |        |           |              |

---

### [PATCH] `/api/cart/items/{cartItemId}` : 상품 수량 변경

> 장바구니에 담긴 특정 상품의 **수량** 또는 **선택 여부**를 변경합니다.
> `{cartItemId}` : `CART` 테이블의 `cartNo` (장바구니 항목 ID)

### 🛎️ Request Body

```json
{
    "quantity": 3,
    "selected": false
}
```

`quantity` 와 `selected` 둘 중 하나 필수

| 필드       | 타입    | 최대 길이  | 필수 / 선택 | 설명                      |
| ---------- | ------- | ---------- | ----------- | ------------------------- |
| `quantity` | Int     |            | 선택        | 변경 안 할 경우 필드 제외 |
| `selected` | Boolean | true/false | 선택        | 변경 안 할 경우 필드 제외 |

### 💬 Response Example

```json
{
    "resultCode": 200,
    "resultMsg": "SUCCESS",
    "resultTime": "2025-11-04 16:30:00",
    "data": {
        "summary": {
            "totalProductAmount": 98000,
            "deliveryFee": 0,
            "finalPaymentAmount": 98000
        },
        "items": [
            {
                "cartItemId": 101,
                "productId": 1,
                "name": "하이드레이팅 세럼",
                "brand": "글로우랩",
                "price": 28000,
                "quantity": 1,
                "imageUrl": "https://.../img.jpg",
                "selected": true,
                "skinAlert": { "type": "recommend", "message": "..." }
            },
            {
                "cartItemId": 102,
                "productId": 2,
                "name": "비타민 C 토너",
                "brand": "글로우랩",
                "price": 28000,
                "quantity": 2,
                "imageUrl": "https://.../img2.jpg",
                "selected": false,
                "skinAlert": null
            }
        ]
    }
}
```

| 필드     | 타입   | 최대 길이 | 설명         |
| -------- | ------ | --------- | ------------ |
| `필드명` | String | 100       | 회원 고유 ID |
|          |        |           |              |

---

### [DELETE] `/api/cart/items` : 상품 삭제

> 특정 상품 `Request Body`로 전달 받은 `cartItemIds` 목록에 해당하는 상품들을 장바구니에서 삭제합니다.

### 🛎️ Request Body

```json
{
    "cartItemIds": [101, 102]
}
```

| 필드          | 타입          | 최대 길이 | 필수 / 선택 | 설명                                    |
| ------------- | ------------- | --------- | ----------- | --------------------------------------- |
| `cartItemIds` | `Array<Long>` | 100       | 필수        | 삭제할 장바구니 항목 ID 목록 (`cartNo`) |

### 💬 Response Example

```json
{
    "resultCode": 200,
    "resultMsg": "SUCCESS",
    "resultTime": "2025-11-04 16:30:00",
    "data": {
        "summary": {
            "totalProductAmount": 98000,
            "deliveryFee": 0,
            "finalPaymentAmount": 98000
        },
        "items": [
            {
                "cartItemId": 101,
                "productId": 1,
                "name": "하이드레이팅 세럼",
                "brand": "글로우랩",
                "price": 28000,
                "quantity": 1,
                "imageUrl": "https://.../img.jpg",
                "selected": true,
                "skinAlert": { "type": "recommend", "message": "..." }
            },
            {
                "cartItemId": 102,
                "productId": 2,
                "name": "비타민 C 토너",
                "brand": "글로우랩",
                "price": 28000,
                "quantity": 2,
                "imageUrl": "https://.../img2.jpg",
                "selected": false,
                "skinAlert": null
            }
        ]
    }
}
```

| 필드     | 타입   | 최대 길이 | 설명         |
| -------- | ------ | --------- | ------------ |
| `필드명` | String | 100       | 회원 고유 ID |
|          |        |           |              |

---

## 3.4. 관심 상품

### [GET] `/api/dibs` : 관심 상품 목록 조회

> 현재 유저가 관심 등록한 모든 상품 목록을 조회합니다.

### 🛎️ Request Body

```json

```

필요한 설명 있으면 여기에 텍스트 추가하고 아니면 표만 작성해주세요

| 필드     | 타입   | 최대 길이 | 필수 / 선택 | 설명 |
| -------- | ------ | --------- | ----------- | ---- |
| `필드명` | String | 100       | 필수        | 설명 |
|          |        |           |             |      |

### 💬 Response Example

```json
{
    "resultCode": 200,
    "resultMsg": "SUCCESS",
    "resultTime": "2025-11-05 15:10:00",
    "data": {
        "items": [
            {
                "dibNo": 1,
                "productId": 101,
                "name": "하이드레이팅 세럼",
                "brand": "글로우랩",
                "price": 28000,
                "imageUrl": "https://.../img.jpg",
                "prdStock": 50
            },
            {
                "dibNo": 2,
                "productId": 102,
                "name": "비타민 C 토너",
                "brand": "퓨어스킨",
                "price": 35000,
                "imageUrl": "https://.../img2.jpg",
                "prdStock": 0
            }
        ]
    }
}
```

상품 없으면 items는 `[]`

| 필드     | 타입   | 최대 길이 | 설명         |
| -------- | ------ | --------- | ------------ |
| `필드명` | String | 100       | 회원 고유 ID |
|          |        |           |              |

---

### [Post] `/api/dibs` : 관심 등록

> 특정 상품을 관심 목록에 추가합니다.

### 🛎️ Request Body

```json
{
    "productId": 3
}
```

필요한 설명 있으면 여기에 텍스트 추가하고 아니면 표만 작성해주세요

| 필드        | 타입     | 최대 길이 | 필수 / 선택 | 설명                       |
| ----------- | -------- | --------- | ----------- | -------------------------- |
| `productId` | `Number` |           | 필수        | `PRODUCT` 테이블의 `prdNo` |
|             |          |           |             |                            |

### 💬 Response Example

```json
{
    "resultCode": 200,
    "resultMsg": "SUCCESS",
    "resultTime": "2025-11-05 15:10:00",
    "data": {
        "items": [
            {
                "dibNo": 1,
                "productId": 101,
                "name": "하이드레이팅 세럼",
                "brand": "글로우랩",
                "price": 28000,
                "imageUrl": "https://.../img.jpg",
                "prdStock": 50
            },
            {
                "dibNo": 2,
                "productId": 102,
                "name": "비타민 C 토너",
                "brand": "퓨어스킨",
                "price": 35000,
                "imageUrl": "https://.../img2.jpg",
                "prdStock": 0
            }
        ]
    }
}
```

상품 없으면 items는 `[]`

| 필드     | 타입   | 최대 길이 | 설명         |
| -------- | ------ | --------- | ------------ |
| `필드명` | String | 100       | 회원 고유 ID |
|          |        |           |              |

---

### [DELETE] `/api/dibs/{productId}` : 관심 해제

> 특정 상품을 관심 목록에서 삭제합니다.
> `{productId}` : `PRODUCT` 테이블의 `prdNo` (상품 번호)

### 🛎️ Request Body

```json

```

필요한 설명 있으면 여기에 텍스트 추가하고 아니면 표만 작성해주세요

| 필드     | 타입   | 최대 길이 | 필수 / 선택 | 설명 |
| -------- | ------ | --------- | ----------- | ---- |
| `필드명` | String | 100       | 필수        | 설명 |
|          |        |           |             |      |

### 💬 Response Example

```json

```

필요한 설명 있으면 여기에 텍스트 추가하고 아니면 표만 작성해주세요

| 필드     | 타입   | 최대 길이 | 설명 |
| -------- | ------ | --------- | ---- |
| `필드명` | String | 100       | 설명 |
|          |        |           |      |

---

## 3.5. 성분

### [GET] `/api/products/{prdNo}/ingredients` : 성분 목록 조회

> 현재 상품이 가지고 있는 성분에 대한 목록을 조회합니다.
> `{prdNo}` : `PRODUCT` 테이블의 `prdNo` (상품 번호)

### 🛎️ Request Body

```json

```

필요한 설명 있으면 여기에 텍스트 추가하고 아니면 표만 작성해주세요

| 필드     | 타입   | 최대 길이 | 필수 / 선택 | 설명 |
| -------- | ------ | --------- | ----------- | ---- |
| `필드명` | String | 100       | 필수        | 설명 |
|          |        |           |             |      |

### 💬 Response Example

```json
{
  "resultCode": 200,
  "resultMsg": "SUCCESS",
  "resultTime": "2025-11-07 10:30:00",
  "data": {
   "totalCount": 25,
    "ingredients": [
      {
        "ingNo": 101,
        "ingName": "정제수",
        "ingEngName": "Water",
        "ingAllergen": 0, // 알레르기 유발 (0: false)
        "ingDanger": 0,   // 20가지 주의 (0: false)
        "ingFunctional": "여드름"
      },
      {
        "ingNo": 102,
        "ingName": "부틸렌글라이콜",
        "ingEngName": "Butyl...",
        "ingAllergen": 1, // 알레르기 유발 (1: true)
        "ingDanger": 0,
        "ingFunctional": "각질 제거"
      },
      { ... }
    ]
  }
}
```

총점, 리뷰들, 몇개 보여줄지 결정할 필요 있음…

| 필드     | 타입   | 최대 길이 | 설명 |
| -------- | ------ | --------- | ---- |
| `필드명` | String | 100       | 설명 |
|          |        |           |      |

---

### [GET] `/api/ingredients/{ingNo}` : 성분 상세 조회

> 현재 상품이 가지고 있는 성분에 대한 목록을 조회합니다.
> `{ingNo}` : `INGREDIENTS`테이블의 `ingNo` (상품 번호)

### 🛎️ Request Body

```json

```

필요한 설명 있으면 여기에 텍스트 추가하고 아니면 표만 작성해주세요

| 필드     | 타입   | 최대 길이 | 필수 / 선택 | 설명 |
| -------- | ------ | --------- | ----------- | ---- |
| `필드명` | String | 100       | 필수        | 설명 |
|          |        |           |             |      |

### 💬 Response Example

```json
{
    "resultCode": 200,
    "resultMsg": "SUCCESS",
    "resultTime": "2025-11-07 10:30:00",
    "data": {
        "ingNo": 102,
        "ingName": "부틸렌글라이콜",
        "ingEngName": "Butyl...",
        "ingDesc": "피부에 수분을...", //설명
        "ingAllergen": 1,
        "ingDanger": 0,
        "ingFunctional": "보습", //기능성(식약처 인증)
        "ingGrpName": "~~~계열",

        "effects": {
            "positive": [
                {
                    "effName": "피부 보습",
                    "effAim": "수분 공급",
                    "effSkin": 2
                },
                {
                    "effName": "수분 증발 차단",
                    "effAim": "피부 보호",
                    "effSkin": 2
                }
            ],
            "negative": [
                {
                    "effName": "피부 자극",
                    "effAim": "민감성 주의",
                    "effSkin": 3
                }
            ]
        }
    }
}
```

총점, 리뷰들, 몇개 보여줄지…

| 필드     | 타입   | 최대 길이 | 설명 |
| -------- | ------ | --------- | ---- |
| `필드명` | String | 100       | 설명 |
|          |        |           |      |

---

### [GET] `/api/ingredient/effect/prd_skin?` : 제품에 포함된 피부타입 추천/주의 성분 및 효과 조회

> 제품 번호와 사용자의 피부타입 값을 이용해 제품에 들어있는 성분 중 사용자의 피부타입에 추천/주의 효과가 있는 성분의 명칭과 효과를 목록으로 제공

### 🛎️ Request Body

| 필드       | 타입   | 최대 길이 | 필수 / 선택 | 설명            |
| ---------- | ------ | --------- | ----------- | --------------- |
| `prdNo`    | String | 7         | 필수        | 제품 번호       |
| `userSkin` | String | 2         | 필수        | 사용자 피부타입 |

### 💬 Response Example

```json
{
  "resultCode": 200,
  "resultMsg": "SUCCESS",
  "data": {
	  "good": [
      {
        "EFFNO": 22,
        "EFFNAME": "수렴 및 진정",
        "EFFAIM": "피부컨디셔닝제, 피부보호제",
        "EFFSTATUS": 1,
        "EFFSKIN": 3,
        "EFFREGDATE": "2025-11-21T04:31:49.000+00:00"
      }, ...
    ],
    "bad": [
      {
        "EFFNO": 26,
        "EFFNAME": "고농도일경우 피부자극",
        "EFFAIM": "pH 조정제, 금속이온봉쇄제/킬레이트화제, 착향제",
        "EFFSTATUS": 2,
        "EFFSKIN": 3,
        "EFFREGDATE": "2025-11-21T04:53:45.000+00:00"
      }, ...
    ]
  },
  "resultTime": "2025-11-21 14:19:23"
}
```

필요한 설명 있으면 여기에 텍스트 추가하고 아니면 표만 작성해주세요

| 필드      | 타입   | 최대 길이 | 설명                                     |
| --------- | ------ | --------- | ---------------------------------------- |
| `good`    | list   | -         | 사용자의 피부타입에 추천하는 성분의 목록 |
| `bad`     | list   | -         | 사용자의 피부타입에 주의하는 성분의 목록 |
|           |        |           |                                          |
| `EFFNAME` | String |           |                                          |
|           |        |           |                                          |
|           |        |           |                                          |
|           |        |           |                                          |
|           |        |           |                                          |
|           |        |           |                                          |

---

## 3.6. 리뷰

### [GET] `/api/products/{prdNo}/reviews` : 리뷰 목록 조회

> 현재 상품에 달린 리뷰 목록을 조회합니다.
> `{prdNo}` : `PRODUCT` 테이블의 `prdNo` (상품 번호)

### 🛎️ Request Body

```json

```

필요한 설명 있으면 여기에 텍스트 추가하고 아니면 표만 작성해주세요

| 필드    | 타입   | 최대 길이 | 필수 / 선택 | 설명                                |
| ------- | ------ | --------- | ----------- | ----------------------------------- |
| `page`  | Int    |           | 선택        | 조회할 페이지의 번호                |
| `limit` | Int    |           | 선택        | 한 페이지에서 보여줄 리뷰 수        |
| `sort`  | String |           | 선택        | 정렬 기준(lastest, like, rating 등) |

### 💬 Response Example

```json
{
  "resultCode": 200,
  "resultMsg": "SUCCESS",
  "resultTime": "2025-11-06 14:30:00",
  "data": {
    "summary": {
      "totalCount": 125,
      "averageRating": 4.5
    },
    "reviews": [
      {
        "revNo": 101,
        "userName": "beaut***",
        "revRank": "A",
        "revStar": 5,
        "revGood": "수분보습 완전 최고. 인생템입니다.",
        "revBad": "용량좀 늘려주세요",
        "revImg": "https://.../img.jpg",
        "revDate": "2025-11-05",
        "likeCount": 15
      },
      { ... }
    ],
    "pagination": {
      "page": 1,
      "limit": 10,
      "totalCount": 125,
      "totalPages": 13
    }
  }
}
```

필요한 설명 있으면 여기에 텍스트 추가하고 아니면 표만 작성해주세요

| 필드     | 타입   | 최대 길이 | 설명 |
| -------- | ------ | --------- | ---- |
| `필드명` | String | 100       | 설명 |
|          |        |           |      |

---

### [Post] `/api/products/{prdNo}/reviews` : 리뷰 작성

> 특정 제품에 대한 리뷰를 새로 작성합니다.
> `{pathVariable}` : url에 넣으면 무조건 필수고 자료형은 상관없으므로 무슨값인지만 간단히 설명

### 🛎️ Request Body

```json
{
    "userNo": 789,
    "prdNo": 123,
    "revStar": 5,
    "revGood": "수분보습 완전 최고. 인생템입니다.",
    "revBad": "용량좀 늘려주세요",
    "revImg": "https://.../img.jpg"
}
```

필요한 설명 있으면 여기에 텍스트 추가하고 아니면 표만 작성해주세요

| 필드      | 타입   | 최대 길이 | 필수 / 선택 | 설명             |
| --------- | ------ | --------- | ----------- | ---------------- |
| `userNo`  | int    | 7         | 필수        | 사용자 번호      |
| `prdNo`   | int    | 7         | 필수        | 제품 번호        |
| `revStar` | int    | 2         | 필수        | 제품에 대한 별점 |
| `revText` | String | 미정      | 필수        | 리뷰 텍스트      |
| `revImg`  | String | 255       | 필수        | 리뷰 이미지      |

### 💬 Response Example

```json
{
    "resultCode": 200,
    "resultMsg": "SUCCESS",
    "resultTime": "2025-11-06 14:30:00",
    "data": {
        "revNo": 101,
        "userName": "beaut***",
        "revRank": "A",
        "revStar": 5,
        "revGood": "수분보습 완전 최고. 인생템입니다.",
        "revBad": "용량좀 늘려주세요",
        "revImg": "https://.../img.jpg",
        "revDate": "2025-11-05",
        "likeCount": 0
    }
}
```

필요한 설명 있으면 여기에 텍스트 추가하고 아니면 표만 작성해주세요

| 필드     | 타입   | 최대 길이 | 설명 |
| -------- | ------ | --------- | ---- |
| `필드명` | String | 100       | 설명 |
|          |        |           |      |

---

### [PUT] `/api/reviews/{revNo}` : 리뷰 수정

> revNo에 해당하는 리뷰를 수정합니다.
> `{revNo}` : `Review` 테이블의 `revNo` (리뷰 번호)

### 🛎️ Request Body

```json
{
    "revStar": 5,
    "revGood": "평생 써도 만족.",
    "revBad": "용량 제발 늘려주세",
    "revImg": "https://.../img.jpg"
}
```

필요한 설명 있으면 여기에 텍스트 추가하고 아니면 표만 작성해주세요

| 필드      | 타입   | 최대 길이 | 필수 / 선택 | 설명             |
| --------- | ------ | --------- | ----------- | ---------------- |
| `revStar` | int    | 2         | 필수        | 제품에 대한 별점 |
| `revText` | String | 미정      | 필수        | 리뷰 텍스트      |
| `revImg`  | String | 255       | 필수        | 리뷰 이미지      |

### 💬 Response Example

```json

```

필요한 설명 있으면 여기에 텍스트 추가하고 아니면 표만 작성해주세요

| 필드     | 타입   | 최대 길이 | 설명 |
| -------- | ------ | --------- | ---- |
| `필드명` | String | 100       | 설명 |
|          |        |           |      |

---

### [DELETE] `/api/reviews/{revNo}` : 리뷰 삭제

> revNo에 해당하는 리뷰를 삭제합니다.
> `{revNo}` : Review 테이블의 revNo (리뷰 번호)

### 🛎️ Request Body

```json
{
    "resultCode": 200,
    "resultMsg": "SUCCESS",
    "resultTime": "2025-11-06 14:30:00"
}
```

필요한 설명 있으면 여기에 텍스트 추가하고 아니면 표만 작성해주세요

| 필드     | 타입   | 최대 길이 | 필수 / 선택 | 설명 |
| -------- | ------ | --------- | ----------- | ---- |
| `필드명` | String | 100       | 필수        | 설명 |
|          |        |           |             |      |

### 💬 Response Example

```json

```

필요한 설명 있으면 여기에 텍스트 추가하고 아니면 표만 작성해주세요

| 필드     | 타입   | 최대 길이 | 설명 |
| -------- | ------ | --------- | ---- |
| `필드명` | String | 100       | 설명 |
|          |        |           |      |

---

### [POST] `/api/reviews/{revNo}/like` : 리뷰 ‘좋아요’

> revNo에 해당하는 리뷰에 좋아요를 누릅니다.
> (REVIEW_UPDOWN 테이블에 추가)(재요청시 좋아요 취소-DELETE)
> `{revNo}` : `Review` 테이블의 `revNo` (리뷰 번호)

### 🛎️ Request Body

```json

```

필요한 설명 있으면 여기에 텍스트 추가하고 아니면 표만 작성해주세요

| 필드     | 타입   | 최대 길이 | 필수 / 선택 | 설명 |
| -------- | ------ | --------- | ----------- | ---- |
| `필드명` | String | 100       | 필수        | 설명 |
|          |        |           |             |      |

### 💬 Response Example

```json
{
    "resultCode": 200,
    "resultMsg": "SUCCESS",
    "data": {
        "revNo": 101,
        "likeCount": 16
    }
}
```

필요한 설명 있으면 여기에 텍스트 추가하고 아니면 표만 작성해주세요

| 필드     | 타입   | 최대 길이 | 설명 |
| -------- | ------ | --------- | ---- |
| `필드명` | String | 100       | 설명 |
|          |        |           |      |

---

### [POST] `/api/reviews/{revNo}/report` : 리뷰 신고

> revNo에 해당하는 리뷰를 신고합니다.
> `{revNo}` : `Review` 테이블의 `revNo` (리뷰 번호)

### 🛎️ Request Body

```json
{
    "reportReason": "스팸/광고성 댓글입니다.(예시임)"
}
```

필요한 설명 있으면 여기에 텍스트 추가하고 아니면 표만 작성해주세요

| 필드     | 타입   | 최대 길이 | 필수 / 선택 | 설명 |
| -------- | ------ | --------- | ----------- | ---- |
| `필드명` | String | 100       | 필수        | 설명 |
|          |        |           |             |      |

### 💬 Response Example

```json
{
    "resultCode": 200,
    "resultMsg": "SUCCESS"
}
```

필요한 설명 있으면 여기에 텍스트 추가하고 아니면 표만 작성해주세요

| 필드     | 타입   | 최대 길이 | 설명 |
| -------- | ------ | --------- | ---- |
| `필드명` | String | 100       | 설명 |
|          |        |           |      |

---

# 4. 관리자 API - 8085

---

## 4.1. 회원

## 4.2. 상품

### [GET] `/api/admin/products` : 상품 목록 조회

> 관리자 페이지에서 사용할 상품 목록 전체를 필터링하여 조회합니다.

### 🛎️ Request Body

```json

```

| 필드     | 타입   | 최대 길이 | 필수 / 선택 | 설명 |
| -------- | ------ | --------- | ----------- | ---- |
| `필드명` | String | 100       | 필수        | 설명 |
|          |        |           |             |      |

### 💬 Response Example

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

| 필드              | 타입   | 최대 길이 | 설명                       |
| ----------------- | ------ | --------- | -------------------------- |
| `data`            | Object |           | 응답 데이터 객체           |
| `data.products`   | Array  |           | 상품의 모든 정보 포함 목록 |
| `data.pagination` | Object |           | 페이징 정보                |
|                   |        |           |                            |

---

### [POST] `/api/admin/products` : 상품 등록

> 무슨 기능인지 간략한 소개

### 🛎️ Request Body

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

| 필드          | 타입   | 최대 길이 | 필수 / 선택 | 설명             |
| ------------- | ------ | --------- | ----------- | ---------------- |
| `prdName`     | String | 100       | 필수        | 상품명           |
| `prdPrice`    | Int    | 9         | 필수        | 상품가격         |
| `prdVolume`   | Int    | 5         | 필수        | 용량(ml)         |
| `prdCompany`  | String | 100       | 선택        | 제조사           |
| `prdStock`    | Int    | 5         | 필수        | 재고 수량        |
| `prdMainCate` | String | 2         | 필수        | 상품 대분류      |
| `prdSubCate`  | String | 5         | 필수        | 상품 소분류      |
| `prdImg`      | String | 255       | 선택        | 상품 이미지 경로 |
| `prdDesc`     | String | 255       | 선택        | 상품 상세 설명   |

### 💬 Response Example

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

| 필드   | 타입   | 최대 길이 | 설명                                    |
| ------ | ------ | --------- | --------------------------------------- |
| `data` | Object |           | 생성이 완료된 상품 정보(prdno만 반환함) |

---

### [PUT] `/api/admin/products/{prdNo}` : 상품 수정

> `{prdNo}` : 제품 고유 ID

### 🛎️ Request Body

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

| 필드          | 타입   | 최대 길이 | 필수 / 선택 | 설명             |
| ------------- | ------ | --------- | ----------- | ---------------- |
| `prdName`     | String | 100       | 필수        | 상품명           |
| `prdPrice`    | Int    | 9         | 필수        | 상품가격         |
| `prdVolume`   | Int    | 5         | 필수        | 용량(ml)         |
| `prdCompany`  | String | 100       | 선택        | 제조사           |
| `prdStock`    | Int    | 5         | 필수        | 재고 수량        |
| `prdMainCate` | String | 2         | 필수        | 상품 대분류      |
| `prdSubCate`  | String | 5         | 필수        | 상품 소분류      |
| `prdImg`      | String | 255       | 선택        | 상품 이미지 경로 |
| `prdDesc`     | String | 255       | 선택        | 상품 상세 설명   |

### 💬 Response Example

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

| 필드          | 타입   | 최대 길이 | 설명                |
| ------------- | ------ | --------- | ------------------- |
| `data`        | Object |           | 수정 완료 제품 정보 |
| `prdNo`       | Int    |           | 상품 고유 id        |
| `prdName`     | String |           | 상품명              |
| `prdPrice`    | Int    |           | 상품가격            |
| `prdVolume`   | Int    |           | 용량(ml)            |
| `prdCompany`  | String |           | 제조사              |
| `prdStock`    | Int    |           | 재고 수량           |
| `prdMainCate` | String |           | 상품 대분류         |
| `prdSubCate`  | String |           | 상품 소분류         |
| `prdImg`      | String |           | 상품 이미지 경로    |
| `prdDesc`     | String |           | 상품 상세 설명      |
| `prdUpdate`   | String |           | 최종 수정일         |
| `prdRegdate`  | String |           | 최초 등록일         |
|               |        |           |                     |

---

### [DELETE] `/api/admin/products/{prdNo}` : 상품 삭제

> `{prdNo}` : 제품 고유 ID

### 🛎️ Request Body

```json
{
    "resultCode": 200,
    "resultMsg": "SUCCESS",
    "resultTime": "2025-10-02 11:35:00"
}
```

| 필드     | 타입   | 최대 길이 | 필수 / 선택 | 설명 |
| -------- | ------ | --------- | ----------- | ---- |
| `필드명` | String | 100       | 필수        | 설명 |
|          |        |           |             |      |

### 💬 Response Example

```json

```

| 필드     | 타입   | 최대 길이 | 설명         |
| -------- | ------ | --------- | ------------ |
| `필드명` | String | 100       | 회원 고유 ID |
|          |        |           |              |

---

## 4.7. 주문

### [GET] `/api/orders/list?` : 주문 목록 조회

> 주문정보와 해당하는 고객의 정보를 함께 불러오는 기능

### 🛎️ Request Body

| 필드           | 타입   | 최대 길이 | 필수 / 선택 | 설명                                   |
| -------------- | ------ | --------- | ----------- | -------------------------------------- |
| `page`         | String | -         | 선택        | 기본값 1. 목록의 페이징 번호.          |
| `page_gap`     | String | -         | 선택        | 기본값 10. 한 페이지에 표시할 목록 수. |
| `mem_name`     | String | -         | 선택        | 결제 고객명.                           |
| `od_start_day` | String | -         | 선택        | 주문 접수날짜 조회 시작일              |
| `od_end_day`   | String | -         | 선택        | 주문 접수날짜 조회 종료일              |

### 💬 Response Example

```json
{
  "resultCode": 200,
  "resultMsg": "SUCCESS",
  "data": {
    "total": 5,
    "list": [
      {
        "ODNO": 205,
		    "ODREGDATE": "2025-11-04T15:00:00.000+00:00",
		    "ODPRDPRICE": 35000,
		    "ODDELVPRICE": 2500,
		    "ODNAME": "최수연",
		    "ODHP": "010-5678-9012",
		    "ODBCCODE": 555555555555,
		    "ODZIP": 56789,
		    "ODJIBUNADDR": "서울 영등포구 여의도동 5-5",
		    "ODROADADDR": "서울특별시 영등포구 국제금융로 5",
		    "ODDETAILADDR": "505동 505호",
		    "ODDELVKEYTYPE": 1,
		    "ODDELVKEY": "00",
		    "ODDELVMSG": "초인종 누르고 알려주세요",
		    "USERNO": 5,
		    "USERNAME": "문유빈",
		    "USERID": "user005",
		    "USERNICK": "유빈_뷰티",
		    "USERHP": "010-8124-2635"
      }, ...
    ]
  },
  "resultTime": "2025-11-17 13:55:01"
}
```

| 필드    | 타입  | 최대 길이 | 설명                                            |
| ------- | ----- | --------- | ----------------------------------------------- |
| `total` | Int   | 100       | 검색 조건에 맞는 레코드 총 갯수. 페이징에 사용. |
| `list`  | Array | -         | 검색 조건에 페이징 범위를 적용한 결과 데이터.   |

---

### [GET] `/api/orders/{odNo}` : 주문 번호 조회

> 주문번호에 해당하는 주문정보와 해당하는 고객의 정보를 함께 불러오는 기능
> `{odNo}` : 주문번호

### 🛎️ Request Body

PathVariable만 있으면 조회 가능

### 💬 Response Example

```json
{
    "resultCode": 200,
    "resultMsg": "SUCCESS",
    "data": {
        "ODNO": 205,
        "ODREGDATE": "2025-11-04T15:00:00.000+00:00",
        "ODPRDPRICE": 35000,
        "ODDELVPRICE": 2500,
        "ODNAME": "최수연",
        "ODHP": "010-5678-9012",
        "ODBCCODE": 555555555555,
        "ODZIP": 56789,
        "ODJIBUNADDR": "서울 영등포구 여의도동 5-5",
        "ODROADADDR": "서울특별시 영등포구 국제금융로 5",
        "ODDETAILADDR": "505동 505호",
        "ODDELVKEYTYPE": 1,
        "ODDELVKEY": "00",
        "ODDELVMSG": "초인종 누르고 알려주세요",
        "USERNO": 5,
        "USERNAME": "문유빈",
        "USERID": "user005",
        "USERNICK": "유빈_뷰티",
        "USERHP": "010-8124-2635"
    },
    "resultTime": "2025-11-17 15:17:06"
}
```

| 필드            | 타입   | 최대 길이 | 설명                                                        |
| --------------- | ------ | --------- | ----------------------------------------------------------- |
| `ODNO`          | Int    | 7         | 주문번호                                                    |
| `ODREGDATE`     | DATE   | -         | 주문일                                                      |
| `ODPRDPRICE`    | Int    | 9         | 상품 가격 합계                                              |
| `ODDELVPRICE`   | Int    | 9         | 택배비                                                      |
| `ODNAME`        | String | 18        | 수취인 성명                                                 |
| `ODHP`          | String | 13        | 수취인 연락처                                               |
| `ODBCCODE`      | Int    | 12        | 수취인 주소 시군구코드                                      |
| `ODZIP`         | Int    | 5         | 수취인 주소 우편번호                                        |
| `ODJIBUNADDR`   | String | 255       | 수취인 주소 지번주소                                        |
| `ODROADADDR`    | String | 255       | 수취인 주소 도로명주소                                      |
| `ODDETAILADDR`  | String | 255       | 수취인 주소 상세주소                                        |
| `ODDELVKEYTYPE` | Int    | 1         | 택배기사 출입방법(1:공동현관번호/2:자유출입가능)            |
| `ODDELVKEY`     | String | 100       | 택배기사 출입번호(출입방법1에 출입번호null이면 [없음] 표기) |
| `ODDELVMSG`     | String | 255       | 택배기사 요청사항                                           |
| `USERNO`        | Int    | 7         | 결제고객 번호                                               |
| `USERNAME`      | String | 18        | 결제고객 성명                                               |
| `USERID`        | String | 255       | 결제고객 ID                                                 |
| `USERNICK`      | String | 20        | 결제고객 닉네임                                             |
| `USERHP`        | String | 13        | 결제고객 연락처                                             |

---

### [GET] `/api/order_delivery/list?` : 택배 목록 조회

> 택배정보와 해당하는 고객의 정보를 함께 불러오는 기능

### 🛎️ Request Body

| 필드               | 타입   | 최대 길이 | 필수 / 선택 | 설명                                   |
| ------------------ | ------ | --------- | ----------- | -------------------------------------- |
| `page`             | String | -         | 선택        | 기본값 1. 목록의 페이징 번호.          |
| `page_gap`         | String | -         | 선택        | 기본값 10. 한 페이지에 표시할 목록 수. |
| `mem_name`         | String | -         | 선택        | 결제 고객명.                           |
| `delv_s_start_day` | String | -         | 선택        | 택배접수일 조회 시작일(이상)           |
| `delv_s_end_day`   | String | -         | 선택        | 택배접수일 조회 종료일(이하)           |
| `delv_e_start_day` | String | -         | 선택        | 택배종료일 조회 시작일(이상)           |
| `delv_e_end_day`   | String | -         | 선택        | 택배종료일 조회 종료일(이하)           |

### 💬 Response Example

```json
{
  "resultCode": 200,
  "resultMsg": "SUCCESS",
  "data": {
    "total": 5,
    "list": [
      {
        "DELVNO": 3,
		    "ODNO": 203,
		    "DELVREGDATE": "2025-11-03 00:00:00",
		    "DELVENDDATE": "2025-11-07 00:00:00",
		    "DELVCOMPANY": "한진택배",
		    "DELVCOMNUM": "TNUM0003",
		    "DELVTYPE": 1,
		    "DELVSTATUS": 3,
		    "DELVGETNAME": "박영희",
        "DELVGETHP": "010-5036-5580",
		    "DELVGETZIP": 34567,
		    "DELVGETJIBUNADDR": "서울 송파구 잠실동 3-3",
		    "DELVGETROADADDR": "서울특별시 송파구 올림픽로 3",
		    "DELVGETDETAILADDR": "303동 303호",
		    "DELVGETBCCODE": 333333333333,
		    "USERNO": 3,
		    "USERNAME": "이민아",
		    "USERID": "user003",
		    "USERNICK": "mina.gl",
		    "USERHP": "010-5036-5580"
      }, ...
    ]
  },
  "resultTime": "2025-11-19 16:19:18"
}
```

| 필드    | 타입  | 최대 길이 | 설명                                            |
| ------- | ----- | --------- | ----------------------------------------------- |
| `total` | Int   | 100       | 검색 조건에 맞는 레코드 총 갯수. 페이징에 사용. |
| `list`  | Array | -         | 검색 조건에 페이징 범위를 적용한 결과 데이터.   |

---

### [GET] `/api/order_delivery/{delvNo}` : 택배 번호 조회

> 택배번호에 해당하는 택배정보와 해당하는 고객의 정보를 함께 불러오는 기능
> `{delvNo}` : 택배번호

### 🛎️ Request Body

PathVariable만 있으면 조회 가능

### 💬 Response Example

```json
{
    "resultCode": 200,
    "resultMsg": "SUCCESS",
    "data": {
        "DELVNO": 3,
        "ODNO": 203,
        "DELVREGDATE": "2025-11-03 00:00:00",
        "DELVENDDATE": "2025-11-07 00:00:00",
        "DELVCOMPANY": "한진택배",
        "DELVCOMNUM": "TNUM0003",
        "DELVTYPE": 1,
        "DELVSTATUS": 3,
        "DELVGETNAME": "박영희",
        "DELVGETHP": "010-5036-5580",
        "DELVGETZIP": 34567,
        "DELVGETJIBUNADDR": "서울 송파구 잠실동 3-3",
        "DELVGETROADADDR": "서울특별시 송파구 올림픽로 3",
        "DELVGETDETAILADDR": "303동 303호",
        "DELVGETBCCODE": 333333333333,
        "USERNO": 3,
        "USERNAME": "이민아",
        "USERID": "user003",
        "USERNICK": "mina.gl",
        "USERHP": "010-5036-5580"
    },
    "resultTime": "2025-11-19 16:06:51"
}
```

| 필드                                   | 타입   | 최대 길이 | 설명                                                                          |
| -------------------------------------- | ------ | --------- | ----------------------------------------------------------------------------- |
| `DELVNO`                               | Int    | 7         | 택배 번호                                                                     |
| `ODNO`                                 | Int    | 7         | 주문 번호                                                                     |
| `DELVREGDATE`                          | DATE   | -         | 택배 접수일                                                                   |
| `DELVENDDATE`                          | DATE   | -         | 택배 완료일                                                                   |
| `DELVCOMPANY`                          | String | 50        | 택배 회사명                                                                   |
| `DELVCOMNUM`                           | String | 20        | 택배 송장번호                                                                 |
| `DELVTYPE`                             | Int    | 2         | 11:배송/12:재배송/13:취소                                                     |
| 21:교환회수/22:교환재발송//31:반품회수 |
| `DELVSTATUS`                           | Int    | 2         | 1: 배송준비중, 2: 집화완료, 3: 배송중, 4: 지점 도착, 5: 배송출발, 6:배송 완료 |
| `DELVGETNAME`                          | String | 100       | 수령인 성명                                                                   |
| `DELVGETHP`                            | String | 13        | 수령인 연락처                                                                 |
| `DELVGETZIP`                           | Int    | 5         | 수령지 우편번호                                                               |
| `DELVGETJIBUNADDR`                     | String | 255       | 수령지 지번주소                                                               |
| `DELVGETROADADDR`                      | String | 255       | 수령지 도로명주소                                                             |
| `DELVGETDETAILADDR`                    | String | 255       | 수령지 상세주소                                                               |
| `DELVGETBCCODE`                        | Int    | 10        | 수령지 시군구코드                                                             |
| `USERNO`                               | Int    | 7         | 결제고객 번호                                                                 |
| `USERNAME`                             | String | 18        | 결제고객 성명                                                                 |
| `USERID`                               | String | 255       | 결제고객 ID                                                                   |
| `USERNICK`                             | String | 20        | 결제고객 닉네임                                                               |
| `USERHP`                               | String | 13        | 결제고객 연락처                                                               |

---
