# `dp_core` 핀 갱신

`apps/mobile/pubspec.yaml` 의 `dp_core.git.ref` 는 `devpath-frontend` 의 40자 커밋 SHA 다. 브랜치명·태그를 쓰지 않는다.

## 언제

서버 API·모델 계약이 바뀌어 모바일이 그 변경을 받아야 할 때.

## 절차

1. 대상 커밋을 고른다: `git ls-remote https://github.com/DevPathAi/devpath-frontend.git refs/heads/main`
2. `develop` 에서 `chore/bump-dp-core-<짧은SHA>` 브랜치를 만든다.
3. `apps/mobile/pubspec.yaml` 의 `ref` 를 새 SHA 로 바꾼다.
4. `flutter pub get` → `pubspec.lock` 의 `dp_core.resolved-ref` 가 새 SHA 인지 확인한다.
5. `cd apps/mobile && flutter analyze && flutter test --exclude-tags golden`
6. PR → CI 녹색 → 머지. PR 본문에 frontend 쪽 변경 범위를 적는다:
   `git -C <frontend clone> log --oneline <옛SHA>..<새SHA> -- packages/dp_core`
