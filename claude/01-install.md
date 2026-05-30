# ❶ 사용 준비하기

Claude Code는 **Desktop 앱**과 **CLI** 두 가지 방식으로 사용할 수 있습니다. \
목적에 맞게 하나를 선택하거나, 둘 다 설치해도 됩니다.

***

## ⏵ 어떤 걸 선택할까?

```
개발이 목적 (코드, Git, 자동화)   →  CLI
일반 업무 (문서, 보고서, 이메일)   →  Desktop 앱
둘 다                        →  둘 다 설치
설치 없이 먼저 체험             →  claude.ai/code (웹)
```

***

## ⏵ 사전 요건

| 항목                 | 내용                                   |
| ------------------ | ------------------------------------ |
| **Anthropic 계정**   | [claude.ai](https://claude.ai) 가입 필요 |
| **유료 구독**          | Pro, Max, Teams, Enterprise 중 하나     |
| **OS (Desktop 앱)** | macOS 13.0+, Windows 10 1809+        |
| **OS (CLI)**       | macOS 13.0+, Windows 10 1809+        |
| **RAM**            | 4GB 이상                               |
| **프로세서**           | x64 또는 ARM64                         |

***

## ⏵ Desktop 앱

### Desktop 앱이란?

터미널 없이 GUI로 Claude를 사용하는 방법입니다. 앱 안에 **Chat / Cowork / Code** 3개 탭이 있고, Code 탭에서 로컬 파일을 직접 읽고 수정할 수 있습니다.

| 탭          | 용도                       | 로컬 파일        |
| ---------- | ------------------------ | ------------ |
| **Chat**   | 일반 대화, 문서 초안, 브레인스토밍     | 없음 (첨부만 가능)  |
| **Cowork** | 긴 작업을 맡기면 클라우드에서 자율 실행   | 없음 (클라우드 VM) |
| **Code**   | 내 폴더 파일을 직접 읽고 수정, 변경 승인 | 직접 접근        |

### macOS 설치

1. [claude.ai/download](https://claude.ai/download) 접속
2. **macOS** 버튼 클릭 → `.dmg` 파일 다운로드
3. `.dmg` 파일 열기 → Claude 아이콘을 **Applications 폴더로 드래그**
4. Launchpad 또는 Applications에서 Claude 실행
5. Anthropic 계정으로 로그인

상단에 Chat / Cowork / Code 탭이 보이면 설치 완료입니다.

### Windows 설치

1. [claude.ai/download](https://claude.ai/download) 접속
2. **Windows** 버튼 클릭 → `.exe` 설치 파일 다운로드
3. 다운로드한 `.exe` 파일 실행
4. 설치 마법사 따라 진행
5. Anthropic 계정으로 로그인

상단에 Chat / Cowork / Code 탭이 보이면 설치 완료입니다.

### 자동 업데이트

Desktop 앱은 시작 시 자동으로 업데이트를 확인하고 백그라운드에서 최신 버전을 유지합니다. 별도로 업데이트를 신경 쓸 필요가 없습니다.

### 첫 대화 (Desktop 앱)

Chat 탭을 선택하고 아무 말이나 입력해보세요.

```
안녕
```

***

## ⏵ CLI

### CLI란?

터미널에서 `claude` 명령어로 사용하는 방법입니다. Git 연동, 스크립트 자동화, IDE 통합에 강합니다. Desktop 앱과 별도로 설치해야 합니다.

### 사전 요건 (CLI 전용)

| 항목                  | 내용                                                 |
| ------------------- | -------------------------------------------------- |
| **Node.js**         | v18 이상 필요 ([nodejs.org](https://nodejs.org) 에서 설치) |
| **Git for Windows** | Windows에서 Git 기능 사용 시 필요. 없으면 PowerShell로 대체됨      |

### macOS 설치

터미널을 열고 아래 명령어를 실행합니다.

```bash
# 안정 버전 설치
curl -fsSL https://claude.ai/install.sh | bash

# 최신 버전으로 설치
curl -fsSL https://claude.ai/install.sh | bash -s latest
```

### Windows 설치

**PowerShell** (권장):

```powershell
irm https://claude.ai/install.ps1 | iex
```

**명령 프롬프트 (CMD)**:

```cmd
curl -fsSL https://claude.ai/install.cmd -o install.cmd && install.cmd && del install.cmd
```

> `sudo npm install -g`로 설치하지 마세요. 권한 문제와 보안 위험이 생길 수 있습니다.

### 설치 확인

```bash
claude doctor
```

모든 항목이 정상으로 표시되면 설치 완료입니다.

### 로그인

처음 `claude`를 실행하면 브라우저가 자동으로 열립니다.

```bash
claude
```

브라우저에서 Anthropic 계정으로 로그인하면 터미널에 프롬프트가 나타납니다. 이후부터는 `claude` 명령어만 입력하면 바로 시작됩니다.

### 첫 대화 (CLI)

연습용 폴더를 만들고 실행해봅니다.

```bash
# macOS
mkdir ~/claude-test && cd ~/claude-test
```

```powershell
# Windows PowerShell
mkdir $HOME\claude-test; cd $HOME\claude-test
```

```bash
claude
```

프롬프트가 나타나면 입력합니다.

```
안녕
```

종료하려면 `/exit` 입력 또는 `Ctrl+C` 두 번.

***

## ⏵ 기타 환경

| 환경            | 설치 방법                                                |
| ------------- | ---------------------------------------------------- |
| **VS Code**   | 확장 프로그램에서 `Claude Code` 검색 후 설치                      |
| **JetBrains** | JetBrains Marketplace에서 `Claude Code` 검색 후 설치        |
| **웹**         | [claude.ai/code](https://claude.ai/code) 접속 (설치 불필요) |
| **모바일**       | App Store(iOS) 또는 Google Play(Android)에서 `Claude` 검색 |
