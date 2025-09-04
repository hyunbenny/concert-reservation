# 유저 로그인 토큰 발급 API
- **Method:** POST
- **URL:** `/api/v1/auth/login`
- **설명:** 로그인

## Request
### Body
| 이름       | 타입   | 필수 | 설명   | 기타   |
|----------|--------|------|------|------|
| username | String | Y    | 아이디  |      |
| password | String | Y    | 비밀번호 |      |

## Response
### Body
| 이름           | 타입     | 필수  | 설명              | 기타               |
|--------------|--------|-----|-----------------|------------------|
| errorCode    | String | Y   | 응답코드            |                  |
| message      | String | Y   | 응답메시지           |                  |
| accessToken  | String | N   | 토큰              | Bearer           |
| expiresIn    | int    | N   | 유효시간            | 단위: 초, 기본값: 900초 |
| userId       | String | N   | 아이디             |                  |
| username     | String | N   | 사용자 이름          |                  |
| refreshToken | String | N   | 리프레시 토큰(토큰 갱신용) |                  |

# 유저 로그인 토큰 갱신 API
- **Method:** POST
- **URL:** `/api/v1/auth/refresh`
- **설명:** access token 만료 시, refresh token 갱신

## Request
### Body
| 이름           | 타입   | 필수 | 설명 | 기타   |
|--------------|--------|------|----|------|
| refreshToken | String | Y    |  리프레시 토큰  |      |

## Response
### Body
| 이름           | 타입     | 필수  | 설명              | 기타               |
|--------------|--------|-----|-----------------|------------------|
| errorCode    | String | Y   | 응답코드            |                  |
| message      | String | Y   | 응답메시지           |                  |
| accessToken  | String | N   | 토큰              | Bearer           |
| expiresIn    | int    | N   | 유효시간            | 단위: 초, 기본값: 900초 |
| userId       | String | N   | 사용자 uuid        |                  |
| username     | String | N   | 사용자 이름          |                  |
| refreshToken | String | N   | 리프레시 토큰(토큰 갱신용) | 갱신할 때마다 변경       |


# 대기열 토큰 발급 API
- **Method:** POST
- **URL:** `/api/v1/queue/token`
- **설명:** 대기열에 사용자를 등록, 토큰을 발급

## Request
### Header
| 이름          | 타입   | 필수 | 설명      | 기타                                |
|---------------|--------|------|---------|----|
| Authorization | String | Y    | 엑세스 토큰  | Bearer |

## Response
### Body
| 이름         | 타입     | 필수  | 설명       | 기타                                |
|------------|--------|-----|----------|-----------------------------------|
| errorCode    | String | Y   | 응답코드     |     |
| message      | String | Y   | 응답메시지    |     |
| queueToken | String | N   | 대기열 토큰   |                                   |
| userId     | String | N   | 사용자 uuid |                                   |
| status     | String | N   | 대기 상태    | WAITING, READY, BOOKED, COMPLETED |
| expiresIn  | int    | N   | 유효시간     | 기본 1200초(20분)                     |


# 대기열 조회 API
- **Method:** POST
- **URL:** `/api/v1/queue/status`
- **설명:** 대기열의 상태 조회

## Request
### Header
| 이름          | 타입   | 필수 | 설명      | 기타                                |
|---------------|--------|------|---------|----|
| Authorization | String | Y    | 엑세스 토큰  | Bearer |

### Body
| 이름         | 타입   | 필수  | 설명     | 기타   |
|------------|--------|-----|--------|------|
| queueToken | String | Y   | 대기열 토큰 |      |

## Response
### Body
| 이름         | 타입     | 필수  | 설명       | 기타                                                    |
|------------|--------|-----|----------|-------------------------------------------------------|
| errorCode    | String | Y   | 응답코드     |                                                       |
| message      | String | Y   | 응답메시지    |                                                       |
| userId     | String | N   | 사용자 uuid |                                                       |
| status     | String | N   | 대기 상태    | WAITING(대기), READY(처리중), BOOKED(좌석 예약), COMPLETED(완료) |
| expiresIn  | int    | N   | 유효시간     | 기본 1200초(20분)                                         |


# 공연 조회 API
- **Method:** GET
- **URL:** `/api/v1/shows`
- **설명:** 현재 공연 중인 공연 목록 조회

## Request
### Header
| 이름          | 타입   | 필수 | 설명      | 기타                                |
|---------------|--------|------|---------|----|
| Authorization | String | Y    | 엑세스 토큰  | Bearer |

### Body
| 이름                             | 타입   | 필수  | 설명     | 기타   |
|--------------------------------|--------|-----|--------|------|
| queueToken                     | String | Y   | 대기열 토큰 |      |


## Response
### Body
| 이름                                   | 타입     | 필수  | 설명     | 기타                                             |
|--------------------------------------|--------|-----|--------|------------------------------------------------|
| errorCode                            | String | Y   | 응답코드   |                                                |
| message                              | String | Y   | 응답메시지  |                                                |
| shows                                | Array  | N   | 공연 리스트 |                                                |
| &nbsp;&nbsp;&nbsp;&nbsp; showId      | String | N   | 공연 아이디 |                                                |
| &nbsp;&nbsp;&nbsp;&nbsp; title       | String | N   | 공연 타이틀 |                                                |
| &nbsp;&nbsp;&nbsp;&nbsp; category    | String | N   | 공연 분류  |                                                |
| &nbsp;&nbsp;&nbsp;&nbsp; location    | String | N   | 공연 장소  |                                                |
| &nbsp;&nbsp;&nbsp;&nbsp; runningTime | int    | N   | 공연 시간  | 단위: 분                                          |
| &nbsp;&nbsp;&nbsp;&nbsp; status      | String | N   | 공연 상태  | SCHEDULED, OPEN, SOLD_OUT, COMPLETED, CANCELED |
| &nbsp;&nbsp;&nbsp;&nbsp; startDate   | String | N   | 공연 시작일 | YYYY-MM-DD                                       |
| &nbsp;&nbsp;&nbsp;&nbsp; endDate     | String | N   | 공연 종료일 | YYYY-MM-DD                                       |


# 공연 상세 정보 조회 API
- **Method:** GET
- **URL:** `/api/v1/shows/{showId}`
- **설명:** 선택한 공연의 정보 조회

## Request
### Header
| 이름          | 타입   | 필수 | 설명      | 기타                                |
|---------------|--------|------|---------|----|
| Authorization | String | Y    | 엑세스 토큰  | Bearer |

### Body
| 이름                             | 타입   | 필수  | 설명     | 기타   |
|--------------------------------|--------|-----|--------|------|
| queueToken                     | String | Y   | 대기열 토큰 |      |

## Response
### Body
| 이름                  | 타입     | 필수  | 설명         | 기타                                             |
|---------------------|--------|-----|------------|------------------------------------------------|
| errorCode           | String | Y   | 응답코드       |                                                |
| message             | String | Y   | 응답메시지      |                                                |
| showId              | String | N   | 공연 아이디     |                                                |
| title               | String | N   | 공연 타이틀     |                                                |
| category            | String | N   | 공연 분류      |                                                |
| location            | String | N   | 공연 장소      |                                                |
| price               | int     | N   | 금액         | 좌석 당 금액                                        |
| runningTime         | int    | N   | 공연 시간      | 단위: 분                                          |
| cast                | String | N   | 출연진        |                                                |
| posterUrl           | String | N   | 공연 포스터 url |                                                |
| status              | String | N   | 공연 상태      | SCHEDULED, OPEN, SOLD_OUT, COMPLETED, CANCELED |
| availableSeatsCount | String | N   | 남은 좌석 수    |                                                |
| startDate           | String | N   | 공연 시작일     | YYYY-MM-DD                                     |
| endDate             | String | N   | 공연 종료일     | YYYY-MM-DD                                     |


# 예약 가능 날짜 조회 API
- **Method:** GET
- **URL:** `/api/v1/shows/{showId}/dates`
- **설명:** 선택한 공연의 날짜 조회

## Request
### Header
| 이름          | 타입   | 필수 | 설명      | 기타                                |
|---------------|--------|------|---------|----|
| Authorization | String | Y    | 엑세스 토큰  | Bearer |

### Body
| 이름                             | 타입   | 필수  | 설명     | 기타   |
|--------------------------------|--------|-----|--------|------|
| queueToken                     | String | Y   | 대기열 토큰 |      |

## Response
### Body
| 이름             | 타입     | 필수  | 설명         | 기타       |
|----------------|--------|-----|------------|----------|
| errorCode      | String | Y   | 응답코드       |          |
| message        | String | Y   | 응답메시지      |          |
| showId         | String | N   | 공연 아이디     |          |
| startDate      | String   | N   | 공연 시작일     | YYYY-MM-DD |
| endDate        | String   | N   | 공연 종료일     | YYYY-MM-DD         |


# 예약 가능 좌석 조회 API
- **Method:** GET
- **URL:** `/api/v1/shows/{showId}/dates/{date}/seats`
- **설명:** 선택한 날짜의 공연의 좌석 조회

## Request
### Header
| 이름          | 타입   | 필수 | 설명          |
|---------------|--------|------|---------------|
| Authorization | String | Y    | Bearer 토큰   |

### Body
| 이름                             | 타입   | 필수  | 설명     | 기타   |
|--------------------------------|--------|-----|--------|------|
| queueToken                     | String | N   | 대기열 토큰 |      |

## Response
### Body
| 이름             | 타입            | 필수  | 설명              | 기타                                             |
|----------------|---------------|-----|-----------------|------------------------------------------------|
| errorCode      | String        | Y   | 응답코드            |                                                |
| message        | String        | Y   | 응답메시지           |                                                |
| showId         | String        | N   | 공연 아이디          |                                                |
| availableSeats | Array<String> | N   | 에약 가능한 공연 좌석 정보 |                                                |


# 좌석 예약 요청 API
- **Method:** POST
- **URL:** `/api/v1/reservation`
- **설명:** 선택한 날짜의 공연과 좌석을 예약

## Request 
### Header
| 이름          | 타입   | 필수 | 설명          |
|---------------|--------|------|---------------|
| Authorization | String | Y    | Bearer 토큰   |

### Body
| 이름         | 타입            | 필수  | 설명         | 기타   |
|------------|---------------|-----|------------|------|
| queueToken | String        | Y   | 대기열 토큰     |      |
| showId     | String        | Y   | 예약할 공연 아이디 |      |
| showDate   | String        | Y   | 예약할 공연 시간  |      |
| seats      | Array<String> | Y   | 예약할 좌석     |      |
| totalPrice | int           | Y   | 총 금액       |      |

## Response
### Body
| 이름            | 타입            | 필수  | 설명     | 기타                                           |
|---------------|---------------|-----|--------|----------------------------------------------|
| errorCode     | String        | Y   | 응답코드   |                                              |
| message       | String        | Y   | 응답메시지  |                                              |
| reservationId | String        | N   | 예약 아이디 |                                              |
| status        | String        | N   | 예약 상태  | BOOKED(좌석 예약), COMPLETED(결제완료), CANCELED(취소) |

# 잔액 충전 API
- **Method:** POST
- **URL:** `/api/v1/points`
- **설명:** 결제에 사용될 금액을 충전

## Request
### Header
| 이름          | 타입   | 필수 | 설명          |
|---------------|--------|------|---------------|
| Authorization | String | Y    | Bearer 토큰   |

### Body
| 이름           | 타입   | 필수  | 설명    |
|--------------|------|-----|-------|
| chargeAmount | int  | Y   | 충전 금액 |    |

# Response
### Body
| 이름           | 타입     | 필수  | 설명       | 기타                                           |
|--------------|--------|-----|----------|----------------------------------------------|
| errorCode    | String | Y   | 응답코드     |                                              |
| message      | String | Y   | 응답메시지    |                                              |
| userId       | String | N   | 사용자 uuid |                                              |
| chargeAmount | int    | N   | 충전 금액    |                                              |
| balance      | int    | N   | 충전 후 금액  |  |


# 잔액 조회 API
- **Method:** POST
- **URL:** `/api/v1/points`
- **설명:** 사용 후 남은 잔액을 조회

## Request
### Header
| 이름          | 타입   | 필수 | 설명          |
|---------------|--------|------|---------------|
| Authorization | String | Y    | Bearer 토큰   |

## Response
### Body
| 이름        | 타입     | 필수  | 설명       | 기타                                           |
|-----------|--------|-----|----------|----------------------------------------------|
| errorCode | String | Y   | 응답코드     |                                              |
| message   | String | Y   | 응답메시지    |                                              |
| userId    | String | N   | 사용자 uuid |                                              |
| balance   | int    | N   | 잔액  |  |

# 사용 내역 조회 API
- **Method:** GET
- **URL:** `/api/v1/points/histories`
- **설명:** 충전/사용 내역을 조회

## Request
### Header
| 이름          | 타입   | 필수 | 설명          |
|---------------|--------|------|---------------|
| Authorization | String | Y    | Bearer 토큰   |

## Response
### Body
| 이름                               | 타입       | 필수  | 설명             | 기타                    |
|----------------------------------|----------|-----|----------------|-----------------------|
| errorCode                        | String   | Y   | 응답코드           |                       |
| message                          | String   | Y   | 응답메시지          |                       |
| userId    | String | N   | 사용자 uuid |                                              |
| histories                        | Array    | N   | 사용 내역 리스트      |                       |
| &nbsp;&nbsp;&nbsp;&nbsp; type    | String   | N   | 거래 타입(충전/사용) |                       |
| &nbsp;&nbsp;&nbsp;&nbsp; amount  | int      | N   | 충전/사용 금액       |                       |
| &nbsp;&nbsp;&nbsp;&nbsp; balance | int      | N   | 잔액             |                       |
| &nbsp;&nbsp;&nbsp;&nbsp; date    | String   | N   | 요청일시           | YYYY-MM-DD HH24:MM:SS |


# 결제 API
- **Method:** POST
- **URL:** `/api/v1/payment`
- **설명:** 예약한 공연/좌석을 결재

## Request
### Header
| 이름          | 타입   | 필수 | 설명          |
|---------------|--------|------|---------------|
| Authorization | String | Y    | Bearer 토큰   |

### Body
| 이름            | 타입     | 필수  | 설명        | 기타                    |
|---------------|--------|-----|-----------|-----------------------|
| queueToken    | String | Y   | 대기열 토큰 |      |
| reservationId | String | Y   | 예약 아이디    |                       |

## Response
### Body
| 이름       | 타입       | 필수  | 설명             | 기타                    |
|----------|----------|-----|----------------|-----------------------|
| errorCode | String   | Y   | 응답코드           |                       |
| message  | String   | Y   | 응답메시지          |                       |