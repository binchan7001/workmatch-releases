# WorkMatch v1.2.0-beta.3

PR [#192](https://github.com/binchan7001/workmatch/pull/192) Tier 1 hotfix + PR [#196](https://github.com/binchan7001/workmatch/pull/196) 등록 인력 검색 + PR [#197](https://github.com/binchan7001/workmatch/pull/197) 총 요청 인력 정렬/필터 종합 빌드.

build: `1.2.0+2026050702`

## PR #192 — 시스템 백 버튼 + 입력 글씨 색상 (Tier 1 hotfix)

| 항목 | 내용 |
|---|---|
| #4 | 인력 요청 등 비-홈 탭에서 시스템 백 버튼 → 홈 탭 복귀 (`PopScope`). 홈 탭에서 백 버튼 → 앱 종료 (시스템 디폴트). |
| #6 | 인력 요청 등록 / 도로명 주소 검색 입력 글씨가 일부 단말 / dark 모드에서 묻히던 문제 수정. 모든 TextFormField + 검색 결과 ListTile 에 `colorScheme.onSurface` 명시. |

## PR #196 — 등록 인력 검색 (Tier 3 #2)

`_SimpleListPage` 공통 컴포넌트에 검색 추가. 인력 / 업체 / 출결 / 공지 등 admin 목록 모두에 일관 적용.

- 매치 필드 (substring, lowercase): `name` / `phone` / `employeeNo` / `jobCategory` / `title` / `publicTitle` / `senderName`
- AppBar 아래 sticky `TextField` (테마 onSurface 색상 명시, dark/일부 단말 호환)
- Clear (×) 버튼, 카운트 표시 (`전체 N` / `M / N`), 빈 결과 안내

## PR #197 — 총 요청 인력 정렬/필터 (Tier 3 #3)

홈 대시보드 '총 요청 인력' 카드 → `StatDetailPage` (`statType='headcount'` 한정) 에 정렬/필터 추가.

- **기본 정렬**: `createdAt DESC` (최근 등록 위쪽)
- **정렬 키** (단일): 등록일 / 근무일 / 직종 / 시설 + desc/asc 토글
- **상태 필터** (다중 선택): 검토대기 / 승인 / 부분충원 / 진행중 / 완료 / 취소
- AppBar 정렬 + 필터 IconButton (필터 활성 시 dot 배지)
- body 상단 정렬/필터 상태 strip + 매치 카운트
- BottomSheet (정렬: 라디오 + 방향 / 필터: FilterChip + 초기화 + 적용)
- 영향 범위: `headcount` 만. `activeRequests` / `agencies` / `workers` 는 기존 동작 유지.

## 호환성

- 모든 역할 적용 (관리자 / 시설 / 워커) — PR #192 한정
- PR #196/#197 은 admin/manager 진입 화면 (시스템/운영 관리자 + 시설 관리자) 에 노출
- 백엔드 변경 없음 (mobile-only)

## 단말 검증

### PR #192 검증
1. 비-홈 탭 → 시스템 백 버튼 → **홈 탭 복귀 ✅**
2. 홈 탭 → 시스템 백 버튼 → **앱 종료 ✅**
3. 인력 요청 등록 진입 → 모든 입력 필드 글씨 가시성 ✅
4. 도로명 주소 검색 다이얼로그 → 검색 결과 ListTile 글씨 ✅

### PR #196 검증
5. 관리자 허브 → '인력 현황' → 검색 입력 (이름/전화/사번/직종 부분 매치) ✅
6. 입력값 clear → 전체 목록 복귀 ✅
7. 매치 0건 → '검색 결과가 없습니다' 안내 ✅
8. '업체 현황' / 다른 목록도 검색 동일 작동 ✅

### PR #197 검증
9. 홈 대시보드 → '총 요청 인력' 카드 → StatDetailPage 진입 ✅
10. 기본 정렬 = 등록일 ↓ (최근 위쪽) ✅
11. 정렬 BottomSheet → 근무일 / 직종 / 시설 각각 적용 ✅
12. 방향 토글 (오름차순) ✅
13. 상태 필터 BottomSheet → 다중 선택 → 매치/전체 카운트 변화 ✅
14. '초기화' → 필터 해제 ✅
15. 매치 0건 → '조건에 맞는 항목이 없습니다' ✅
16. '활성 요청' / '업체' / '워커' StatDetailPage 는 정렬/필터 미노출 (기존 동작) ✅

## 다운로드

[workmatch.apk (28.1MB)](https://github.com/binchan7001/workmatch-releases/releases/download/v1.2.0-beta.3/workmatch.apk) — arm64-v8a only
