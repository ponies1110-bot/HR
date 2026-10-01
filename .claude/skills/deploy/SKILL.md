---
name: deploy
description: 작업 내용을 GitHub에 올리고 Vercel 배포(Preview/Production)까지 진행한다. "배포해줘", "올려줘", "main에 반영", "PR 만들어줘" 같은 요청에 사용한다.
---

# 배포 (deploy)

Vercel은 GitHub 저장소와 연결되어 있어 push만 하면 자동 배포된다.
- 작업 브랜치에 push → Preview 배포
- `main`에 반영 → Production 배포

## 순서
1. `dev-check` 스킬로 린트/타입 검사/빌드를 통과시킨다.
2. 변경 내용을 커밋하고 현재 작업 브랜치에 push한다 (`git push -u origin <브랜치>`).
3. Production 배포가 필요하면 작업 브랜치 → `main` PR을 만든다. 사용자가 요청할 때만 PR을 만들고, 머지는 사용자가 결정한다.
4. `main`에 직접 push하지 않는다.

## 새 환경변수를 추가했을 때
코드에서 새 환경변수를 쓰기 시작했다면 사용자에게 알려준다.
- Vercel: Project → Settings → Environment Variables에 추가 후 재배포
- `.env.example`에도 변수 이름(값 없이)을 추가한다.

## 참고
- `VERCEL_TOKEN`이 있고 `vercel.com` 접속이 허용된 경우에만 `npx vercel` CLI를 사용할 수 있다. 기본은 GitHub 연동 자동 배포다.
