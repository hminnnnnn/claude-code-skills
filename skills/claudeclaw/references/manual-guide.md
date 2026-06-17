# ClaudeClaw 수동 가이드 (전체 단계 폴백)

> 이 문서는 `claudeclaw` 스킬이 대화로 안내하는 내용의 **전체 텍스트 버전**입니다.
> 스킬(SKILL.md)이 한 단계씩 안내하다 막히거나, 처음부터 끝까지 한눈에 보고 싶을 때 참고하세요.
> 명령어·경로·동작은 공식 telegram 플러그인 v0.0.6 문서(README.md / ACCESS.md)와 일치하도록 검증했습니다.

Claude Code의 **Channels(채널)** 기능을 사용하면, 텔레그램에서 직접 내 PC의 Claude Code 세션에 작업을 지시하고 결과를 받아볼 수 있습니다.

> **Channels란?** MCP(Model Context Protocol)를 활용해, 텔레그램 같은 메신저 앱을 실행 중인 내 PC의 Claude Code 세션과 양방향으로 연결해 주는 기능입니다. 컴퓨터 앞을 떠나 이동 중에도 휴대폰으로 Claude에게 일을 시키고 결과를 받을 수 있습니다.

---

## 사전 준비물

> **윈도우에서 Claude Code 자체가 아직 없다면**, 먼저 `references/windows-setup.md`
> ("0단계: 윈도우 사전 준비")로 Claude Code 설치·로그인을 끝낸 뒤 이 가이드로 오세요.

| 준비물 | 설명 |
|--------|------|
| **Claude Code v2.1.80+** | 채널(Channels)은 이 버전부터 지원되는 **리서치 프리뷰** 기능입니다. `claude --version` 으로 확인하고, 낮으면 업데이트하세요 |
| **로그인 상태** | **Pro / Max 개인 계정은 추가 결제 없이** 사용 가능. 회사(Team/Enterprise) 계정이면 조직 관리자가 채널 기능을 켜야 할 수 있습니다(결제 등급이 아니라 기능 토글) |
| **Bun 런타임** | 텔레그램 플러그인 서버가 Bun에서 동작합니다 (아래 설치 방법 참고) |
| **텔레그램 앱 + 계정** | 휴대폰 또는 데스크톱. 봇 생성에 필요 |

### Bun 설치 (아직 없을 때만)

**Windows (PowerShell):**

```powershell
powershell -c "irm bun.sh/install.ps1 | iex"
```

설치 후 **PowerShell 창을 완전히 닫고 새로 열어야** `bun` 명령이 인식됩니다.

**macOS / Linux:**

```bash
curl -fsSL https://bun.sh/install | bash
```

설치 후 터미널을 새로 열거나 `source ~/.zshrc`(zsh) / `source ~/.bashrc`(bash)를 실행합니다.

**설치 확인 (공통):**

```bash
bun --version
```

버전 번호(예: `1.3.x`)가 나오면 정상입니다.

---

## 1단계: 텔레그램 봇 만들기 + 토큰 받기 (휴대폰에서)

1. 텔레그램에서 **@BotFather** 를 검색해 대화를 엽니다.
2. **Start** 를 누른 뒤 `/newbot` 을 보냅니다.
3. **봇 이름(Name)** 을 입력합니다 — 화면에 보이는 이름. 한글/공백 가능 (예: `내 클로드 봇`).
4. **사용자명(Username)** 을 입력합니다 — 고유해야 하고 반드시 `bot` 으로 끝나야 합니다 (예: `helen_claude_bot`). 이게 봇 주소가 됩니다: `t.me/helen_claude_bot`.
5. BotFather가 **토큰**을 보내줍니다. 이런 형태입니다:

   ```
   123456789:AAHfiqksKZ8...
   ```

   맨 앞 숫자와 콜론(`:`)까지 **전부** 복사하세요.

> **주의:** 토큰은 비밀번호와 같습니다. 절대 남에게 공유하지 마세요. 클로드 채팅창에 그대로 붙여넣지 말고, 아래 3단계에서 `/telegram:configure` 명령 뒤에 붙여 입력하세요.

---

## 2단계: 텔레그램 플러그인 설치

`claude` 를 실행한 뒤(이미 실행 중이면 그대로), 입력창에 차례로 입력합니다:

```
/plugin install telegram@claude-plugins-official
```
```
/reload-plugins
```

- 설치 범위(scope)를 물으면 `user`(모든 프로젝트에서 사용 — 추천) 또는 `project`(현재 프로젝트만) 중 선택합니다.
- **`/reload-plugins` 는 꼭 실행하세요.** 공식 문서에 따르면 이 명령이 다음 단계의
  `/telegram:configure` 명령을 *활성화*합니다("activate the plugin's configure command").
  빠뜨리면 3단계에서 `Unknown command: /telegram:configure` 가 납니다.
- 아직 **프리뷰(Preview)** 기능이라 일부 불안정할 수 있습니다.

> **"플러그인을 찾을 수 없음(not found in any marketplace)" 에러:** 마켓플레이스가 없거나 오래된 것입니다.
> 처음이면 `/plugin marketplace add anthropics/claude-plugins-official` 로 추가하고, 이미 있으면
> `/plugin marketplace update claude-plugins-official` 로 갱신한 뒤 설치를 다시 시도하세요.

---

## 3단계: 토큰 등록

1단계에서 받은 토큰을 등록합니다. 입력창에:

```
/telegram:configure 123456789:AAHfiqksKZ8...
```

`123456789:AAH...` 자리에 본인 토큰을 붙여넣으세요. 이 명령은 토큰을
`~/.claude/channels/telegram/.env` 파일에 저장합니다(권한 600).

> **`Unknown command: /telegram:configure` 가 뜨면?** 명령이 틀린 게 아니라 2단계 플러그인이
> 아직 로드되지 않은 것입니다. `/reload-plugins` 를 실행하고 다시 시도하세요. 그래도 안 되면
> Claude Code를 종료(`/exit`)했다 다시 켜면 플러그인 명령이 확실히 등록됩니다.

---

## 4단계: `--channels` 플래그로 다시 켜기 (가장 중요)

> 이 단계가 핵심입니다. `--channels` 없이 켜면 텔레그램 메시지를 **수신하지 못합니다.**

현재 세션을 종료합니다 — 입력창에 `/exit` 입력(또는 키보드로 **Ctrl+C 두 번**):

```
/exit
```

그런 다음 터미널(윈도우는 PowerShell)에서 채널 플래그를 붙여 다시 실행합니다:

```sh
claude --channels plugin:telegram@claude-plugins-official
```

(Windows에서도 동일한 명령입니다.)

---

## 5단계: 페어링 (내 휴대폰 연결)

1. 텔레그램에서 1단계에 만든 봇을 찾아 **Start** 를 누르거나 아무 메시지나 보냅니다.
2. 봇이 **6자 페어링 코드**(영문+숫자 조합, 예: `a4f91c`)를 답장으로 보냅니다.
3. Claude Code 입력창으로 돌아와 입력합니다:

   ```
   /telegram:access pair a4f91c
   ```

   `a4f91c` 자리에 받은 코드를 넣으세요. 승인(Yes)이 뜨면 **Yes**.

> **봇이 코드를 안 보내면** → 거의 항상 4단계의 `--channels` 플래그를 빠뜨린 경우입니다. 세션을 종료하고 `claude --channels plugin:telegram@claude-plugins-official` 로 다시 켰는지 확인하세요. 코드는 시간이 지나면 만료되니, 안 되면 봇에 메시지를 다시 보내 새 코드를 받으세요.

---

## 6단계: 보안 잠금 (allowlist)

> 반드시 하세요. 페어링 모드(`pairing`)는 *내 ID를 캡처하기 위한 임시 상태*일 뿐입니다. 그대로 두면 봇 주소를 아는 누구나 페어링 코드를 받아 접근을 시도할 수 있습니다.

입력창에:

```
/telegram:access policy allowlist
```

이제 허용 목록(allowFrom)에 등록된 본인만 봇으로 Claude에 접근할 수 있습니다.

---

## 7단계: 최종 확인

텔레그램 봇 채팅창에 메시지를 보냅니다:

```
안녕 클로드
```

Claude Code 터미널에 메시지가 도착하고 응답이 텔레그램으로 돌아오면 **연동 성공**입니다.

실제 작업도 시켜보세요:

```
지금 폴더에 hello.txt 파일 만들어줘
```

---

## 전체 명령 요약 (Quick Reference)

```sh
# 0. Bun 설치 (Windows PowerShell) — 없을 때만. 설치 후 PowerShell 새로 열기
powershell -c "irm bun.sh/install.ps1 | iex"
#    (macOS/Linux: curl -fsSL https://bun.sh/install | bash)
bun --version

# 1. (휴대폰) @BotFather → /newbot → 이름·사용자명 → 토큰 복사

# 2. Claude Code 입력창에서
/plugin install telegram@claude-plugins-official
/reload-plugins

# 3. 토큰 등록
/telegram:configure <봇토큰>

# 4. 종료 후 채널 플래그로 재시작
/exit
claude --channels plugin:telegram@claude-plugins-official

# 5. (텔레그램에서 봇에 메시지 → 6자 코드 수신) 후 입력창에서
/telegram:access pair <6자코드>

# 6. 보안 잠금
/telegram:access policy allowlist

# 7. 텔레그램 봇에 "안녕 클로드" → 도착하면 성공
```

---

## 트러블슈팅

| 증상 | 가장 흔한 원인 → 해결 |
|------|----------------------|
| `bun: command not found` | Bun 설치 후 터미널/PowerShell을 새로 열지 않음 → 창을 완전히 닫고 새로 열기 |
| 플러그인 "not found in any marketplace" | 처음이면 `/plugin marketplace add anthropics/claude-plugins-official`, 아니면 `/plugin marketplace update claude-plugins-official` → 설치 재시도 (인터넷 확인) |
| `Unknown command: /telegram:configure` (또는 `/telegram:access`) | 명령 오타가 아니라 플러그인 미로드 → `/reload-plugins` 실행, 안 되면 Claude Code 재시작. 2단계 설치를 마쳤는지도 확인 |
| 봇이 페어링 코드를 안 줌 | `--channels` 플래그 누락이 1순위 → `/exit` 후 `claude --channels plugin:telegram@claude-plugins-official` 로 재시작. 그 다음 봇에 메시지 다시 |
| `/telegram:access pair` 실패 | 코드 오타 또는 만료 → 봇에 메시지 다시 보내 새 코드 받기 |
| 페어링 후에도 메시지가 안 옴 | 재시작 시 `--channels` 플래그를 넣었는지 / 토큰이 맞는지(`/telegram:configure`로 재등록) 확인 |
| 토큰 오류 | BotFather에서 `/token` 으로 재확인 후 `/telegram:configure` 로 다시 등록 |

---

## Windows 사용자 참고

- **터미널:** PowerShell 또는 Windows Terminal 권장. Bun 설치 명령은 **CMD가 아니라 PowerShell**에서 실행하세요.
- **관리자 권한 불필요:** Bun 설치·Claude Code 사용 모두 일반 권한으로 됩니다. (Bun은 사용자 단위로 PATH에 등록되므로 관리자 권한이 필요 없습니다.)
- **설치 후 창 새로 열기:** Bun 설치 후 PATH가 갱신되려면 PowerShell 창을 **완전히 닫고 새로 열어야** `bun --version` 이 인식됩니다.
- **방화벽:** 별도의 Windows Defender 예외 설정은 필요 없습니다. 처음 연결 시 네트워크 허용 팝업이 뜨면 **허용**을 선택하세요.
- **세션 유지:** 연동을 유지하려면 `claude --channels ...` 로 켠 터미널 창을 **닫지 말고 열어 두세요.** 창을 닫으면 연결이 끊깁니다.
- **토큰 붙여넣기 확인:** 긴 토큰을 붙여넣을 때 윈도우에서 일부가 잘리거나 공백이 섞일 수 있습니다. `/telegram:configure` 후 7단계(메시지 왕복)가 안 되면 토큰을 다시 복사해 재등록하세요.
- **(드문 케이스) `bun` 이 설치했는데도 계속 안 잡히거나 bash가 이상하게 동작:** WSL과 Git Bash가 둘 다 깔려 있으면 Claude Code가 `bash` 를 WSL로 잘못 잡는 알려진 문제가 있습니다. 이때는 `settings.json` 에 Git Bash 경로를 지정하세요:
  ```json
  { "env": { "CLAUDE_CODE_GIT_BASH_PATH": "C:\\Program Files\\Git\\bin\\bash.exe" } }
  ```
  (Claude Code는 윈도우에서 WSL 없이 동작합니다. Git for Windows는 선택사항으로, 설치하면 Bash 도구가 활성화됩니다.)

## 전부 맞게 했는데도 봇이 끝까지 무응답이라면

설정이 모두 정확한데도 봇이 한 번도 반응하지 않으면, 설정 문제가 아니라 **접근 권한** 문제일 수 있습니다:

1. `claude --version` 이 **v2.1.80 이상**인지 확인하세요(채널은 이 버전부터). 낮으면 업데이트.
2. 채널은 **점진 출시(research preview)** 중이라, 설정이 옳아도 아직 내 계정에 기능이 안 열렸을 수 있습니다.
3. **회사(Team/Enterprise) 계정**이면 조직 관리자가 채널 기능을 켜야 합니다. (Pro/Max 개인 계정은 추가 조치 불필요.)

---

## 더 알아보기

- 그룹 채팅, 여러 사용자 허용, 멘션 설정 등 고급 접근 제어 → 공식 플러그인 `ACCESS.md`
- 텔레그램 외 채널(Discord 등) → Anthropic "Build your own channel" 문서
