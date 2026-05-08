# WorkMatch v1.2.1

v1.2.0 시리즈 (베타 1~4) 의 Tier 1 hotfix + Tier 3 #2/#3 + 가족분 폰 검증 hotfix 누적분 종합. **stable 정식 patch 릴리스**.

build: `1.2.1+2026050802`

## 누적 변경분 (v1.2.0 → v1.2.1)

### PR #192 — 시스템 백 버튼 + 입력 글씨 색상 (Tier 1 hotfix)
- 비-홈 탭 → 시스템 백 버튼 → 홈 탭 복귀 (`PopScope`)
- 홈 탭 → 시스템 백 버튼 → 앱 종료
- 인력 요청 등록 / 도로명 주소 검색 입력 글씨 일부 단말 / dark 모드 가시성 개선

### PR #196 — 등록 인력 검색 (Tier 3 #2)
- `_SimpleListPage` 공통 컴포넌트 검색
- 매치 필드 (substring lowercase): name / phone / employeeNo / jobCategory / title / publicTitle / senderName

### PR #197 — 총 요청 인력 정렬/필터 (Tier 3 #3)
- 기본 정렬: `createdAt DESC`
- 정렬 키 (단일): 등록일 / 근무일 / 직종 / 시설 + desc/asc
- 상태 필터 (다중): 검토대기 / 승인 / 부분충원 / 진행중 / 완료 / 취소

### PR #208 — 가족분 검증 hotfix (도로명 + 등록 인력 검색/클릭)

#### Issue 1 — 도로명 주소 검색 입력 글씨 안 보임 (PR #192 후속)
- `Theme.of(context).brightness` 명시 분기 (colorScheme 의존 제거)
- 색 hardcode: text white/black87, fill #2A2A2A/#F5F5F5, hint white70/black54
- enabledBorder + focusedBorder + cursorColor 명시

#### Issue 2 — 홈 > 등록 인력 검색 누락 + 워커 클릭 미동작 (신규)
- `StatDetailPage` 모든 statType 에 검색 일관 적용 (workers/agencies/headcount/activeRequests)
- 매치 필드 statType 별 분기 (PR #196 패턴 일관)
- 워커 행 클릭 → 약식 인사카드 다이얼로그 (이름/사번/전화/직종/평점/근무일/상태/근무상태/가입일)
- 추가 API 호출 X, list 응답 데이터 활용
- PII 적은 기본 정보만 (RRN/계좌/장애 등은 admin SPA 전용)

## 호환성
- 모든 역할 적용
- 백엔드 변경 없음 (mobile-only)

## 단말 검증 시나리오 (v1.2.0-beta 시리즈 28건 + 신규)

### Tier 1 (PR #192)
1. 비-홈 탭 → 시스템 백 버튼 → 홈 탭 복귀
2. 홈 탭 → 시스템 백 버튼 → 앱 종료
3. 인력 요청 등록 화면 → 모든 입력 필드 글씨 가시성
4. 도로명 주소 검색 → 검색 결과 ListTile 글씨 + **입력 글자 가시성 (PR #208 후속)**

### Tier 3 #2 (PR #196)
5. 인력 현황 → 검색 (이름/전화/사번/직종 부분 매치)
6. 검색 clear → 전체 복귀
7. 매치 0건 → 안내 메시지
8. 업체 현황 등 다른 목록도 동일 작동

### Tier 3 #3 (PR #197)
9. 총 요청 인력 → StatDetailPage 진입
10. 기본 정렬 = 등록일 ↓
11. 정렬 BottomSheet → 근무일 / 직종 / 시설
12. 방향 토글
13. 다중 상태 필터 → 매치/전체 카운트
14. 초기화
15. 매치 0건 안내
16. 다른 statType 정렬/필터 미노출

### PR #208 신규 (등록 인력)
17. 홈 > 등록 인력 → 검색 sticky 노출
18. "홍" 검색 → 매치만 표시 + `M / N` 카운트
19. × 버튼 → 전체 복귀
20. 매치 0건 → "조건에 맞는 항목이 없습니다"
21. 워커 행 클릭 → 약식 인사카드 다이얼로그
22. 다이얼로그 표시: 이름 / 사번 / 전화 / 직종 / 평점 / 근무일 / 상태 / 가입일
23. "닫기" → list 복귀
24. 홈 > 업체/총요청/활성요청 카드도 검색 노출
25. 총 요청 인력 (=headcount) PR #197 정렬/필터 IconButton 유지

## 다운로드

[workmatch.apk (28.2MB)](https://github.com/binchan7001/workmatch-releases/releases/download/v1.2.1/workmatch.apk) — arm64-v8a only

[workmatch.kr/#app](https://workmatch.kr/#app) 에서도 바로 받기 가능.
