# 고덕1구역 홍보 사이트 — 구축 및 운용 매뉴얼

최종 갱신 2026년 10월 30일

## 1. 현재 운영 구조

```
방문자
  ↓
godeok1.netlify.app
  ↓  (Netlify에는 아래 로더만 고정 배포)
Real_estates/main/godeok1/index.html
  ↓  브라우저가 실행 시 직접 읽음
GitHub Issue #1의 godeok1-live-part 댓글 1~3
  ↓
실제 최신 HTML 화면
```

### 핵심 원칙

- Netlify에는 **로더 HTML만 고정**해 둡니다.
- 실제 사이트 전체 HTML은 GitHub 저장소의 **Issue #1 댓글 3개**에 나누어 저장합니다.
- 이후 문구·숫자·CSS·JS·레이아웃 등 일반적인 사이트 수정은 **Issue 댓글만 수정**합니다.
- Issue 댓글 수정은 Git commit/push가 아니므로 **Netlify 자동배포를 발생시키지 않습니다.**
- 따라서 로더 자체를 바꾸지 않는 한 일상적인 사이트 수정에는 Netlify Production Deploy가 필요 없습니다.
- `main` 브랜치의 `godeok1/index.html`과 `godeok1/godeok1-owners.html`은 앞으로 **일상 수정 대상이 아닙니다.**

## 2. 사이트 주소와 고정 파일

| 항목 | 값 |
|---|---|
| 사이트 | `https://godeok1.netlify.app/` |
| GitHub 저장소 | `comfunnylj/Real_estates` |
| Netlify 배포 브랜치 | `main` |
| Netlify Publish directory | `godeok1` |
| 고정 로더 | `godeok1/index.html` |
| 동일 로더 사본 | `godeok1/godeok1-owners.html` |
| 라이브 HTML 저장소 | GitHub Issue #1 댓글 |
| 라이브 파트 | `godeok1-live-part:1` ~ `3` |

## 3. 일상적인 사이트 수정 방법 — Netlify 재배포 없음

1. GitHub Issue #1의 라이브 HTML 댓글 3개를 읽습니다.
2. 댓글 3개에서 마커 줄을 제외한 HTML을 순서대로 합칩니다.
3. 전체 HTML을 수정합니다.
4. 수정 완료본을 다시 3개 파트로 나눕니다.
5. 기존 댓글 ID에 `update_issue_comment`로 덮어씁니다.
6. 방문자가 사이트를 새로 열거나 새로고침하면 브라우저가 최신 댓글을 직접 읽습니다.

현재 라이브 댓글 ID:
- part 1: `6519863230`
- part 2: `6519864063`
- part 3: `6519864853`

### 절대로 하지 말 것

- 일반 본문 수정 때문에 `main/godeok1/index.html`을 커밋하지 않습니다.
- 일반 본문 수정 때문에 Netlify에서 Deploy를 다시 실행하지 않습니다.
- 라이브 HTML 댓글을 삭제하거나 순서를 바꾸지 않습니다.
- 댓글 맨 첫 줄의 `<!-- godeok1-live-part:N -->` 마커를 삭제하지 않습니다.

## 4. Netlify 배포가 필요한 예외

다음 경우에만 Netlify 로더 자체를 수정해야 하므로 Production Deploy가 필요합니다.

- GitHub API 주소 또는 라이브 저장 위치를 바꾸는 경우
- 로더의 장애 처리 방식 자체를 바꾸는 경우
- OG 메타태그(카카오톡·SNS 공유 미리보기)를 바꾸는 경우
- Netlify 도메인·사이트 자체 설정을 바꾸는 경우

사이트 본문·숫자·표·계산기·CSS·JavaScript·접이식 구조 등은 라이브 HTML 안에 있으므로 위 예외가 아닙니다.

## 5. 라이브 HTML 로딩 방식

브라우저는 아래 GitHub API를 직접 호출합니다.

```
https://api.github.com/repos/comfunnylj/Real_estates/issues/1/comments
```

그중 마커가 다음 형식인 댓글만 사용합니다.

```
<!-- godeok1-live-part:1 -->
<!-- godeok1-live-part:2 -->
<!-- godeok1-live-part:3 -->
```

번호 순서대로 합친 뒤 전체 HTML 문서로 표시합니다.

GitHub API가 일시적으로 실패하면 브라우저에 저장된 직전 정상본(localStorage)이 있으면 그 본문을 대신 표시합니다.

## 6. 최신 사업수지 기준

사이트 수치는 반드시 Google Drive
`부동산 > 고덕 1구역 최종본 자료 모음`
폴더의 최신 Final/제출용 자료를 원본으로 사용합니다.

현재 Rev.3 핵심값:
- 총 세대수 2,701세대
- 적용 용적률 264.42%
- 일반분양 1,107세대
- 조합원 배정 1,137세대
- 총매출 36,453.6억원
- 총사업비 19,690.2억원
- 사업이익 16,763.5억원
- 종전자산 13,662.0억원
- 추정 비례율 122.7%
- 총연면적 가중평균 공사비 1,050만원/평

다른 시나리오 숫자를 일괄 치환하지 않습니다.

## 7. 장애 확인

| 증상 | 확인 |
|---|---|
| 오래된 본문이 보임 | 강력 새로고침 또는 브라우저 캐시 확인 |
| 최신 본문 로딩 실패 | GitHub Issue #1 댓글 존재 여부와 마커 확인 |
| 일부 본문이 잘림 | part 1~3 댓글이 모두 존재하는지 확인 |
| 사이트 전체가 로딩 화면에서 멈춤 | GitHub API 응답 또는 로더 스크립트 확인 |
| SNS 공유 제목/설명이 옛날 값 | OG 메타는 로더에 있으므로 Netlify 재배포가 필요한 예외 |

## 8. 백업

- 라이브 HTML: GitHub Issue #1 댓글 수정 이력
- 로더: GitHub `main` 커밋 이력
- 사업수지 원본: Google Drive `고덕 1구역 최종본 자료 모음`

## 9. 변경 이력

- 2026-10-18: GitHub → Netlify 자동배포 구조 도입
- 2026-10-30: 일상 수정 시 Netlify Production Deploy가 발생하지 않도록 **고정 로더 + GitHub Issue 라이브 HTML 구조**로 전환
