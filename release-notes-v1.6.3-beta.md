# WorkMatch v1.6.3 (beta)

**릴리스 일자**: 2026-06-01
**APK 크기**: arm64 35.8MB

## 변경 사항

### 출퇴근 화면 퍼포먼스 개선 (체감 응답성 + 배터리 절약)

`attendance_page.dart` 의 폴링 동작이 30초/45초/20초마다 전체 위젯 트리를 재빌드하던 부담을 해소.

- **ValueNotifier 3개 도입** — QR 윈도우 (30초), 지오펜스 (45초), 조기퇴근 (20초) 폴링이 해당 UI 영역만 부분 재빌드
- **History 탭 분리** — `_HistoryTabView` StatefulWidget 으로 격리, chip 토글이 Today 탭에 영향 없음
- **Today 탭 위젯 추출** — 지오펜스 배지 / 배포 카드 / 보고 버튼을 순수 StatelessWidget 으로 분리, const 최적화

### 백엔드 변경 없음 (v1.6.2 와 동일 API 호환)

## 호환성

- Android 5.0+ (arm64-v8a)
- 백엔드 API: https://api.workmatch.kr/v1
- v1.6.2 에서 v1.6.3 으로 무중단 업그레이드

## 다운로드

- [workmatch.apk](https://github.com/binchan7001/workmatch-releases/releases/download/v1.6.3-beta/workmatch.apk) (arm64-v8a, 36MB)
