---
name: study-chapter-pr
description: >-
  Commits a study chapter note and opens a pull request for this repository.
  Use whenever the user asks to commit a chapter summary, push study notes,
  create a branch like N장/이름, or open a PR for 챕터/장 정리 — even if they
  only say "올려줘", "PR 만들어줘", or mention a file under N장/*.md.
---

# Study Chapter PR

이 저장소의 스터디 챕터 정리 파일을 브랜치에 커밋하고 `main`으로 PR을 연다.

## When to use

- `N장/<이름>.md` 정리본을 커밋·푸시·PR 할 때
- 브랜치명 `N장/<이름>` 패턴으로 올리라고 할 때
- 챕터 노트 / 스터디 정리 PR 관련 요청일 때

## Preconditions

- `gh`로 이 저장소에 접근 가능해야 한다.
- 커밋은 사용자가 명시적으로 요청했을 때만 만든다.
- `.omc/`, `.DS_Store` 등 스터디 노트와 무관한 파일은 절대 스테이징하지 않는다.

## Workflow

### 1. 입력 파악

대상 경로에서 장 번호와 작성자를 추출한다.

- 경로 패턴: `N장/<이름>.md` (예: `1장/오혜성.md` → 장=`1`, 이름=`오혜성`)
- 파일이 없거나 장/이름을 알 수 없으면 사용자에게 묻는다.
- 챕터 제목은 저장소 루트 `README.md` 일정/목차에서 해당 장을 찾는다.

### 2. 브랜치

`main`이 최신인지 확인한 뒤 브랜치를 만든다.

```bash
git checkout main
git pull --ff-only origin main
git checkout -b "N장/<이름>"
```

이미 같은 이름의 로컬/원격 브랜치가 있으면 그 브랜치를 재사용하고, 충돌·분기 상태는 사용자에게 알린다.

### 3. 스테이징과 커밋

해당 노트 파일만 추가한다.

```bash
git add "N장/<이름>.md"
git commit -m "$(cat <<'EOF'
N장 <이름> 정리 추가

EOF
)"
```

커밋 메시지는 짧은 한글 요약으로 맞춘다 (예: `1장 오혜성 정리 추가`).

### 4. 푸시

```bash
git push -u origin HEAD
```

네트워크/인증이 필요하면 적절한 권한으로 실행한다.

### 5. PR 생성

제목 규칙은 `.github/pull_request_template.md`를 따른다.

```text
[이름] 챕터 n: 챕터 이름
```

예: `[오혜성] 챕터 1: 프로그래머에서 이제는 엔지니어로`

```bash
gh pr create --base main --title "[이름] 챕터 n: 챕터 이름" --body "$(cat <<'EOF'
<!--
PR 제목을 확인해 주세요!
`[이름] 챕터 n: 챕터 이름`
ex. [오혜성] 챕터 1: 리액트 개발을 위해 꼭 알아야 할 자바스크립트
 -->

## Summary
- N장 독서 정리 파일 `N장/<이름>.md` 추가

EOF
)"
```

### 6. 완료 보고

PR URL을 사용자에게 반환한다. PR에 노트 md 추가분만 포함됐는지 확인한다.

## Done criteria

- 원격에 `N장/<이름>` 브랜치가 있다.
- `main` ← `N장/<이름>` PR이 열려 있고 URL을 전달했다.
- 커밋/PR에 스터디 노트 파일만 포함됐다.
