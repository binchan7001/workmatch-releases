# WorkMatch v1.2.0-beta.4

PR [#208](https://github.com/binchan7001/workmatch/pull/208) — 가족분 폰 v1.2.0-beta.3 라이브 검증에서 발견한 모바일 이슈 2건 hotfix.

build: `1.2.0+2026050801`

## Issue 1 — 도로명 주소 검색 입력 글씨 안 보임 (PR #192 후속)

### 증상
PR #192 hotfix 적용 후에도 가족분 폰 다크모드에서 인력 요청 등록 → 도로명 주소 검색 다이얼로그의 입력 텍스트가 배경과 거의 동일 검정 색이 되어 글자 안 보임. 보라색 커서만 표시.

### 원인
PR #192 가 `colorScheme.onSurface` + `surfaceContainerHighest` (Material 3 token) 만 명시했는데, 일부 Flutter SDK 버전에서 `surfaceContainerHighest` 가 fallback → `surface` 로 떨어져 입력 배경이 거의 검정. 동시에 `onSurface` 도 contrast 불충분으로 글자가 묻힘.

### Fix
`flutter-app/lib/features/requests/presentation/address_search_dialog.dart`:
- `Theme.of(context).brightness` 명시 분기 (`colorScheme` 의존 제거)
- 색 hardcode: text `white`/`black87`, fill `#2A2A2A`/`#F5F5F5`, hint `white70`/`black54`, border `#555`/`#CCC`
- `enabledBorder` + `focusedBorder` (focus 시 primary 2px) + `cursorColor` 명시

## Issue 2 — 홈 > 등록 인력 검색 누락 + 워커 클릭 미동작 (신규)

### 증상
- 등록 인력 화면(`StatDetailPage(statType: 'workers')`) 에 검색 기능 없음 — PR #196 가 admin_hub_page `_SimpleListPage` 에만 추가됐고 `StatDetailPage` 는 별도 컴포넌트라 누락
- 워커 행 클릭 시 아무 동작 없음

### Fix
`flutter-app/lib/features/home/presentation/stat_detail_page.dart`:

**검색 (모든 statType 일관 적용)**
- body 상단 sticky `TextField` (Issue 1 과 동일한 brightness 분기 hardcode 패턴)
- substring lowercase, statType 별 매치 필드:
  - `workers`: name / phone / employeeNo / jobCategory / preferredCategories
  - `agencies`: orgName / name / phone / contactPhone
  - `headcount` / `activeRequests`: title / publicTitle / jobCategory / facilityName
- clear (×) + 매치 카운트 (`전체 N` / `M / N`) + 빈 결과 안내

**워커 약식 인사카드 다이얼로그**
- `_buildWorkerItem` ListTile `onTap` → `_showWorkerInfoDialog`
- `/workers` list 응답 데이터 활용 (추가 API 호출 X)
- 표시: 이름 / 사번 / colorTag / 전화 / 직종 / 평점 / 근무일 / 상태 / 근무상태 / 가입일
- PII 적은 기본 정보만 (RRN/계좌/장애 등은 admin SPA `/workers/{id}/card` 만 노출 유지)

### 기존 동작 보존
- PR #197 의 headcount 정렬/필터 IconButton 그대로
- agencies/headcount/activeRequests 의 클릭 동작 (없던 것은 없음 유지)

## 호환성
- 모든 역할 적용 (시스템/운영 관리자 + 시설 관리자 등)
- 백엔드 변경 없음 (mobile-only)

## 단말 검증 (재테스트 권장)

### Issue 1 검증
1. 다크모드 단말 → 인력 요청 등록 → 도로명 주소 검색
2. 한글 입력 → **입력 글자 보임 ✅**
3. 검색 결과 ListTile 글씨 표시 (PR #192 fix 유지)

### Issue 2 검증
4. 홈 > 등록 인력 카드 → StatDetailPage 진입
5. AppBar 아래 sticky 검색 필드 노출
6. "홍" 검색 → 매치 워커만 표시 + `M / N` 카운트
7. × 버튼 → 전체 복귀
8. 매치 0건 → "조건에 맞는 항목이 없습니다" 안내
9. 워커 행 클릭 → 다이얼로그 (이름/사번/전화/직종/평점/근무일/상태/가입일)
10. 다이얼로그 "닫기" → list 복귀
11. 홈 > 업체/총요청/활성요청 카드도 검색 노출 (workers 외 statType)
12. 총 요청 인력(=headcount) 의 PR #197 정렬/필터 IconButton 유지

## 다운로드

[workmatch.apk (28.2MB)](https://github.com/binchan7001/workmatch-releases/releases/download/v1.2.0-beta.4/workmatch.apk) — arm64-v8a only
