# claude-code-skills

흐민의 [Claude Code](https://code.claude.com) 스킬 모음입니다. 누구나 설치해서 쓸 수 있어요.

## 수록 스킬

### 🦅 claudeclaw — 텔레그램 봇 ↔ Claude Code 연결 도우미

휴대폰 텔레그램에서 내 PC의 Claude Code에게 일을 시킬 수 있도록, **봇 연동을 처음부터 끝까지
대화로 안내**하는 스킬입니다. 비개발자·Windows 초보를 기준으로 만들었어요.

- 한 단계씩 설명하고, **매 단계 실제로 됐는지 Claude가 직접 확인**해 줍니다.
- 중간에 세션을 다시 켜야 하는 구간이 있어도, `/claudeclaw`를 다시 실행하면 **멈춘 지점부터 이어서** 끝까지 진행합니다.
- 토큰 비노출·접근 권한은 사용자가 직접 설정 등 **보안 가드레일** 내장.

> **Windows에서 Claude Code가 아직 없다면?** 먼저
> [skills/claudeclaw/references/windows-setup.md](skills/claudeclaw/references/windows-setup.md)
> 의 "0단계: 사전 준비"를 따라 Claude Code와 Bun을 설치하세요. 그 다음 아래 설치로 진행합니다.

---

## 설치 방법

### 방법 1 — 플러그인 마켓플레이스 (권장)

Claude Code 안에서 차례로 입력하세요:

```
/plugin marketplace add hminnnnnn/claude-code-skills
/plugin install claudeclaw@claude-code-skills
```

설치 후 `/claudeclaw` 를 실행하면 안내가 시작됩니다.

### 방법 2 — 스킬 폴더에 직접 복사

이 저장소를 받은 뒤 `skills/claudeclaw` 폴더를 개인 스킬 폴더에 복사합니다.

- **Windows:** `%USERPROFILE%\.claude\skills\claudeclaw\`
- **macOS / Linux:** `~/.claude/skills/claudeclaw/`

```bash
git clone https://github.com/hminnnnnn/claude-code-skills.git
cp -R claude-code-skills/skills/claudeclaw ~/.claude/skills/claudeclaw
```

Claude Code를 다시 실행하면 `/claudeclaw` 로 사용할 수 있습니다.

---

## 사용법

```
/claudeclaw
```

Claude가 현재 상태를 점검한 뒤, 봇 생성(BotFather) → 플러그인 설치 → 토큰 등록 →
`--channels` 재시작 → 페어링 → 보안 잠금 → 최종 확인 순서로 안내합니다. 가이드만 따라가면 됩니다.

전체 수동 가이드는 [skills/claudeclaw/references/manual-guide.md](skills/claudeclaw/references/manual-guide.md) 에 있습니다.

---

## 라이선스

[MIT](LICENSE)
