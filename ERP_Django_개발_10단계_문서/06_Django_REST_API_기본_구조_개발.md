# 6단계. Django REST API 기본 구조 개발

## 목적

Django REST Framework 기반으로 ERP API 서버의 기본 구조를 개발한다.

프론트엔드는 React / Refine으로 개발하고, 백엔드는 DRF API를 제공한다.

## DRF 구성 요소

각 앱은 다음 구조를 기본으로 한다.

- models.py
- serializers.py
- views.py
- urls.py
- permissions.py
- filters.py
- services.py
- admin.py
- tests.py

## API 기본 규칙

REST API 기준으로 설계한다.

자재 API 예시

- GET /api/materials/
- POST /api/materials/
- GET /api/materials/{id}/
- PUT /api/materials/{id}/
- PATCH /api/materials/{id}/
- DELETE /api/materials/{id}/

DELETE는 실제 삭제가 아니라 비활성화 처리한다.

## 주요 API 그룹

인증

- POST /api/auth/login/
- POST /api/auth/refresh/
- POST /api/auth/logout/
- GET /api/auth/me/

사용자

- GET /api/users/
- POST /api/users/
- GET /api/users/{id}/
- PATCH /api/users/{id}/
- POST /api/users/{id}/deactivate/

자재

- GET /api/materials/
- POST /api/materials/
- GET /api/materials/{id}/
- PATCH /api/materials/{id}/
- POST /api/materials/{id}/deactivate/

거래처

- GET /api/partners/
- POST /api/partners/
- GET /api/partners/{id}/
- PATCH /api/partners/{id}/

창고 및 위치

- GET /api/warehouses/
- POST /api/warehouses/
- GET /api/warehouse-locations/
- POST /api/warehouse-locations/

입고

- GET /api/stock-in/
- POST /api/stock-in/
- GET /api/stock-in/{id}/
- POST /api/stock-in/{id}/cancel/

출고

- GET /api/stock-out/
- POST /api/stock-out/
- GET /api/stock-out/{id}/
- POST /api/stock-out/{id}/cancel/

재고

- GET /api/stock-balances/
- GET /api/stock-transactions/
- POST /api/stock-adjustments/

## Serializer 설계

Serializer는 입력용과 출력용을 분리한다.

예시

- MaterialListSerializer
- MaterialDetailSerializer
- MaterialCreateSerializer
- MaterialUpdateSerializer

분리 이유

- 목록 조회는 가볍게 응답
- 상세 조회는 상세 정보 포함
- 등록 및 수정은 검증 로직 포함
- 불필요한 데이터 노출 방지

## ViewSet 사용

기본 CRUD는 ModelViewSet을 사용한다.

복잡한 업무 로직은 ViewSet에 직접 작성하지 않고 service 계층으로 분리한다.

처리 흐름

- ViewSet
- Serializer
- Service
- Model
- Database

## Service 계층 분리

입고, 출고, 재고 조정처럼 업무 로직이 중요한 기능은 service.py에 작성한다.

예시

- create_stock_in
- cancel_stock_in
- create_stock_out
- cancel_stock_out
- adjust_stock
- update_stock_balance
- create_stock_transaction

## 필터 및 검색

django-filter를 사용한다.

주요 검색 조건

자재

- 자재코드
- 자재명
- 자재구분
- 사용여부

현재고

- 자재
- 창고
- 위치
- LOT
- 안전재고 미달 여부

입출고 이력

- 기간
- 자재
- 창고
- 입출고구분
- 담당자

## 페이지네이션

대량 데이터 조회를 대비하여 서버 사이드 페이지네이션을 적용한다.

기본 페이지 크기

- 50건

최대 페이지 크기

- 500건

대량 다운로드는 엑셀 다운로드 API를 별도로 제공한다.

## 공통 응답 구조

DRF 기본 응답 구조를 사용하되, 프론트엔드에서 처리하기 쉽도록 일관성을 유지한다.

성공 응답

- data
- message

오류 응답

- error_code
- message
- detail

## API 문서

drf-spectacular를 사용하여 OpenAPI 문서를 생성한다.

제공 문서

- Swagger UI
- ReDoc
- OpenAPI JSON

운영 환경에서는 인증된 사용자만 접근할 수 있도록 제한한다.

## 산출물

- DRF API 기본 구조
- Serializer 구조
- ViewSet 구조
- Service 계층 구조
- Filter 구조
- Pagination 설정
- API 문서 자동화

## 완료 기준

- DRF API 호출 가능
- Swagger 문서 확인 가능
- 자재 목록 API 구현
- 권한 없는 API 호출 차단
- 페이지네이션 적용
- 필터 검색 적용
