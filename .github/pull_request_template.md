# [Agent] Short description

## 변경사항
<!-- 이 PR이 무엇을 하는가? 정확하고 명확하게 -->

## 관련 Issue
Closes #<!-- Issue 번호 -->

## 변경된 파일
<!-- 수정된 파일 목록 (선택: `git diff --name-only main...` 실행) -->
- `src/module/file.ts`
- `test/file.test.ts`

## 테스트 & 검증

### 로컬 테스트
```bash
npm test

# 또는 특정 테스트만
npm test -- --testPathPattern="file.test.ts"
```

### 결과
<!-- 테스트 결과 복붙 -->
```
✅ All tests pass
✅ No linting errors
✅ Physical assumptions validated
```

### 수동 확인
<!-- UI 변경이 있으면: 스크린샷 또는 작동 설명 -->

## 물리적 정확성

- [ ] 물리 모델 변경 없음
- [ ] 새로운 물리 가정 추가:
  ```
  가정: ...
  근거: ...
  검증: ...
  ```
- [ ] 기존 물리 동작 유지

## 설계 결정

<!-- 왜 이렇게 구현했나? -->
- 결정 1: 이유
- 결정 2: 이유

## 알려진 제약

- 제약 1
- 제약 2

## 추가 정보

<!-- 스크린샷, 성능 비교, 의견 요청 등 -->

---

## 리뷰 요청

<!-- 리뷰어 언급 -->
@other-agent - 리뷰 요청

## 체크리스트

- [ ] PR 템플릿 사용
- [ ] Issue 링크 (`Closes #123`)
- [ ] 모든 테스트 통과
- [ ] 커밋 메시지 명확
- [ ] 물리 가정 명시
- [ ] 리뷰어 명시
