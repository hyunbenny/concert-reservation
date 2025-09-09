# 유저 로그인 토큰 발급 API
- **Method:** POST
- **URL:** `/api/v1/auth/login`
- **설명:** 로그인

## Request
### Body
| 이름       | 타입   | 필수 | 설명   | 기타   |
|----------|--------|------|------|------|
| userId   | String | Y    | 아이디  |      |
| password | String | Y    | 비밀번호 |      |

## Response
### Body
| 이름           | 타입     | 필수  | 설명              | 기타               |
|--------------|--------|-----|-----------------|------------------|
| errorCode    | String | Y   | 응답코드            |                  |
| message      | String | Y   | 응답메시지           |                  |
| accessToken  | String | N   | 토큰              | Bearer           |
| expiresIn    | int    | N   | 유효시간            | 단위: 초, 기본값: 900초 |
| userId       | String | N   | 사용자 아이디             |                  |
| username     | String | N   | 사용자 이름          |                  |
| refreshToken | String | N   | 리프레시 토큰(토큰 갱신용) |                  |

# 유저 로그인 토큰 갱신 API
- **Method:** POST
- **URL:** `/api/v1/auth/refresh`
- **설명:** access token 만료 시, refresh token을 사용하여 새로운 토큰 발급

## Request
### Body
| 이름           | 타입   | 필수 | 설명 | 기타   |
|--------------|--------|------|----|------|
| refreshToken | String | Y    |  리프레시 토큰  |      |

## Response
### Body
| 이름           | 타입     | 필수  | 설명             | 기타          |
|--------------|--------|-----|----------------|-------------|
| errorCode    | String | Y   | 응답코드           |             |
| message      | String | Y   | 응답메시지          |             |
| accessToken  | String | N   | 토큰             | Bearer      |
| expiresIn    | int    | N   | 유효시간           | 단위: 초, 기본값: 900초 |
| userId       | String | N   | 사용자 아이디        |             |
| username     | String | N   | 사용자 이름         |             |
| refreshToken | String | N   | 리프레시 토큰(토큰 갱신용) | 갱신할 때마다 변경  |


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
| 이름         | 타입     | 필수  | 설명   | 기타                      |
|------------|--------|-----|------|-------------------------|
| errorCode    | String | Y   | 응답코드 |                         |
| message      | String | Y   | 응답메시지 |                         |
| queueToken | String | N   | 대기열 토큰 |                         |
| userId     | String | N   | 사용자 아이디 |                         |
| status     | String | N   | 대기 상태 | WAITING, READY, EXPIRED |
| expiresIn  | int    | N   | 유효시간 | 기본 1200초(20분)           |


# 대기열 조회 API
- **Method:** GET
- **URL:** `/api/v1/queue/status`
- **설명:** 대기열의 상태 조회

## Request
### Header
| 이름          | 타입   | 필수 | 설명      | 기타                                |
|---------------|--------|------|---------|----|
| Authorization | String | Y    | 엑세스 토큰  | Bearer |
| Queue-Token   | String        | Y   | 대기열 토큰     |      |


## Response
### Body
| 이름         | 타입     | 필수  | 설명      | 기타                                                    |
|------------|--------|-----|---------|-------------------------------------------------------|
| errorCode    | String | Y   | 응답코드    |                                                       |
| message      | String | Y   | 응답메시지   |                                                       |
| userId     | String | N   | 사용자 아이디 |                                                       |
| status     | String | N   | 대기 상태   | WAITING, READY, EXPIRED |
| expiresIn  | int    | N   | 유효시간    | 기본 1200초(20분)                                         |


# 공연 조회 API
- **Method:** GET
- **URL:** `/api/v1/shows`
- **설명:** 현재 공연 중인 공연 목록 조회

## Request
### Header
| 이름          | 타입   | 필수 | 설명      | 기타                                |
|---------------|--------|------|---------|----|
| Authorization | String | Y    | 엑세스 토큰  | Bearer |
| Queue-Token   | String        | Y   | 대기열 토큰     |      |

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
- **설명:** 선택한 공연의 정보 및 예약 가능한 날짜를 조회

## Request
### Header
| 이름          | 타입   | 필수 | 설명      | 기타                                |
|---------------|--------|------|---------|----|
| Authorization | String | Y    | 엑세스 토큰  | Bearer |
| Queue-Token   | String        | Y   | 대기열 토큰     |      |

## Response
### Body
| 이름                                           | 타입       | 필수  | 설명         | 기타                                              |
|----------------------------------------------|----------|-----|------------|-------------------------------------------------|
| errorCode                                    | String   | Y   | 응답코드       |                                                 |
| message                                      | String   | Y   | 응답메시지      |                                                 |
| showId                                       | String   | N   | 공연 아이디     |                                                 |
| title                                        | String   | N   | 공연 타이틀     |                                                 |
| category                                     | String   | N   | 공연 분류      |                                                 |
| location                                     | String   | N   | 공연 장소      |                                                 |
| price                                        | String   | N   | 금액         | 좌석 당 금액                                         |
| runningTime                                  | int      | N   | 공연 시간      | 단위: 분                                           |
| cast                                         | String   | N   | 출연진        |                                                 |
| posterUrl                                    | String   | N   | 공연 포스터 url |                                                 |
| startDate                                    | String   | N   | 공연 시작일     | YYYY-MM-DD                                      |
| endDate                                      | String   | N   | 공연 종료일     | YYYY-MM-DD                                      |
| schedules                                    | Array    | N   | 공연 회차 정보   |                                                 |
| &nbsp;&nbsp;&nbsp;&nbsp; scheduleId          | String   | N   | 공연 회차 아이디  |                                                 |
| &nbsp;&nbsp;&nbsp;&nbsp; availableSeatsCount | int      | N   | 남은 좌석 수    |                                                 |
| &nbsp;&nbsp;&nbsp;&nbsp; scheduleStatus     | String   | N   | 공연 상태      | SCHEDULED, OPEN, SOLD_OUT, COMPLETED, CANCELED  |
| &nbsp;&nbsp;&nbsp;&nbsp; scheduleDate        | dateTime | N   | 공연 날짜      | YYYY-MM-DD   HH24:MM:SS                         |


# 예약 가능 좌석 조회 API
- **Method:** GET
- **URL:** `/api/v1/shows/{showId}/{scheduleId}`
- **설명:** 선택한 날짜의 공연의 좌석 조회

## Request
### Header
| 이름          | 타입   | 필수 | 설명          |
|---------------|--------|------|---------------|
| Authorization | String | Y    | Bearer 토큰   |
| Queue-Token   | String        | Y   | 대기열 토큰     |      |

## Response
### Body
| 이름                              | 타입     | 필수  | 설명        | 기타                                                  |
|---------------------------------|--------|-----|-----------|-----------------------------------------------------|
| errorCode                       | String | Y   | 응답코드      |                                                     |
| message                         | String | Y   | 응답메시지     |                                                     |
| showId                          | String | N   | 공연 아이디    |                                                     |
| scheduleId                      | String | N   | 공연 회차 아이디 |                                                     |
| availableSeats                  | Array  | N   | 좌석 정보     |                                                     |
| &nbsp;&nbsp;&nbsp;&nbsp; seatId | String | N   | 좌석 아이디    |                                                     |
| &nbsp;&nbsp;&nbsp;&nbsp; grade  | String | N   | 좌석 등급     |                                                     |
| &nbsp;&nbsp;&nbsp;&nbsp; price  | int    | N   | 가격        |                                                     |
| &nbsp;&nbsp;&nbsp;&nbsp; status | String | N   | 상태        | AVAILABLE(구매 가능), RESERVED(결제 대기), BOOKED(좌석 예약 완료) |


# 좌석 예약 요청 API
- **Method:** POST
- **URL:** `/api/v1/reservation`
- **설명:** 선택한 날짜의 공연과 좌석을 예약

## Request 
### Header
| 이름            | 타입   | 필수 | 설명          |
|---------------|--------|------|---------------|
| Authorization | String | Y    | Bearer 토큰   |
| Queue-Token   | String        | Y   | 대기열 토큰     |      |

### Body
| 이름         | 타입            | 필수  | 설명            | 기타        |
|------------|---------------|-----|---------------|-----------|
| showId     | String        | Y   | 예약할 공연 아이디    |           |
| scheduleId | String        | Y   | 예약할 공연 회차 아이디 |           |
| seats      | Array<String> | Y   | 예약할 좌석        | 좌석 아이디 배열 |

## Response
### Body
| 이름            | 타입            | 필수  | 설명      | 기타                                                          |
|---------------|---------------|-----|---------|-------------------------------------------------------------|
| errorCode     | String        | Y   | 응답코드    |                                                             |
| message       | String        | Y   | 응답메시지   |                                                             |
| reservationId | String        | N   | 예약 아이디  |                                                             |
| totalPrice    | String        | N   | 총 결제 금액 |                                                             |
| status        | String        | N   | 예약 상태   | RESERVED(결제 대기), COMPLETED(결제완료), CANCELED(취소), EXPIRED(만료) |

# 좌석 예약 요청 API
- **Method:** GET
- **URL:** `/api/v1/reservation/{reservationId}`
- **설명:** 예약 상세 조회

## Request
### Header
| 이름            | 타입   | 필수 | 설명          |
|---------------|--------|------|---------------|
| Authorization | String | Y    | Bearer 토큰   |
| Queue-Token   | String        | Y   | 대기열 토큰     |      |


## Response
### Body
| 이름                              | 타입            | 필수  | 설명       | 기타                                                          |
|---------------------------------|---------------|-----|----------|-------------------------------------------------------------|
| errorCode                       | String        | Y   | 응답코드     |                                                             |
| message                         | String        | Y   | 응답메시지    |                                                             |
| reservationId                   | String        | N   | 예약 아이디   |                                                             |
| reservedSeats                   | Array  | N   | 예약 좌석 정보 |                                                     |
| &nbsp;&nbsp;&nbsp;&nbsp; seatId | String | N   | 좌석 아이디   |                                                     |
| &nbsp;&nbsp;&nbsp;&nbsp; grade  | String | N   | 좌석 등급    |                                                     |
| &nbsp;&nbsp;&nbsp;&nbsp; price  | int    | N   | 가격       |                                                     |
| totalPrice                      | String        | N   | 총 결제 금액  |                                                             |
| status                          | String        | N   | 예약 상태    | RESERVED(결제 대기), COMPLETED(결제완료), CANCELED(취소), EXPIRED(만료) |


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
- **Method:** GET
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

### Parameters
| 이름       | 타입 | 필수  | 설명      |
|----------|--|-----|---------|
| page     | int | N   | 페이지     |
| pageSize | int | N   | 페이지 사이즈 |
| type     | String | N   | 거래 타입 |
| from     | date | N   | 조회 시작 날짜  |
| to     | date | N   | 조회 마지막 날짜  |


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
| Queue-Token   | String        | Y   | 대기열 토큰     |      |


### Body
| 이름            | 타입     | 필수  | 설명     | 기타                    |
|---------------|--------|-----|--------|-----------------------|
| reservationId | String | Y   | 예약 아이디 |                       |
| paymentType   | String | Y   | 결제 타입  |                       |

## Response
### Body
| 이름                              | 타입     | 필수  | 설명       | 기타                    |
|---------------------------------|--------|-----|----------|-----------------------|
| errorCode                       | String | Y   | 응답코드     |                       |
| message                         | String | Y   | 응답메시지    |                       |
| paymentId                       | String | N   | 결제 아이디   |                       |
| status                          | String | N   | 결제 상태    |                       |
| totalPrice                      | String | N   | 결제 총 금액  |                       |
| paymentDate                     | String | N   | 결제 일시    | YYYY-MM-DD HH24:MM:SS |
| reservedShowTitle               | String | N   | 예약 공연 명  |                       |
| reservedSeats                   | Array  | N   | 예약 좌석 정보 |                       |
| &nbsp;&nbsp;&nbsp;&nbsp; seatId | String | N   | 좌석 아이디   |                       |
| &nbsp;&nbsp;&nbsp;&nbsp; grade  | String | N   | 좌석 등급    |                       |
| &nbsp;&nbsp;&nbsp;&nbsp; price  | int    | N   | 가격       |                       |
