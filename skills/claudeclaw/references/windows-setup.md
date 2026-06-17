# 0단계: 윈도우 사전 준비 — 아무것도 없는 상태에서 Claude Code 준비하기

> 이 문서는 **Claude Code 자체가 아직 없는 윈도우 사용자**를 위한 "0단계"입니다.
> 여기까지 끝내면 `claude` 가 실행되고 `/claudeclaw` 같은 스킬을 쓸 수 있어요.
> 명령·요구사항은 공식 문서(code.claude.com/docs)로 검증했습니다. (2026-06 기준)

> **스킬은 따로 "설치"하지 않아요.** `claude` 를 켜고 입력창에 `/` 를 치면 사용할 수 있는
> 명령과 스킬이 바로 보입니다. 즉 0단계의 목표는 "스킬 설치"가 아니라 **`claude` 로그인까지** 끝내는 것입니다.

---

## 시작 전 확인 (요구사항)

| 항목 | 필요 조건 |
|------|-----------|
| **운영체제** | Windows 10 (1809 / 2018년 10월 업데이트) 이상, 또는 Windows Server 2019+ |
| **하드웨어** | RAM 4GB 이상, x64 또는 ARM64 |
| **네트워크/지역** | 인터넷 연결 + Anthropic 지원 국가 |
| **계정(중요)** | **Pro · Max · Team · Enterprise 구독** 또는 **Console(API 종량제) 계정** 중 하나. ⚠️ **무료 Claude.ai 플랜에는 Claude Code가 포함되지 않습니다.** (구독이 유일한 길은 아니에요 — Console에 선불 크레딧을 넣어 API 키로 쓰는 방법도 됩니다.) |
| **관리자 권한** | **필요 없음.** 일반 사용자 권한으로 전부 됩니다. |
| **WSL / Node.js** | **불필요.** 아래는 윈도우 네이티브 설치 방식입니다(권장). |

---

## PowerShell에 순서대로 입력

### 1. PowerShell 열기

시작 메뉴 → **"PowerShell"** 검색 → 실행. 창 맨 앞이 `PS C:\Users\...>` 처럼 **`PS` 로 시작**하는지 확인하세요.
(`PS` 가 없으면 CMD(명령 프롬프트)이고, 다음 명령이 `'irm' 은(는) ... 인식되지 않습니다` 오류가 납니다.)

### 2. Claude Code 설치 (네이티브 윈도우 · 관리자 권한 불필요)

```powershell
irm https://claude.ai/install.ps1 | iex
```

> 이 명령은 공식 설치 스크립트를 내려받아 바로 실행합니다(공식 방식). 출처는 반드시
> `https://claude.ai/install.ps1` 여야 합니다 — 다른 주소의 스크립트는 실행하지 마세요.

### 3. PowerShell 창을 닫고 새로 열기

설치로 추가된 경로(PATH)가 적용되려면 **현재 창을 닫고 PowerShell을 새로 열어야** 합니다.
(이걸 안 하면 4번에서 `claude` 를 못 찾는 경우가 대부분입니다.)

### 4. 설치 확인

```powershell
claude --version
```
버전 번호가 나오면 설치 성공입니다. 더 꼼꼼히 점검하려면:
```powershell
claude doctor
```
(설치 상태·설정·MCP 서버 등을 진단해 줍니다.)

### 5. 작업 폴더로 이동

```powershell
cd C:\경로\내프로젝트
```
Claude Code는 "현재 폴더"를 기준으로 동작합니다(스킬·설정·맥락이 폴더 단위). 연습용 빈 폴더라도 괜찮아요.

### 6. 첫 실행 + 로그인

```powershell
claude
```
처음 실행하면 **브라우저 로그인 창**이 열립니다. 로그인을 마치면 자동으로 터미널로 돌아오고,
이후에는 다시 로그인하지 않아도 됩니다.
- 로그인 옵션: **Pro/Max/Team/Enterprise** 계정, 또는 **Claude Console(API 선불 크레딧)** 등.
- 브라우저가 자동으로 안 열리면 안내에 따라 `c` 를 눌러 주소를 복사해 직접 열거나, 코드 붙여넣기 방식을 쓰면 됩니다.

### 7. 준비 완료 확인

세션 안에서 `/` 를 입력해 보세요. 사용할 수 있는 명령과 **스킬 목록**이 뜨면 끝입니다.
`/help` 로도 확인할 수 있어요.

---

## claudeclaw(텔레그램 연동)를 쓰려면 — 추가 준비물

위 1~7번은 Claude Code 자체 준비입니다. **텔레그램 봇 연동(`/claudeclaw`)** 까지 하려면
플러그인 서버 런타임인 **Bun**이 하나 더 필요합니다. (Bun은 Claude Code 자체의 필수 요소는
아니고, 텔레그램 플러그인이 Bun에서 돌기 때문입니다.)

```powershell
powershell -c "irm bun.sh/install.ps1 | iex"
```
→ 설치 후 **PowerShell을 새로 열고** 확인:
```powershell
bun --version
```

**(선택) Git for Windows** — 꼭 필요하진 않습니다. 설치하면 Claude Code의 Bash 도구가
활성화되고(없으면 PowerShell 도구로 동작), 다운로드는 `https://git-scm.com/downloads/win` 에서
받아 GUI 설치 마법사를 따라가면 됩니다.

준비가 끝나면 `/claudeclaw` 를 실행하세요. 나머지는 스킬이 한 단계씩 안내합니다.

---

## 막힐 때

| 증상 | 원인 → 해결 |
|------|-------------|
| `'irm' 은(는) ... 인식되지 않습니다` | CMD에서 실행함 → **PowerShell**(`PS` 로 시작하는 창)에서 다시 |
| `claude` 를 찾을 수 없음 (`command not found`) | 설치 후 창을 안 새로 엶 → PowerShell을 **완전히 닫고 새 창**에서 `claude --version` 다시 |
| 로그인 후 "플랜이 없다"는 안내 | 무료 Claude.ai 플랜은 Claude Code 미포함 → Pro/Max/Team/Enterprise 구독 또는 Console(API 크레딧) 계정으로 로그인 |
| `bun` 을 찾을 수 없음 | Bun 설치 후 PowerShell 새 창에서 다시 (claudeclaw 진행 시에만 필요) |

> 자동 업데이트는 네이티브 설치에서 백그라운드로 동작하므로, 평소 따로 업데이트할 필요는 없습니다.
