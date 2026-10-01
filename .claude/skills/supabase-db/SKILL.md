---
name: supabase-db
description: Supabase 프로젝트 연결, 테이블 생성/변경(마이그레이션), 원격 DB 반영을 처리한다. "테이블 만들어줘", "DB 스키마 변경", "Supabase 연결", "마이그레이션" 같은 요청에 사용한다.
---

# Supabase DB 작업 (supabase-db)

## 사전 조건 확인
값은 출력하지 말고 존재 여부만 확인한다.
- `NEXT_PUBLIC_SUPABASE_URL`, `NEXT_PUBLIC_SUPABASE_ANON_KEY`: 앱에서 사용
- `SUPABASE_ACCESS_TOKEN`: CLI로 원격 프로젝트에 연결/반영할 때 필요
- 네트워크에서 `supabase.com`, `*.supabase.co` 접속 가능 여부

없으면 작업을 멈추고, 사용자에게 클라우드 환경 설정(Edit)에서 환경변수/허용 도메인을 추가한 뒤 새 세션을 열라고 안내한다. 토큰을 채팅에 붙여넣으라고 요청하지 않는다.

## 프로젝트 연결 (최초 1회)
- 프로젝트 ref는 `NEXT_PUBLIC_SUPABASE_URL`의 서브도메인(`https://<ref>.supabase.co`)이다.
- `npx supabase link --project-ref <ref>`

## 테이블 변경 흐름
1. `npm run db:new -- <변경_이름>` → `supabase/migrations/<timestamp>_<변경_이름>.sql` 생성
2. 생성된 SQL 파일에 변경 내용을 작성한다.
   - 새 테이블에는 항상 RLS를 켠다: `alter table <name> enable row level security;`
   - 필요한 정책(policy)을 함께 작성한다.
3. 원격 DB에 반영하기 전에 사용자에게 변경 내용을 요약해 확인받는다.
4. `npm run db:push`로 반영한다.
5. 마이그레이션 파일을 커밋한다.

## 코드에서 사용
- 클라이언트 컴포넌트: `@/lib/supabase/client`의 `createClient()`
- 서버 컴포넌트 / Route Handler / Server Action: `@/lib/supabase/server`의 `await createClient()`
- service_role 키는 클라이언트 코드나 `NEXT_PUBLIC_` 변수에 절대 넣지 않는다.
