# 빠른 참고서 (Quick Reference)

> 협력 워크플로우 한 장 정리

## 역할 (Role)

| 역할 | 담당 | 브랜치 | Issue라벨 |
|------|------|--------|----------|
| **Codex** | 물리 모델, 아키텍처, 통합 | `codex/name` | `codex` |
| **Claude Code** | UI, 테스트, 도구, 디버깅 | `claude/name` | `claude-code` |

---

## 빠른 시작 (Quick Start)

### 1. Issue 생성 (Issue Create)
```
제목: [Claude] Task description
라벨: claude-code / codex / task
템플릿: Task 템플릿 사용 (.github/ISSUE_TEMPLATE/task.md)
```

### 2. 브랜치 생성 (Branch)
```bash
git fetch origin main
git checkout -b claude/feature-name  # 또는 codex/feature-name
```

### 3. 커밋 (Commit)
```bash
git add .
git commit -m "Change description

- what changed
- why changed"
```

### 4. PR 생성 (Pull Request)
```
제목: [Agent] Change description
템플릿: PR 템플릿 사용 (.github/pull_request_template.md)
리뷰어: @other-agent
```

### 5. 리뷰 & 병합 (Review & Merge)
상대 에이전트 리뷰 → 승인 → 병합

---

## 파일 수정 전 체크 (Before Editing)

```bash
# ✅ 현재 상태 확인
git fetch origin
git status

# ✅ 변경 이력 확인
git log --oneline -5 origin/main
git log --oneline -5 origin/claude/*
git log --oneline -5 origin/codex/*

# ✅ 열린 PR 확인
# GitHub → Pull Requests

# ✅ Issue 확인
# GitHub → Issues → 관련 이슈 검사

# ✅ 다른 에이전트 작업 확인
# 같은 파일을 수정 중인가?
git diff main..origin/claude/* -- <file>
git diff main..origin/codex/* -- <file>
```

**규칙**: 같은 파일 동시 수정 ❌ → Issue에서 조율

---

## 통신 템플릿 (Communication Template)

PR/Issue 댓글에 이 구조 사용:

```markdown
### STATUS
- Agent: Claude Code
- Task: Add physics view
- Branch: claude/physics-view
- State: working

### CHANGED
- src/ui/physics-view.ts: Added Canvas renderer
- test/ui/physics-view.test.ts: Added tests

### VERIFIED
- Tests: npm test → ✅ all pass
- Lint: npm run lint → ✅ pass

### NEXT
- Need: Codex to integrate with simulation API
- Suggestion: Use data interface in docs/interfaces.md

### BLOCKERS
- None
```

---

## 물리 모델 변경 (Physics Changes)

물리 가정을 바꾸거나 추가하려면:

1. **PR에 명시**
   ```markdown
   ## Physical Changes
   - Assumption: gravity = 9.8 m/s²
   - Justification: Earth surface
   - Validation: Unit tests confirm
   ```

2. **리뷰 필수**: 상대 에이전트 리뷰 후 병합

3. **침묵의 변경 금지**: "상수 튜닝"은 물리 변경이 아님 명시

---

## 테스트 실행 (Testing)

```bash
# 전체 테스트
npm test

# 특정 파일만
npm test -- --testPathPattern="file.test.ts"

# 감시 모드
npm test -- --watch

# 커버리지 확인
npm test -- --coverage
```

결과를 PR에 포함:
```
✅ All tests pass
✅ No new warnings
```

---

## 용어 정의 (Terminology)

| 용어 | 의미 |
|------|------|
| **Issue** | 작업 정의 (GitHub에서 생성) |
| **PR** | 코드 변경 제안 (branch에서 main으로) |
| **Branch** | 독립적 작업 공간 (`claude/name` 또는 `codex/name`) |
| **Merge** | PR을 main에 적용 |
| **Rebase** | 커밋 이력 정렬 (일반적 사용 ❌) |
| **Conflict** | 같은 파일 동시 수정 → 수동 해결 필요 |

---

## 자주 묻는 질문 (FAQ)

### Q: 두 에이전트가 같은 파일을 수정하려면?
**A**: Issue에서 먼저 말하기. 옵션:
1. 순차: 한 명이 먼저, 다른 한 명이 병합 후
2. 분할: 함수 단위로 나누어 작업

### Q: 테스트가 실패하면?
**A**: 
1. 로컬에서 재현
2. PR에 "BLOCKED" 상태로 보고
3. 작성자가 수정 또는 상대 에이전트에게 요청

### Q: 물리 가정을 바꿔야 하면?
**A**: 
1. PR에서 "Physical Changes" 섹션 추가
2. 가정, 근거, 검증 명시
3. 상대 에이전트 리뷰 필수

### Q: main을 수정하려면?
**A**: ❌ 절대 금지. `claude/name` 또는 `codex/name` 브랜치 사용.

### Q: PR이 실수로 닫혔으면?
**A**: 다시 열기 또는 새 PR 생성 (GitHub "Reopen" 버튼)

---

## 유용한 명령어 (Commands)

```bash
# 현재 브랜치에서 main과의 차이 보기
git diff main...

# 커밋 목록
git log --oneline main..

# 원격 브랜치 목록
git branch -r

# 로컬과 원격 동기화
git fetch origin
git pull origin main

# 브랜치 삭제
git branch -d branch-name           # 로컬
git push origin --delete branch-name  # 원격
```

---

## 체크리스트 인쇄용 (Printable Checklist)

### 📋 작업 시작 전
- [ ] Issue 생성 및 템플릿 작성
- [ ] 담당 에이전트 확인 (Claude Code / Codex)
- [ ] 최신 main 동기화: `git fetch origin`
- [ ] 관련 파일 검사: PR/Issue 확인
- [ ] 다른 에이전트 작업 확인: 같은 파일 수정 중인지

### 📝 코드 변경 중
- [ ] 파일 수정
- [ ] 테스트 작성 또는 수정
- [ ] 로컬 테스트 통과: `npm test`
- [ ] 불필요한 파일 제외

### 🔨 커밋 전
- [ ] 커밋 메시지 작성 (what + why)
- [ ] Issue 참조: `#123`
- [ ] 최종 테스트 확인: `npm test`

### 🔍 PR 생성 전
- [ ] PR 템플릿 사용
- [ ] 모든 테스트 통과 (로컬 + CI)
- [ ] 물리 변경 있으면 명시
- [ ] 관련 Issue 링크: `Closes #123`
- [ ] 리뷰어 명시: `@other-agent`

### ✅ 리뷰 중
- [ ] 상대 에이전트 피드백 반영
- [ ] 필요시 커밋 추가
- [ ] 테스트 재실행
- [ ] 리뷰 완료 후 병합

---

## 문제 해결 (Troubleshooting)

### 브랜치 충돌 (Branch Conflict)
```bash
# 최신 main 가져오기
git fetch origin
git merge origin/main

# 충돌 해결
# (편집기에서 <<<<, ====, >>>> 표시 수정)

# 변경 계속
git add .
git commit -m "Resolve merge conflict"
git push origin <branch>
```

### 실수한 커밋
```bash
# 마지막 커밋 수정
git commit --amend

# 또는 새 커밋으로 수정
git revert HEAD
```

### PR 수정
1. 로컬에서 수정
2. 커밋 추가
3. 같은 브랜치에 푸시
4. PR이 자동 갱신됨

---

## 참고 문서 (Reference)

- **COLLABORATION.md** (이것) - 협력 워크플로우
- **docs/agent-protocol.md** - 정책과 원칙
- **AGENTS.md** - Codex 운영 규칙
- **CLAUDE.md** - Claude Code 운영 규칙
- **TASKS.md** - 작업 보드

---

**마지막 갱신**: 2026-10-04  
**피드백**: Issue 또는 PR로 제안해주세요.
