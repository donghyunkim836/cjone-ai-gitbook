# 04. 자율권과 안전

Claude Code를 쓰다 보면 파일 수정이나 명령 실행마다 "허용할까요?"라고 물어봅니다. 처음에는 안심이 되지만, 익숙해지면 매번 승인하는 것이 번거로워집니다.

사람에게 일을 맡길 때와 같습니다. 신입에게는 "매번 확인받고 진행해"라고 하지만, 신뢰가 쌓이면 "알아서 해, 대신 큰 건만 보고해"라고 합니다.

권한 모드는 Claude에게 주는 이 자율권을 조절하는 장치입니다. 처음에는 신입처럼 매번 확인받게 하고, Claude와 신뢰가 생기면 자율권을 올려주면 됩니다. 동시에 위험한 행동은 아예 봉쇄해두는 것도 중요합니다.

이 챕터에서는 자율권을 조절하는 권한 모드, 실행 전 계획을 세우는 Plan Mode, 그리고 민감 정보를 보호하는 규칙 설정을 다룹니다.

## 권한 모드

위에서 아래로 갈수록 자율성이 높아집니다.

| 모드 | CLI 플래그 | 설명 |
|------|-----------|------|
| Plan | `plan` | 코드 수정 불가, 분석과 계획만 수행 |
| Default | `default` | 파일 읽기는 자동, 수정/명령 실행은 승인 필요 (기본값) |
| Auto-accept Edits | `acceptEdits` | 파일 수정은 자동 허용, 명령 실행은 승인 필요 |
| Auto | `auto` | 분류기가 백그라운드에서 안전 검사. 프롬프트 피로 없이 작업 (리서치 프리뷰, Team/Enterprise/API 플랜. Pro/Max 불가. Sonnet 4.6/Opus 4.6 필요) |
| Don't Ask | `dontAsk` | `/permissions`이나 설정 파일에서 사전 승인된 것만 허용, 나머지는 자동 거부 |
| Bypass Permissions | `bypassPermissions` | 모든 작업을 자동 실행 (위험) |

처음에는 그냥 `claude`로 시작하면 됩니다. Default 모드로 시작됩니다. 특정 모드로 시작하고 싶을 때 `--permission-mode` 플래그를 사용합니다.

```bash
# Default 모드로 시작 (기본)
claude

# 특정 모드로 시작
claude --permission-mode plan
```

## 모드 전환

**Desktop 앱**: Code 탭 프롬프트 아래의 모드 선택기에서 클릭으로 전환합니다. Ask permissions, Auto accept edits, Plan mode, Bypass permissions 4가지를 선택할 수 있습니다.

**CLI**: `Shift+Tab`으로 일상적으로 쓰는 3개 모드가 순환합니다.

```
Shift+Tab
```

Normal(Default) → Plan → Auto-accept 순서로 전환됩니다. 현재 모드는 입력창 옆에 표시됩니다.

- Default: 아무 표시 없음
- Plan: `plan mode on`
- Auto-accept Edits: `accept edits on`
- Bypass Permissions: `bypass permissions on`

나머지 2개(Don't Ask, Bypass Permissions)는 CLI 플래그나 설정 파일로 지정합니다.

```bash
claude --permission-mode dontAsk
claude --permission-mode bypassPermissions
```

`--dangerously-skip-permissions`는 `--permission-mode bypassPermissions`와 동일한 단축 플래그입니다. 이름에 "dangerously"를 붙여 위험성을 강조한 것입니다.

## Plan Mode - 실행 전에 계획

AI에게 바로 "해줘"라고 시키면 Claude는 즉시 작업을 시작합니다. 파일을 수정하고, 코드를 작성하고, 명령을 실행합니다. 방향이 맞으면 빠르지만, 방향이 틀리면 전부 되돌리고 처음부터 다시 해야 합니다.

Plan 모드는 이 문제를 해결합니다. "먼저 계획을 세워줘"라고 한 마디 하는 것과 같습니다. Claude가 현재 상태를 분석하고, 어떤 순서로 무엇을 할지 정리해서 보여줍니다. 파일은 건드리지 않습니다. 계획을 검토하고, 수정하고, 확정한 뒤에 실행하면 됩니다.

> AI와의 작업에서 가장 큰 비용은 코딩 시간이 아니라 방향 수정 시간입니다. 10분 동안 계획을 잡으면 2시간짜리 삽질을 피할 수 있습니다. Claude Code 창시자 Boris도 "복잡한 작업은 항상 Plan 모드로 시작한다"고 합니다.

`Shift+Tab`으로 Plan 모드로 전환한 뒤 작업을 요청합니다.

```
이 폴더에 촬영 원본 영상이 10개 있어.
각각 YouTube Shorts(9:16), 인스타 릴스(9:16), 블로그용(16:9) 세 버전으로 만들고 싶어.
파일을 확인하고 작업 계획을 세워줘.
```

Claude가 파일 목록을 분석하고, 변환 순서와 방법을 정리합니다. 이때 파일은 건드리지 않습니다.

계획이 마음에 들지 않으면 Plan 모드 상태에서 바로 피드백합니다.

```
블로그용은 빼고, Shorts와 릴스만. 해상도는 1080x1920으로 통일해줘.
```

계획이 확정되면 `Shift+Tab`으로 Normal 또는 Auto-accept 모드로 전환하여 실행합니다.

```
계획대로 실행해줘
```

전체 흐름:

```
Shift+Tab (Plan) → 계획 요청 → 피드백 → 계획 확정
→ Shift+Tab (Normal 또는 Auto-accept) → "계획대로 실행해줘"
```

| 상황 | 권장 모드 |
|------|-----------|
| 여러 파일에 걸친 변경, 방향 결정이 필요한 작업 | Plan 먼저 |
| 오타 수정, 한 줄 변경, 간단한 질문 | Normal로 바로 |

## 권한 규칙 설정 (/permissions)

매번 같은 명령에 "허용"을 누르는 것이 번거롭다면, `/permissions`로 규칙을 미리 설정할 수 있습니다.

```
/permissions
```

4개의 탭이 나타납니다.

| 탭 | 역할 |
|----|------|
| Allow | 허용된 도구는 승인 없이 자동 실행 |
| Ask | 이 도구를 쓸 때마다 항상 확인 요청 |
| Deny | 거부된 도구는 항상 차단 |
| Workspace | Claude가 접근할 수 있는 작업 디렉토리 관리 |

규칙은 `도구명` 또는 `도구명(상세지정)` 형식으로 작성합니다.

| 규칙 예시 | 의미 |
|-----------|------|
| `Bash(npm test)` | npm test 명령을 허용 |
| `Bash(npm run *)` | npm run으로 시작하는 모든 명령을 허용 |
| `Read(.env)` | .env 파일 읽기를 차단 (Deny에 추가) |

규칙이 겹치면 Deny > Ask > Allow 순서로 우선 적용됩니다. Deny에 넣으면 Allow에 있어도 차단됩니다. 규칙은 `.claude/settings.json`에 저장되어 팀원과 공유할 수도 있습니다.

## 민감 정보 보호

Claude는 작업 폴더 내의 파일을 읽을 수 있습니다. `.env` 파일이나 비밀번호가 포함된 파일이 있다면 접근을 차단합니다. Claude에게 부탁하면 됩니다.

```
.env 파일과 secrets/ 폴더를 Claude가 읽지 못하도록 권한 설정해줘
```

Claude가 `.claude/settings.json`에 다음과 같은 deny 규칙을 추가합니다.

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

읽기(Read), 수정(Edit), 셸 명령(Bash)까지 모두 차단해야 확실합니다.

> Deny 규칙은 Claude의 시도 자체를 차단하는 1차 방어선입니다. 공식 문서에서도 이것을 1차 방어선으로 설명하고, 추가로 sandbox와 hooks를 "defense-in-depth"(다층 방어)로 권장합니다.
