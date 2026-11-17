# 예약 작업 프롬프트 (claude-task-sync)

예약 작업 「Claude 작업 관리 동기화·일일 보고 (Notion)」(평일 06:00 KST)의 프롬프트 칸에 아래 블록을 그대로 붙여 넣음.
규칙은 모두 `claude-task-sync.md`(지시서)에 있고, 이 프롬프트는 지시서를 찾아 읽는 순서만 정함.

```
지시서 파일을 읽고 그대로 실행한다. 이 프롬프트에는 규칙을 두지 않으며, 규칙은 모두 지시서에 있음.

[지시서 위치] 저장소 justemhan-collab/plugins 의 justem-routines/claude-task-sync.md

[읽는 순서] 아래 순서로 시도해 처음 성공한 내용을 사용함. 단, 문서 버전이 v2.0 미만이면 다음 순서로 넘어감.
1. git fetch origin main claude/intelligent-fermi-qfipjz claude/cool-maxwell-yu4r9w (실패해도 계속)
2. git show origin/main:justem-routines/claude-task-sync.md
3. git show origin/claude/intelligent-fermi-qfipjz:justem-routines/claude-task-sync.md
4. git show origin/claude/cool-maxwell-yu4r9w:justem-routines/claude-task-sync.md
5. 작업 디렉터리의 justem-routines/claude-task-sync.md

[실행]
- 지시서 전체를 끝까지 읽은 뒤 0~6장 절차를 그대로 수행함. 지시서와 이 프롬프트가 다르면 지시서를 따름.
- 대상은 Notion 「Claude 작업관리」와 「Han 업무 관리」이며, 0.bak 페이지 아래의 옛 DB는 읽지도 쓰지도 않음.
- 마지막 응답 끝에 읽은 경로와 지시서 버전을 적음.
- 확인 필요·실패 사항은 지시서 6장에 따라 카카오톡 나에게 보내기로도 보고함.

[지시서를 못 읽은 경우] Notion·GitHub 등 어디에도 쓰지 말고(카카오톡 나에게 보내기로 실패 1건만 보고), 마지막 응답 첫 줄에 "지시서 읽기 실패"와 시도한 경로·오류를 남기고 종료함. 읽은 지시서가 모두 v2.0 미만이면 "지시서 버전 낮음(v1.x)"으로 같은 방식으로 종료함.
```

## 변경 이력

| 날짜 | 내용 |
|---|---|
| 2026-11-17 | 카카오톡 보고(지시서 v2.1 6장) 문구 추가 |
| 2026-11-16 | v2.0 지시서용으로 수정: 작업 브랜치 claude/intelligent-fermi-qfipjz를 읽는 순서에 추가, v2.0 미만 지시서(옛 DB 기준)는 건너뜀, 0.bak 옛 DB 제외 명시 |
