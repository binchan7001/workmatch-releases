# WorkMatch v1.1.15-beta

## 주요 변경 (단일 세션 7 PR 통합)

### GPS Hybrid 시스템 (Phase 1~5) — 백엔드 + native plugin
- **백엔드** ([#109](https://github.com/binchan7001/workmatch/pull/109), [#113](https://github.com/binchan7001/workmatch/pull/113)): `worker_logs.event_type` (POLL/HEARTBEAT/CLOCK_IN/CLOCK_OUT/OFFSITE_ENTER/OFFSITE_EXIT), `LocationSettings.mode` (POLL|HYBRID), 이벤트 batch 엔드포인트 `/locations/events`, 워커 경로 조회 `/locations/worker/{id}/path`
- **Android native plugin** ([#110](https://github.com/binchan7001/workmatch/pull/110)): Geofencing API + Foreground heartbeat service + EncryptedSharedPreferences (배터리 효율 ~80% ↓)
- **iOS native plugin** ([#111](https://github.com/binchan7001/workmatch/pull/111)): CLLocationManager region monitoring + Significant Location Changes + Keychain (백그라운드 위치 합법 사용)
- **Flutter wrapper** ([#112](https://github.com/binchan7001/workmatch/pull/112)): Dart GpsHybrid 래퍼 + 권한 단계별 안내 (whileInUse → always)
- **Attendance 통합** ([#115](https://github.com/binchan7001/workmatch/pull/115)): attendance_page 모드 분기 (POLL = 기존 폴링, HYBRID = 이벤트 기반)

### 모바일 권한 동적화 (Phase 1~2) — 역할별 메뉴 자동 적용
- **PermissionsService 도입** ([#114](https://github.com/binchan7001/workmatch/pull/114)): `/auth/menus/by-role` 호출 후 ValueNotifier 로 메뉴 키 캐싱, PermissionGate 위젯 제공
- **login/logout 자동 sync** ([#116](https://github.com/binchan7001/workmatch/pull/116)): 로그인 직후 refresh, 로그아웃 시 clear (관리자 웹과 동일한 권한 매트릭스 모바일에 즉시 적용)

### 관리자 SPA 빌드 파이프라인 교체 ([#117](https://github.com/binchan7001/workmatch/pull/117))
- babel-cli + terser → esbuild + terser (트랜스파일 20x ↑, 전체 빌드 시간 절반)
- 모바일 무관 — APK 와 별도 운영. SPA 다이어트 Phase 1b/1c 의 사전 작업

## 호환성
- 백엔드 GPS Hybrid 엔드포인트는 master OCI 배포 후 활성. 본 APK 는 `LocationSettings.mode` 미설정 시 기존 POLL 모드로 자동 fallback.
- 권한 메뉴 동기화는 `/auth/menus/by-role` 백엔드 응답 의존. 실패 시 fail-open (모든 메뉴 표시).

## 단말 검증 항목 (베타 테스터 협조 요청)
1. 신규 GPS Hybrid 모드 (admin 에서 시설 단위 토글 후) — 포그라운드/백그라운드 동작
2. 출퇴근 화면 — POLL/HYBRID 모드 자동 분기 정상 여부
3. 사이드바 메뉴 — 역할 변경 시 자동 갱신 여부
4. APK 크기 28MB 기준선 회귀 없는지 확인

## 다운로드
[workmatch.apk (29.4MB)](https://github.com/binchan7001/workmatch-releases/releases/download/v1.1.15-beta/workmatch.apk) — arm64-v8a only (다이어트 기준)
