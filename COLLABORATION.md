# Claude Code + Codex 협력 환경

## 개요

이 저장소는 Claude Code와 Codex(ChatGPT)가 함께 물리 시뮬레이션을 개발합니다.

- **Codex**: 물리 모델, 시뮬레이션 아키텍처 → `codex/` 브랜치
- **Claude Code**: UI, 테스트, 디버깅, 도구 → `claude/` 브랜치
- **협력**: Issues와 PRs를 통한 명시적 통신

---

## 빠른 시작

### 1단계: 새 작업 시작하기

```bash
# 최신 main 동기화
git fetch origin main
git checkout -B main origin/main

# 작업 브랜치 생성 (본인의 역할에 맞게)
git checkout -b claude/feature-name      # Claude Code
git checkout -b codex/feature-name       # Codex
```

### 2단계: Issue 생성

**제목**: `[Agent] Task description` (예: `[Claude] Add physics visualization`)

**템플릿 사용** (아래 참고)

### 3단계: 작업 및 커밋

```bash
# 작업 진행
git add .
git commit -m "변경 설명

- 무엇을 했나
- 왜 했나"
```

### 4단계: PR 생성

**PR 템플릿 사용** → 상대 에이전트의 리뷰 요청

### 5단계: 리뷰 및 병합

상대 에이전트가 변경사항 검토 → 승인 후 병합

---

## 파일 소유권 (책임 기준)

| 영역 | 담당 | 설명 |
|------|------|------|
| **물리/수학 모델** | Codex | 시뮬레이션 핵심 알고리즘 |
| **공개 인터페이스** | Codex | 모듈 간 계약 |
| **UI/표시** | Claude Code | 사용자 인터페이스 |
| **테스트** | Claude Code | 검증 및 회귀 테스트 |
| **도구/DevOps** | Claude Code | 빌드, 배포 자동화 |
| **디버깅** | Claude Code | 문제 재현 및 보고 |

**교차 영역**: Issue에서 소유권 정의

---

## 충돌 방지

작업 전 체크리스트:

```bash
# 최신 상태 확인
git fetch origin
git status

# 열린 PR 확인
# → GitHub에서 관련 파일 검사

# Issue 확인
# → 다른 에이전트가 같은 파일 작업 중인지 확인
```

**규칙**: 같은 파일을 동시에 수정하면 안 됨 → 먼저 Issue에서 조율

---

## 통신 형식

모든 Issue/PR 댓글에 이 구조 사용:

### STATUS
- 에이전트: (Claude Code / Codex)
- 작업: (간단한 설명)
- 브랜치: `claude/name` 또는 `codex/name`
- 상태: planning | working | blocked | ready

### CHANGED
- 파일 1: 변경 내용
- 파일 2: 변경 내용

### VERIFIED
- 테스트/명령어: `npm test`, `python test.py`, 등
- 결과: ✅ 통과 / ❌ 실패

### NEXT
- 다음 단계
- 상대 에이전트를 위한 제안

### BLOCKERS
- None / 문제 내용

---

## Issue 템플릿

```markdown
# [Claude/Codex] Task Title

## 목표
무엇을 해야 하는가? (명확하게)

## 배경
왜 이것이 필요한가?

## 구체적 요구사항
- 요구사항 1
- 요구사항 2
- 요구사항 3

## 성공 기준
- [ ] 조건 1 (테스트로 검증 가능)
- [ ] 조건 2
- [ ] 조건 3

## 제약
- 파일/모듈: 이 부분만 수정
- 물리 가정: 변경하면 안 되는 부분
- 다른 작업: 의존성 있는 작업

## 담당 에이전트
- [ ] Claude Code
- [ ] Codex

## 관련 파일
- `src/module/file.ts`
- `test/file.test.ts`
```

---

## PR 템플릿

```markdown
# [Claude/Codex] Short description

## 변경사항
이 PR이 무엇을 하는가? (명확한 문장)

- 파일 A: 구체적 변경
- 파일 B: 구체적 변경

## 검증
```bash
# 실행한 테스트
npm test

# 결과
✅ All tests pass
```

## 물리적 정확성
- [ ] 물리 가정 변경 없음
- [ ] 새로운 물리 가정이 있다면:
  - 가정 설명
  - 근거

## 리뷰 요청
@other-agent - 리뷰 요청

## 알려진 제약
- 제약 1
- 제약 2

## 다음 단계
- 이 PR 이후 할 일
- 상대 에이전트의 제안 작업
```

---

## TASKS.md 갱신

주요 작업 완료 후:

```bash
# TASKS.md 편집
# 1. 완료된 항목을 "Completed" 섹션으로 이동
# 2. 새 작업을 "In progress" 또는 "Ready"로 추가
# 3. 이 변경사항을 작업 브랜치에 커밋

git add TASKS.md
git commit -m "Update TASKS: [description]"
git push origin <branch>
```

TASKS.md 형식:

```markdown
## In progress

- [Issue #N] [Agent] Task name
  - Branch: `codex/name` or `claude/name`
  - Files: `src/a`, `test/b`
  - State: working on X

## Ready

- [Issue #N] [Agent] Task name
  - Branch: `codex/name` or `claude/name`
  - State: ready for review
```

---

## 물리 모델 무결성

이것은 물리 프로젝트입니다. 규칙:

1. **명시적 표시**: 물리 가정 ≠ 수치 방법 ≠ 렌더링 선택
2. **침묵의 변경 금지**: 물리 법칙을 바꾸면 Issue에서 설명
3. **불일치 추적**: 결과가 알려진 물리와 안 맞으면:
   - 상수 튜닝 대신 불일치를 노출
   - 원인 분석 제시
4. **리뷰**: 물리 변경 PR은 상대 에이전트 리뷰 필수

---

## 리뷰 프로세스

1. **작성자**: PR 생성 + 상대 에이전트에 `@mention`
2. **리뷰어**: 
   - 변경사항 읽기
   - 테스트 실행
   - 코드 변경 검토
   - GitHub에서 피드백
3. **작성자**: 
   - 의견 대응
   - 필요시 수정 푸시
4. **호스트(당신)**:
   - 물리/아키텍처 영향 있으면 결정
   - 승인 후 병합

---

## 커밋 메시지 규칙

```
한 줄 요약 (50자 이내)

더 자세한 설명 (필요시)
- 무엇을 변경했나
- 왜 변경했나
- 주요 결정사항

파일 목록 (자동):
- src/module/file.ts
- test/file.test.ts
```

예:
```
Add physics visualization panel

- Render particle positions and velocities
- Update each simulation tick
- Use Canvas 2D for performance

Files: src/ui/physics-view.ts, test/ui/physics-view.test.ts
```

---

## 긴급 상황

### 브랜치 충돌 (같은 파일 동시 수정)

1. Issue에서 통보: "파일 X 작업 중, 협력 방식 제안"
2. 상대 에이전트가 응답할 때까지 대기
3. 협력 방식:
   - **순차**: 한 명이 먼저 완료, 다른 한 명이 병합 후 진행
   - **분할**: 파일/함수를 나누어 작업

### 테스트 실패

1. PR 작성자가 실패 원인 파악
2. Issue에 "BLOCKED" 상태로 보고
3. 옵션:
   - 작성자가 수정
   - 상대 에이전트가 수정 제안
   - 호스트 조율

### 물리 가정 충돌

PR에서 "물리 변경 요청" 발견 시:
1. 즉시 중단 (병합 미루기)
2. Issue에서 상세 논의
3. 호스트의 승인 후 진행

---

## 체크리스트

### 작업 시작 전

- [ ] Issue 생성 및 템플릿 작성
- [ ] 담당 에이전트 확인
- [ ] 최신 main 동기화
- [ ] 관련 파일 검사
- [ ] 다른 에이전트의 작업 확인

### 커밋 전

- [ ] 로컬 테스트 통과
- [ ] 불필요한 파일 제외
- [ ] 커밋 메시지 작성
- [ ] Issue 참조 (`#123`)

### PR 생성 전

- [ ] 템플릿 사용
- [ ] 모든 테스트 통과 (CI/로컬)
- [ ] 상대 에이전트 @ 언급
- [ ] 물리 변경 있으면 명시

### 리뷰 요청 시

- [ ] 상대 에이전트의 책임 영역 확인
- [ ] 명확한 설명 제공
- [ ] 테스트 결과 포함

---

## 유용한 명령어

```bash
# 최신 상태 확인
git fetch origin
git log --oneline -10 origin/main

# 브랜치별 작업 현황
git for-each-ref --sort=-committerdate refs/remotes/origin \
  | head -20

# PR 상태 확인 (gh CLI)
gh pr list --state=open
gh pr view <NUMBER>  # 특정 PR 상세

# 테스트 실행
npm test              # 전체 테스트
npm test -- --watch  # 감시 모드
```

---

## 참고 문서

- **`docs/agent-protocol.md`**: 정책과 원칙 (변경하지 않음)
- **`AGENTS.md`**: Codex 운영 규칙
- **`CLAUDE.md`**: Claude Code 운영 규칙
- **`TASKS.md`**: 작업 보드 (실시간 갱신)
- **이 파일**: 협력 워크플로우 (실용 가이드)

---

## 질문?

호스트(당신)에게 물어보세요. 이 파일은 언제든 갱신될 수 있습니다.
