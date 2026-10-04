# Work in Progress (WIP) Tracker

> 실시간 작업 현황 추적 (각 에이전트가 무엇을 하고 있는지)

**마지막 갱신**: 2026-10-04

---

## Claude Code

### 현재 작업
_None_

**담당 영역**:
- UI/Presentation
- Testing & QA
- Debugging
- Developer tooling
- Documentation

**할 준비 완료된 작업**:
- 협력 환경 구축 (완료 예정)

---

## Codex (ChatGPT)

### 현재 작업
_None_

**담당 영역**:
- Physics & Mathematics
- Simulation architecture
- Public interfaces
- Integration
- Model correctness

**할 준비 완료된 작업**:
- 초기 물리 모델 아키텍처 정의

---

## 계획된 작업 (Planned)

### Phase 1: 기초 (Foundation)
- [ ] **[Claude] WIP Dashboard 구축** → CLAUDE (UI 도구)
- [ ] **[Codex] 물리 모델 아키텍처 정의** → CODEX (아키텍처)
- [ ] **[Claude] 첫 번째 단위 테스트 프레임워크** → CLAUDE (테스트)
- [ ] **[Codex] 시뮬레이션 상태 모델** → CODEX (데이터 구조)

### Phase 2: 시뮬레이션 (Simulation)
- [ ] **[Codex] 기본 물리 엔진** (입자, 중력)
- [ ] **[Claude] 물리 엔진 테스트 스위트**
- [ ] **[Codex] 충돌 감지** (sphere-sphere)
- [ ] **[Claude] 충돌 테스트**

### Phase 3: 시각화 (Visualization)
- [ ] **[Claude] Canvas 렌더링 시스템**
- [ ] **[Claude] 입자 시각화**
- [ ] **[Claude] 벡터 필드 표시**
- [ ] **[Claude] 실시간 정보 표시판**

---

## 도메인별 파일 현황

| 파일 | 담당 | 상태 | 마지막 변경 |
|------|------|------|-----------|
| `src/simulation/physics.ts` | Codex | 🔴 미생성 | - |
| `src/simulation/types.ts` | Codex | 🔴 미생성 | - |
| `src/ui/canvas-renderer.ts` | Claude | 🔴 미생성 | - |
| `test/simulation.test.ts` | Claude | 🔴 미생성 | - |
| `docs/api.md` | Codex | 🔴 미생성 | - |

**범례**:
- 🟢 활성 개발 (active)
- 🟡 대기 중 (blocked)
- 🔴 미생성 (not started)
- ⚫ 완료 (done)

---

## 브랜치 현황

| 브랜치 | 담당 | 작업 | PR | 상태 |
|--------|------|------|----|----|
| `main` | - | 베이스 | - | ⚫ 안정 |
| `ccr-f4e17aa8-9q52p1` | Claude | 협력 환경 | TBD | 🟢 활성 |

---

## 의존성 그래프

```
Phase 1
├── CLAUDE: WIP Dashboard
├── CODEX: Physics architecture
├── CLAUDE: Test framework
└── CODEX: Simulation data model
    ↓
Phase 2 (Phase 1 완료 후)
├── CODEX: Basic physics engine
├── CLAUDE: Physics tests
└── CODEX: Collision detection
    ↓
Phase 3 (Phase 2 완료 후)
├── CLAUDE: Canvas renderer
├── CLAUDE: Particle visualization
└── CLAUDE: Info display
```

---

## 이슈 링크

### Codex 이슈
- (없음)

### Claude Code 이슈
- (없음)

### 협력 이슈
- [협력 환경 구축](협력 환경은 이 파일)

---

## 블로커 & 위험 요소

| 이슈 | 심각도 | 상태 | 해결 |
|------|--------|------|------|
| 없음 | - | ✅ 클리어 | - |

---

## 최근 변경사항

### 2026-10-04
- ✅ 협력 환경 구축 완료
  - COLLABORATION.md 작성
  - PR/Issue 템플릿 생성
  - 빠른 참고서 작성
  - 통신 프로토콜 정의

---

## 추적 규칙

**언제 갱신**:
- 새 작업 시작: 상태 추가
- 작업 완료: 상태 변경 (🔴 → 🟢 → ⚫)
- PR 열림: 링크 추가
- 블로커 발생: 이슈 섹션 갱신

**형식**:
```markdown
### YYYY-MM-DD
- [에이전트] 작업 설명
  - 상세: 무엇이 변했나
  - 영향: 다른 작업에 어떤 영향
  - 다음: 다음 단계
```

**예**:
```markdown
### 2026-10-05
- ✅ [CODEX] Physics architecture defined
  - 상세: Particle, Force, Collision types defined
  - 영향: Claude can now write tests
  - 다음: CODEX starts basic physics engine
```

---

## 빠른 통계

```
총 계획 작업: 13개
완료: 1개 (7%)
진행 중: 0개
대기 중: 0개
계획: 12개
```

---

## 호스트 체크리스트

- [ ] 매주 이 파일 검토
- [ ] 블로커 있으면 즉시 대응
- [ ] 3주마다 Phase 평가
- [ ] 분기마다 계획 갱신

---

**갱신 담당**: 각 에이전트가 작업 완료 시 즉시 갱신
