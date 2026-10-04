# Agent Communication Protocol

> Claude Code와 Codex가 GitHub Issues와 PRs를 통해 효과적으로 협력하는 방법

---

## 원칙 (Principles)

1. **GitHub가 중심**: 모든 통신은 Issues, PRs, 댓글에서 이루어짐
2. **비동기 작업**: 즉시 응답을 기대하지 않음 (각자 시간이 있음)
3. **명시적 요청**: "리뷰 부탁", "의견 요청" 명확히 하기
4. **컨텍스트 포함**: 배경 정보 충분히 제공

---

## 메시지 구조 (Message Structure)

### Issue 댓글 (Issue Comment)

상황 업데이트 또는 협력 요청:

```markdown
### STATUS
- Agent: Claude Code
- Task: Issue #N description
- Branch: claude/task-name
- State: [planning | working | blocked | ready]

### CHANGED
- file1.ts: specific change
- file2.test.ts: specific change

### VERIFIED
- Test command: npm test
- Result: ✅ 12 tests pass, 0 failures

### NEXT
- 다음 단계 또는 상대 에이전트의 차례
- 필요한 것: Codex feedback on API design

### BLOCKERS
- None / 문제 설명
```

**사용 시점**:
- 작업 시작할 때
- 중요한 변경 후
- 상대 에이전트의 의견이 필요할 때
- 문제 발생했을 때

---

### PR 댓글 (PR Comment)

코드 리뷰 요청 또는 응답:

**작성자 (Author) - PR 생성 시**:
```markdown
@codex - 리뷰 요청합니다

**개요**: Add physics visualization component

**주요 변경**:
- src/ui/physics-view.ts: Canvas rendering engine
- test/ui/physics-view.test.ts: 45 tests

**테스트**:
```bash
npm test -- --testPathPattern="physics-view"
# ✅ All 45 tests pass
```

**의견 요청**:
1. Canvas vs WebGL - 성능 괜찮은지 확인 요청
2. Physics API integration - 맞게 했는지 확인 요청

**물리 변경**: 없음

---
```

**리뷰어 (Reviewer) - PR 리뷰 후**:
```markdown
### Review Summary

**✅ 좋은 점**:
- Tests are comprehensive
- Code is clean and readable
- Physics integration follows protocol

**⚠️ 개선 필요**:
1. Line 45: Use `const` instead of `let`
2. Recommend: Add error handling for canvas initialization

**❌ 중요한 문제**:
(없으면 생략)

**승인**: Approved ✅

---
```

---

## 메시지 타입 (Message Types)

### 1. 작업 시작 알림 (Task Start)

**목적**: 다른 에이전트에게 어떤 작업을 시작했는지 알림

**위치**: Issue 댓글

**예**:
```markdown
### STATUS
- Agent: Claude Code
- Task: #123 - Add physics visualization
- Branch: claude/physics-view
- State: planning

### NEXT
- Codex feedback needed on simulation API
- 어떤 데이터 구조를 사용할지 조율 필요

/cc @codex
```

---

### 2. 진행 상황 보고 (Progress Update)

**목적**: 진행 상황을 주기적으로 공유

**위치**: Issue 댓글 (큰 진전이 있을 때마다)

**예**:
```markdown
### STATUS
- State: working
- Progress: 60% complete

### VERIFIED
- npm test → ✅ 12/15 tests pass
- 3 tests fail (expected, implementing feature)

### NEXT
- Integrate with physics API (waiting for Codex PR #456)
- ETA: 2 days
```

---

### 3. 협력 요청 (Collaboration Request)

**목적**: 상대 에이전트의 도움이나 의견 필요

**위치**: Issue 댓글

**예**:
```markdown
### 협력 필요

파일 `src/simulation/physics.ts`을 수정해야 하는데, 
Codex가 담당 영역입니다.

**요청**:
- 현재 physics.ts API가 viewer data를 지원하는가?
- 아니면 새로운 export function이 필요한가?

**제안**:
```typescript
export function getParticleState(): ParticleData[] {
  // ...
}
```

이게 맞는지 피드백 부탁합니다.

/cc @codex
```

---

### 4. 문제 보고 (Issue Report)

**목적**: 버그, 테스트 실패, 충돌 등 문제 보고

**위치**: Issue 댓글

**예**:
```markdown
### BLOCKED

테스트 실패: `src/physics.test.ts` line 45

**에러**:
```
AssertionError: Expected 9.8, got undefined
at getGravity() physics.test.ts:45
```

**원인 추측**:
getGravity() API가 아직 구현되지 않았을 가능성

**필요한 것**:
- Codex: getGravity() 구현 또는 Timeline 알려주기

/cc @codex
```

---

### 5. 리뷰 완료 (Review Complete)

**목적**: PR 리뷰 완료 및 승인/피드백

**위치**: PR Review 또는 PR 댓글

**예**:
```markdown
### Review Complete

✅ **Approved**

**변경 사항 요약**:
- 15개 파일 수정
- 120 lines added, 30 lines removed
- 모든 테스트 통과

**결정사항**:
1. Physics assumptions ✅ 변경 없음
2. Architecture ✅ 일관성 있음
3. Tests ✅ 충분함

**마이너 피드백**:
- Line 67: typo "lenght" → "length"
- Consider: error handling in initialization

이러한 사소한 점들은 작성자가 수정하면 됩니다.

---
```

---

## 응답 시간 (Response Time)

| 상황 | 예상 응답 시간 | 우선도 |
|------|----------------|--------|
| BLOCKED (즉시 대응 필요) | 1-2 시간 | 🔴 높음 |
| Collaboration request | 4-12 시간 | 🟡 중간 |
| PR review | 12-24 시간 | 🟡 중간 |
| Progress update | 24 시간 내 | 🟢 낮음 |
| FYI (정보 공유) | 필요 없음 | ℹ️ 참고 |

**규칙**: 긴급하면 "BLOCKED" 마크하고 `/cc @agent` 사용

---

## 에스컬레이션 (Escalation)

상대 에이전트와 합의 불가능할 때:

1. **Issue에서 논의** (최소 2-3 댓글)
2. **합의 불가능** → 호스트(당신)에게 `/cc @host`
3. **호스트의 결정** 대기
4. **결정 기록** → Issue에 결과 기록

예:
```markdown
### 설계 결정 필요

이 두 접근법 중 어느 것이 나은지?

**Option A (Codex 제안)**:
- 장점: 물리 정확도 높음
- 단점: 성능 10% 낮음

**Option B (Claude Code 제안)**:
- 장점: 성능 우수
- 단점: 근사화로 인한 오차 0.1%

**호스트 결정 요청**:
/cc @host
```

---

## 좋은 통신 vs 나쁜 통신 (Good vs Bad)

### ❌ 나쁜 예시

```markdown
"Tests fail lol"
```

문제점:
- 컨텍스트 부족
- 에러 메시지 없음
- 어디서 실패했는지 불명확
- 다음 단계 불명확

---

### ✅ 좋은 예시

```markdown
### BLOCKED

Test failure in src/physics.test.ts

**에러**:
```
TypeError: Cannot read property 'velocity' of undefined
at tests/physics.test.ts:87
```

**재현 방법**:
```bash
npm test -- --testPathPattern="physics.test"
```

**영향**:
- 이 버그로 인해 physics-view 구현 차단됨
- 48시간 지연 예상

**필요한 것**:
- Codex: ParticleState 타입 확인
- 또는: 임시 mock 제공

ETA 알려주실 수 있으신가요?

/cc @codex
```

문제점:
- 명확한 에러 메시지
- 재현 방법 포함
- 영향 설명
- 구체적인 요청
- 리뷰어 명시

---

## 금지 사항 (Do Not)

❌ **하지 말 것**:
1. 채팅/Slack에서 기술적 결정 내리기 → Issue 댓글에서
2. 상대 에이전트의 브랜치 수정하기 → PR로 요청
3. 주요 변경 후 설명 없이 푸시하기 → 반드시 Status 댓글
4. 물리 가정 변경을 몰래 하기 → PR에서 명시
5. "나중에 말하겠다"고 남기고 푸시하기 → 지금 말하기

---

## 예시 시나리오 (Example Scenario)

### 시나리오: 새 기능 구현

**Step 1: Claude Code 시작**
```markdown
# Issue #5: Add physics visualization panel

@codex - 이 작업 시작합니다.

### STATUS
- Agent: Claude Code
- Task: Physics visualization
- Branch: claude/physics-view
- State: planning

### NEXT
- Codex review on API design
- Need: How to query particle state from simulation?
```

---

**Step 2: Codex 응답**
```markdown
@claude-code - 좋습니다!

API 제안:
```typescript
export interface ParticleState {
  position: Vec3;
  velocity: Vec3;
  mass: number;
  id: string;
}

export function getParticles(): ParticleState[] {
  // ...
}
```

PR #456에서 이것을 구현하고 있습니다. 
Main merge될 때까지 기다려야 할까요?
```

---

**Step 3: Claude Code 진행 보고**
```markdown
@codex - 감사합니다!

### STATUS
- State: working (60% complete)
- Waiting for: PR #456 merge

### CHANGED
- src/ui/physics-view.ts: Canvas renderer
- test/ui/physics-view.test.ts: 12 tests

### VERIFIED
- Tests: ✅ 12/12 pass (mocked API)

### NEXT
- 기다리기: PR #456 merge
- 그 후: Real API와 통합

아, PR #456은 내일쯤 merge될 예정인가요?
```

---

**Step 4: Codex 업데이트**
```markdown
@claude-code - PR #456 merged!

원본을 rebase하고 실제 API로 통합하면 됩니다.

진행하세요!
```

---

**Step 5: Claude Code PR 생성**
```markdown
@codex - [Claude] Add physics visualization

### CHANGED
- src/ui/physics-view.ts: Canvas component
- test/ui/physics-view.test.ts: 45 tests

### VERIFIED
```bash
npm test -- --testPathPattern="physics-view"
# ✅ All 45 tests pass
npm run lint
# ✅ No errors
```

### NEXT
- Codex review on physics integration
- Physics assumptions: None changed

/cc @codex - 리뷰 부탁합니다
```

---

**Step 6: Codex 리뷰**
```markdown
### Review: ✅ Approved

**좋은 점**:
- Tests comprehensive
- Canvas rendering efficient
- Physics API integration correct

**마이너**:
- Line 67: Add error handling for missing gravity

approve 합니다!
```

---

**Step 7: 병합 및 마무리**
```markdown
### STATUS
- State: ready
- Merged: main

다음: TASKS.md 갱신하고 새 작업 시작
```

---

## 체크리스트 (Checklist for Good Communication)

댓글을 쓸 때마다 확인:

- [ ] 상대 에이전트 명시 (`@codex` 또는 `@claude-code`)
- [ ] STATUS 섹션 포함 (필요시)
- [ ] 구체적인 파일/줄 번호 제시 (문제 보고 시)
- [ ] 테스트 결과 포함 (변경 보고 시)
- [ ] 다음 단계 명확히 (요청 시)
- [ ] 긴급도 표시 (BLOCKED, URGENT 등)
- [ ] 불필요한 감정 표현 제거

---

## 추가 팁 (Tips)

### 효율적 리뷰

리뷰 요청할 때:
```markdown
@codex - 다음을 리뷰해주세요:

- **Code**: PR 변경사항 (자동 보임)
- **Tests**: npm test 결과 첨부
- **Architecture**: API 설계가 맞는지 확인
- **Physics**: 물리 가정이 맞는지 검증

특히 Line 45의 friction coefficient 결정이 맞는지 봐주세요.
```

### 빠른 피드백

작은 변경:
```markdown
### Quick Question

이 코드가 맞나요?

\`\`\`typescript
function getGravity(): number {
  return 9.8;  // Earth only?
}
\`\`\`

Codex feedback needed: 다른 행성도 지원해야 하나요?

@codex
```

### 승인 전 확인

승인하기 전:
```markdown
### Pre-approval checklist

- [ ] Tests pass: npm test
- [ ] Lint pass: npm run lint
- [ ] Physics correct: Review equation
- [ ] No merge conflicts
- [ ] PR linked to Issue

All ✅ → Approve
```

---

## 트러블슈팅 (Troubleshooting)

### 상대 에이전트가 응답 안 함

1. 2-3시간 기다리기 (비동기)
2. `/cc @agent` 멘션 추가
3. "URGENT" 또는 "BLOCKED" 표시
4. 호스트에게 `/cc @host` 알림

### 충돌

충돌 발생하면:
```markdown
### COLLISION DETECTED

파일 `src/physics.ts` 동시 수정:
- Claude Code: PR #123 (working)
- Codex: PR #125 (working)

**해결 방법**:
1. 누가 먼저 merge?
2. 다른 한 명이 rebase?

@codex - 의견 부탁합니다
```

---

**마지막 갱신**: 2026-10-04  
**질문**: Issue로 제안해주세요
