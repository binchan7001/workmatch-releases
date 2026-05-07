# WorkMatch v1.2.0-beta.2

PR [#192](https://github.com/binchan7001/workmatch/pull/192) 모바일 관리자 UX Tier 1 hotfix 적용 빌드.

build: `1.2.0+2026050701`

## 주요 변경

### #4 시스템 백 버튼 → 홈 탭 복귀 (PopScope)

| 항목 | 내용 |
|---|---|
| 증상 | 인력 요청 등 비-홈 탭에서 폰 시스템 백 버튼 누르면 앱 종료 또는 무시되어 사용자가 "홈 탭 복귀" 멘탈 모델과 충돌 |
| Root Cause | `home_page.dart` BottomNavigationBar 탭들이 `Navigator.push` 가 아닌 `pages[_currentIndex]` 직접 표시 → `Navigator.canPop()` = false |
| Fix | `Scaffold` 를 `PopScope` 로 감싸 `_currentIndex != 0` 일 때 가로채서 홈 탭으로 복귀 |

### #6 인력 요청 등록 입력 글씨 색상 명시

| 항목 | 내용 |
|---|---|
| 증상 | 인력 요청 등록 화면 입력 / 도로명 주소 검색 결과에서 일부 단말 / dark 모드에서 글씨가 배경과 같은 색으로 묻힘 |
| Root Cause | `AddressSearchDialog` 검색 결과 ListTile / `CreateRequestPage` 내 TextFormField 들에 색상 명시 누락 |
| Fix | 검색 결과 ListTile 에 `colorScheme.onSurface` / `onSurfaceVariant` 명시 + `CreateRequestPage` 모든 TextFormField (제목 / 직종 / 인원 / 시급 / 일시 / 주소 / 지오펜스 / 상세설명) 와 선택된 도로명 표시 Text 에 공통 `inputTextStyle` 적용 |

## 호환성

- 모든 역할 적용 (관리자 / 시설 / 워커)
- 백엔드 변경 없음 (mobile-only hotfix)

## 단말 검증

1. 앱 진입 → 인력 요청 탭 (또는 다른 비-홈 탭) → 시스템 백 버튼 → **홈 탭 복귀 ✅**
2. 홈 탭에서 시스템 백 버튼 → **앱 종료 (시스템 디폴트) ✅**
3. 인력 요청 등록 진입 → 모든 입력 필드 글씨 가시성 ✅
4. 도로명 주소 검색 다이얼로그 → 검색 결과 ListTile 글씨 ✅
5. 도로명 주소 선택 후 표시되는 텍스트 ✅

## 다운로드

[workmatch.apk (28.1MB)](https://github.com/binchan7001/workmatch-releases/releases/download/v1.2.0-beta.2/workmatch.apk) — arm64-v8a only
