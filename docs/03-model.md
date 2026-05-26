# 03. 모델과 Effort

Claude Code에서는 모델과 사고 깊이(effort)를 직접 선택할 수 있습니다. 좋은 모델에 높은 effort를 쓸수록 결과가 좋지만, 그만큼 토큰(비용)도 많이 씁니다. 작업 성격에 맞게 조절하는 게 좋습니다.

## 모델 선택

`/model`을 입력하면 사용 가능한 모델 목록이 나타납니다.

| 모델 | 특징 |
|------|------|
| Opus 4.6 (1M context) | 가장 강력. 복잡한 작업에 적합 (기본값, 권장) |
| Sonnet 4.6 | 일반적인 작업에 무난한 선택 |
| Sonnet 4.6 (1M context) | Sonnet에 확장 컨텍스트. 추가 비용 발생 ($3/$15 per Mtok) |
| Haiku 4.5 | 가장 빠름. 간단한 질문에 적합 |

화살표 키로 선택하고 Enter로 확인합니다. 선택한 모델은 이후 세션에도 계속 적용됩니다. CLI에서는 `--model` 플래그로 지정할 수도 있습니다.

```bash
claude --model sonnet
```

`/fast`를 입력하면 Fast mode가 켜집니다. Opus 모델이 더 빠르게 동작하지만 토큰 소진도 빨라집니다. v2.1.142부터 Fast mode 기본 모델은 Opus 4.7이며, Opus 4.6으로 고정하려면 환경변수 `CLAUDE_CODE_OPUS_4_6_FAST_MODE_OVERRIDE=1`를 설정합니다.

## Effort 레벨

같은 모델이라도 사고 깊이를 조절할 수 있습니다.

| 레벨 | 어울리는 작업 | 특징 |
|------|------------|-----------|
| low | 간단한 질문, 오타 수정, 파일 이름 변경 | 빠르고 가볍게 |
| medium | 코드 작성, 문서 정리 | 균형 |
| high | 복잡한 버그 분석, 아키텍처 설계, 여러 파일 리팩토링 (Opus 4.6/Sonnet 4.6 기본값) | 깊고 느리게 |
| xhigh | 난이도 높은 코딩/에이전트 작업 (Opus 4.7 전용, 기본값) | 매우 깊게 |
| max | 토큰 제한 없이 최대 사고 (현재 세션만 적용) | 최대 |

```
/effort [low|medium|high|xhigh|max|auto]
```

`/model`을 열면 하단에 effort 슬라이더도 같이 나옵니다. `← →` 키로 조절하고 Enter로 확인합니다.

`/effort auto`로 모델 기본값으로 되돌릴 수 있습니다. 현재 effort 레벨은 작업 스피너 옆에 "with low effort"처럼 표시됩니다.

## ultrathink

특정 질문 하나에만 깊이 생각하게 하고 싶을 때는 메시지에 `ultrathink`를 넣으면 됩니다. 그 턴에만 high 수준이 적용되고, 위치는 앞이든 뒤든 상관없습니다.

```
ultrathink 이 인증 로직에서 race condition이 생길 수 있는 경로를 모두 분석해줘
```
