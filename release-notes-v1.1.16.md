# WorkMatch v1.1.16-beta

## 주요 변경 (단일 세션 추가 PR)

### 메시징 기능 (관리자 전용) — [#119](https://github.com/binchan7001/workmatch/pull/119)
시스템 관리자(SUPER_ADMIN) / 운영 관리자(OPS_MANAGER) 가 모바일 앱에서도 알림·메시지 발송 가능.
- **위치**: 프로필 → 메시징 메뉴 (실시간 채팅 위)
- **TabBar**: 알림 / 메시지
- **목록**: 보낸 알림 / 보낸 메시지 (RefreshIndicator pull-to-refresh)
- **발송**: FAB 다이얼로그 (제목 + 내용 + 유형/수신자)
- 백엔드 변경 0 — 기존 admin 웹 엔드포인트 그대로 사용

### 실시간 채팅 입력 즉시 표시 버그 수정 — [#119](https://github.com/binchan7001/workmatch/pull/119)
**문제**: 메시지 전송 시 본인 메시지가 WebSocket echo 가 도착할 때까지 화면에 안 나타남. echo 지연/실패 시 사용자는 자기 메시지가 사라진 것처럼 느꼈음.

**수정**: 낙관적 UI — 본인 메시지를 _pending=true 로 즉시 표시. WebSocket echo 도착 시 동일 sender + content 의 pending 항목 dedupe 후 서버 본 추가. 전송 실패 시 낙관적 메시지 제거.

## 호환성
- 메시징 메뉴는 **SUPER_ADMIN / OPS_MANAGER 만 노출**. 다른 역할에는 영향 없음.
- 채팅 버그 수정은 **모든 역할** 적용.

## 단말 검증 항목
1. SUPER_ADMIN / OPS_MANAGER 로그인 → 프로필 → 메시징 메뉴 노출
2. 알림 발송 → 보낸 알림 탭에 즉시 표시
3. 메시지 발송 → 보낸 메시지 탭에 즉시 표시
4. 실시간 채팅 → 메시지 입력 → 즉시 화면 표시 (echo 도착 후 중복 X)

## 다운로드
[workmatch.apk (28.1MB)](https://github.com/binchan7001/workmatch-releases/releases/download/v1.1.16-beta/workmatch.apk) — arm64-v8a only
