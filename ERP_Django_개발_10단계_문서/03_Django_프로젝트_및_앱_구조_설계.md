# 3단계. Django 프로젝트 및 앱 구조 설계

## 목적

Django 기반 ERP 백엔드의 프로젝트 구조와 앱 분리 기준을 설계한다.

ERP는 기능이 계속 늘어나는 시스템이므로 처음부터 앱 단위로 모듈을 분리해야 한다.

## Django 프로젝트 구조

backend

- config
  - settings.py
  - urls.py
  - asgi.py
  - wsgi.py
- apps
  - accounts
  - common
  - partners
  - materials
  - warehouses
  - inventory
  - audit
  - notifications
  - purchases
  - approvals
  - production
  - integrations
- manage.py
- requirements.txt
- Dockerfile
- docker-compose.yml

## 앱 분리 기준

accounts

- 사용자
- 부서
- 그룹
- 권한
- 로그인
- 사용자 프로필

common

- 공통코드
- 공통 상수
- 공통 유틸
- 파일 처리
- 공통 응답

partners

- 거래처
- 공급처
- 고객사
- 담당자

materials

- 자재 마스터
- 자재 분류
- 단위
- 안전재고

warehouses

- 창고
- 창고 위치
- 보관 위치

inventory

- 입고
- 출고
- 현재고
- 입출고 이력
- 재고 조정

audit

- 변경이력
- 접속로그
- 작업로그

notifications

- 내부 알림
- 공지사항
- 알림 읽음 처리

purchases

- 구매신청
- 발주관리
- 후속 확장용

approvals

- 결재관리
- 후속 확장용

production

- BOM
- 작업지시
- 생산관리
- 후속 확장용

integrations

- 은행 연동
- 홈택스 연동
- 하이웍스 연동
- 외부 시스템 연동

## 권장 패키지

필수

- django
- djangorestframework
- django-filter
- django-cors-headers
- drf-spectacular
- psycopg
- python-dotenv
- gunicorn

엑셀 처리

- openpyxl
- pandas

비동기 작업

- celery
- redis

운영 보조

- whitenoise
- django-environ

## Django 설정 분리

운영과 개발 설정을 분리한다.

권장 구조

- settings/base.py
- settings/local.py
- settings/dev.py
- settings/prod.py

설정 분리 항목

- DEBUG
- SECRET_KEY
- DATABASES
- ALLOWED_HOSTS
- CORS_ALLOWED_ORIGINS
- STATIC_ROOT
- MEDIA_ROOT
- LOGGING
- EMAIL 설정
- 보안 설정

## API 문서화

DRF API 문서화를 위해 drf-spectacular를 사용한다.

API 문서 제공

- Swagger UI
- OpenAPI Schema
- ReDoc

주의사항

- 운영 환경에서는 API 문서 접근 권한을 제한한다.
- 내부망이라도 인증 없이 공개하지 않는다.

## 인증 방식

초기에는 Django Auth 기반으로 구성한다.

React 프론트와 연동하기 위해 아래 방식 중 하나를 선택한다.

선택안 1

- 세션 인증
- CSRF 적용
- 내부망 ERP에 적합

선택안 2

- JWT 인증
- React SPA와 API 분리 구조에 적합

권장

- React / Refine과 분리 운영할 것이므로 JWT 인증을 우선 검토한다.
- 단, Django Admin은 Django 기본 세션 인증을 그대로 사용한다.

## 산출물

- Django 프로젝트 구조
- 앱 분리 기준
- requirements.txt 초안
- settings 분리 기준
- API 문서화 기준

## 완료 기준

- Django 프로젝트 생성 가능
- 앱 분리 기준 확정
- 기본 패키지 목록 확정
- 설정 분리 기준 확정
