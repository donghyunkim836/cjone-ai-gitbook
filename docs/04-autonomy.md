# 04. 권한 설정

Claude Code를 쓰다 보면 파일 수정이나 명령 실행마다 "허용할까요?"라고 물어봅니다. 처음에는 안심이 되지만, 익숙해지면 매번 승인하는 게 번거롭습니다.

사람에게 일을 맡기는 것과 비슷합니다. 처음에는 "매번 확인받고 진행해"라고 하지만, 신뢰가 쌓이면 "알아서 해, 큰 건만 보고해"가 됩니다.

권한 모드는 Claude에게 줄 자율권을 조절하는 설정입니다. 상황에 따라 모드를 바꾸고, 건드리면 안 되는 파일은 미리 막아두면 됩니다.

## 권한 모드

아래로 갈수록 자율성이 높아집니다.

| 모드 | CLI 플래그 | 설명 |
|------|-----------|------|
| Plan | `plan` | 파일 수정 불가. 분석과 계획만 |
| Default | `default` | 파일 읽기는 자동, 수정/명령 실행은 매번 승인 (기본값) |
| Auto-accept Edits | `acceptEdits` | 파일 수정은 자동, 명령 실행은 승인 |
| Auto | `auto` | 백그라운드에서 안전 검사 후 자동 실행 (Team/Enterprise/API 전용. Pro/Max 불가) |
| Don't Ask | `dontAsk` | 사전 승인된 것만 허용, 나머지는 자동 거부 |
| Bypass Permissions | `bypassPermissions` | 모든 작업 자동 실행 (위험) |

처음에는 그냥 `claude`로 시작하면 Default 모드로 됩니다. 특정 모드로 시작하려면 `--permission-mode` 플래그를 씁니다.

```bash
claude --permission-mode plan
```

## 모드 전환

**Desktop 앱**: Code 탭 프롬프트 아래 모드 선택기에서 클릭으로 전환합니다.

**CLI**: `Shift+Tab`으로 자주 쓰는 3개 모드가 순환합니다.

```
Shift+Tab
```

Normal → Plan → Auto-accept 순서로 전환되며, 현재 모드는 입력창 옆에 표시됩니다.

- Default: 표시 없음
- Plan: `plan mode on`
- Auto-accept Edits: `accept edits on`
- Bypass Permissions: `bypass permissions on`

Don't Ask, Bypass Permissions는 플래그나 설정 파일로 지정합니다.

```bash
claude --permission-mode dontAsk
claude --permission-mode bypassPermissions
```

## Plan 모드 - 실행 전에 계획 먼저

바로 "해줘"라고 하면 Claude는 즉시 파일을 수정하고 명령을 실행합니다. 방향이 맞으면 빠르지만, 틀리면 전부 되돌리고 처음부터 다시 해야 합니다.

Plan 모드는 이걸 방지합니다. Claude가 현재 상태를 분석하고 어떤 순서로 무엇을 할지 정리해서 보여줍니다. 파일은 건드리지 않습니다. 계획을 보고 수정하거나 확정한 뒤에 실행하면 됩니다.

> Claude Code 창시자 Boris도 "복잡한 작업은 항상 Plan 모드로 시작한다"고 합니다. 10분 계획이 2시간 삽질을 막습니다.

`Shift+Tab`으로 Plan 모드로 전환하고 작업을 요청합니다.

```
이 폴더에 촬영 원본 영상이 10개 있어.
YouTube Shorts(9:16), 인스타 릴스(9:16), 블로그용(16:9) 세 버전으로 만들고 싶어.
파일을 확인하고 작업 순서를 정리해줘.
```

계획이 마음에 안 들면 Plan 모드 상태에서 바로 수정합니다.

```
블로그용은 빼고 Shorts랑 릴스만. 해상도는 1080x1920으로 통일해줘.
```

확정되면 `Shift+Tab`으로 Normal 또는 Auto-accept로 전환하고 실행합니다.

```
계획대로 실행해줘
```

흐름 요약:

```
Shift+Tab (Plan) → 계획 요청 → 수정 → 확정
→ Shift+Tab (Normal 또는 Auto-accept) → 실행
```

## 권한 규칙 미리 설정 (/permissions)

같은 명령에 매번 "허용"을 누르는 게 귀찮다면 `/permissions`로 규칙을 미리 잡아둘 수 있습니다.

```
/permissions
```

탭이 4개 나타납니다.

| 탭 | 역할 |
|----|------|
| Allow | 등록된 도구는 승인 없이 자동 실행 |
| Ask | 이 도구를 쓸 때마다 확인 요청 |
| Deny | 등록된 도구는 항상 차단 |
| Workspace | Claude가 접근할 수 있는 폴더 관리 |

규칙은 `도구명` 또는 `도구명(상세지정)` 형식으로 씁니다.

| 규칙 예시 | 의미 |
|-----------|------|
| `Bash(npm test)` | npm test 명령 허용 |
| `Bash(npm run *)` | npm run으로 시작하는 모든 명령 허용 |
| `Read(.env)` | .env 파일 읽기 차단 (Deny에 추가) |

규칙이 겹치면 Deny > Ask > Allow 순서로 적용됩니다. 규칙은 `.claude/settings.json`에 저장되어 팀원과 공유됩니다.

## 민감한 파일 보호

Claude는 작업 폴더 안의 파일을 읽을 수 있습니다. `.env`나 비밀번호가 담긴 파일이 있다면 미리 차단해두세요.

```
.env 파일과 secrets/ 폴더를 Claude가 읽지 못하도록 권한 설정해줘
```

Claude가 `.claude/settings.json`에 아래 규칙을 추가해줍니다.

```json
{
  "permissions": {
    "deny": [
      "Read(.env)",
      "Read(.env.*)",
      "Read(**/.env)",
      "Read(**/.env.*)",
      "Read(secrets/**)",
      "Edit(.env)",
      "Edit(.env.*)",
      "Edit(**/.env)",
      "Edit(**/.env.*)",
      "Edit(secrets/**)",
      "Bash(cat:*.env*)",
      "Bash(cat:*secrets/*)"
    ]
  }
}
```

읽기(Read), 수정(Edit), 명령(Bash) 세 가지를 모두 막아야 확실합니다.
