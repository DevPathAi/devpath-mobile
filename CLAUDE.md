# CLAUDE.md — devpath-mobile

> Leva 네이티브 모바일 앱(Flutter). 2026-09 에 `devpath-frontend` 에서 분리했다.
> 멤버: `apps/mobile`(앱) · `packages/dp_design`(디자인 시스템 — **이 레포가 소유하는 포크**).
> `dp_core`(API·모델)는 `DevPathAi/devpath-frontend` 를 커밋 핀 git 의존성으로 참조한다.

## 절대 조건

1. 추측·예상 금지 — 모르면 파일을 읽고 명령을 실행해 확인한다.
2. 테스트 우선 — 실패하는 테스트를 먼저 쓰고 통과시키는 최소 구현을 쓴다.
3. 문제가 생기면 코드·로그를 읽어 원인을 규명한 뒤 고친다.
4. 신규 작업은 `develop` 에서 새 브랜치를 분기한다.
5. 검증되지 않은 완료를 보고하지 않는다.

## Git 브랜치 전략

`main` 은 보호 브랜치다. 직접 커밋·푸시·force-push 금지. `develop` 이 통합 브랜치다.
작업 브랜치(`feat/*`·`fix/*`·`chore/*`·`docs/*`) → `develop` PR → CI 녹색 → merge commit.
릴리스 때만 `develop` → `main` PR.

## 빌드·테스트 (레포 루트 기준)

- 의존성: `flutter pub get --enforce-lockfile`
- 소스 가드: `dart run tools/mobile_source_guard.dart`
- 분석·테스트: `cd apps/mobile && flutter analyze && flutter test --exclude-tags golden` (`packages/dp_design` 동일)
- 포맷: `dart format --set-exit-if-changed apps/mobile packages/dp_design tools/mobile_source_guard.dart`
- Android/iOS 빌드 계약은 CI(`.github/workflows/mobile.yml`)가 판정한다.

## 디자인 시스템

`packages/dp_design` 은 mobile-first 시각 언어(플로팅 하단 내비, 큰 반경, 44px 터치 타깃)를 유지한다.
`devpath-frontend` 의 `dp_design` 은 2026-09 부터 데스크톱 웹 문법으로 갈라진다 — **두 패키지를 서로 동기화하지 않는다.**
공통으로 남는 것은 브랜드 색·타이포 값뿐이다.

## `dp_core` 핀

갱신 절차는 `docs/dp-core-pin.md`.

## 아직 옮기지 않은 것

서명 릴리스 파이프라인(`mission-spine-signed-mobile-build.yml`)과 수동 접근성 증거 워크플로는 `devpath-frontend` 의
웹 릴리스 증거 도구에 묶여 있어 가져오지 않았다. 이 레포의 독립 서명·배포 파이프라인은 별도 계획으로 새로 설계하고,
그때 계약 테스트도 함께 쓴다. 서명 시크릿 4종과 환경 `mission-spine-mobile-signing-android` 도 그때 옮긴다.
