# 01. 설치

이 챕터를 마치면 Claude Code를 설치하고, 첫 대화를 나눌 수 있습니다.

## 설치 환경

Claude Code는 여러 환경에서 사용할 수 있습니다. 어떤 환경이든 동일한 AI 엔진이 작동하고, 설정도 공유됩니다.

| 환경 | 설명 |
|------|------|
| Desktop 앱 | macOS/Windows에서 GUI로 사용 |
| 터미널 CLI | 터미널에서 `claude` 명령어로 사용 |
| VS Code / JetBrains | IDE 안에서 확장 프로그램으로 사용 |
| 웹 | claude.ai/code 에서 브라우저로 사용 |
| 모바일 앱 | iOS, Android 앱에서 작업을 시작하고 진행 상황을 확인 |

자신의 컴퓨터에 있는 파일을 직접 다루려면 Desktop 앱, CLI, 또는 IDE 확장을 설치해야 합니다. 웹과 모바일에서는 클라우드 환경에서 작업합니다.

## 어떤 것을 쓸까?

아래 환경 중 최소 하나는 설치해야 합니다.

- **개발이 목적** → CLI 설치 필수. Git 연동과 스크립트 자동화에 강합니다
- **일반 업무가 목적** → Desktop 앱 설치 필수. 터미널 없이 GUI로 문서 분석, 보고서, 이메일 등을 처리할 수 있습니다
- **설치 없이 체험** → claude.ai/code 에서 바로 사용할 수 있습니다

나머지 환경(VS Code, JetBrains 등)은 필요한 시점에 별도 설치하셔도 됩니다.

**필요한 것**
- Anthropic 유료 구독 (Pro, Max, Teams, Enterprise 중 하나)
- macOS, Windows 10+, 또는 Ubuntu 20.04+ (Desktop 앱은 Linux 미지원)

## Step 1. Desktop 앱 설치

`claude.ai/download` 에서 데스크톱 앱을 다운로드합니다.

- **macOS**: .dmg 파일을 열고 Applications 폴더로 드래그합니다
- **Windows**: .exe 설치 파일을 실행합니다

앱을 열고 Anthropic 계정으로 로그인합니다. 상단에 Chat, Cowork, Code 탭이 보이면 성공입니다. Desktop 앱에 Claude Code가 포함되어 있어 별도 설치가 필요 없습니다.

참고로, 세 탭 모두 같은 Claude 모델이고 차이는 파일 접근 방식과 자율성입니다.

| 탭 | 역할 | 로컬 파일 접근 |
|----|------|----------------|
| Chat | 질문, 브레인스토밍, 문서 초안 등 일반 대화 | 없음 (첨부만 가능) |
| Cowork | 긴 작업을 맡기면 클라우드에서 자율 실행. 앱을 닫아도 계속 작업 | 없음 (클라우드 VM 자체 환경) |
| Code | 내 프로젝트 폴더의 파일을 직접 읽고 수정. 변경을 확인하고 승인 | 직접 접근 |

## Step 2. Desktop 앱에서 첫 대화

Chat, Cowork, Code 탭 중 아무 탭에서나 대화를 시작할 수 있습니다. Chat 탭을 선택하고 입력합니다.

```
안녕
```

## Step 3. CLI 설치

터미널에서 `claude` 명령어를 사용하려면 CLI를 별도로 설치합니다.

```bash
# macOS / Linux
curl -fsSL https://claude.ai/install.sh | bash

# Windows PowerShell
irm https://claude.ai/install.ps1 | iex
```

설치 확인:

```bash
claude --version
```

## Step 4. CLI에서 첫 대화

CLI는 현재 폴더 기준으로 동작합니다. 연습용 폴더를 만들고 들어갑니다.

```bash
# macOS / Linux
mkdir ~/claude-test
cd ~/claude-test

# Windows PowerShell
mkdir $HOME\claude-test
cd $HOME\claude-test
```

`claude`를 실행합니다.

```bash
claude
```

처음 실행하면 브라우저가 열리면서 로그인 화면이 나타납니다. 로그인하면 터미널에 프롬프트가 나타납니다.

```
안녕
```

종료하려면 `/exit`을 입력하거나 `Ctrl+C`를 두 번 누릅니다.

## 기타 환경 설치

- **VS Code**: 확장 프로그램에서 "Claude Code" 검색 후 설치
- **JetBrains**: JetBrains Marketplace에서 "Claude Code" 플러그인 설치
- **웹**: claude.ai/code 접속 (설치 불필요)
- **모바일**: App Store(iOS) 또는 Google Play(Android)에서 Claude 앱 설치
