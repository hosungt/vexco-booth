# vexco-booth 운영 매뉴얼 (진행자용)

2026 과학·수학·정보 교사 컨퍼런스 · 서울시교육청 수학 부스 — 1:1 바이브코딩 체험(10분) 진행 가이드입니다.
**이 문서는 진행자 전용**입니다(방문 선생님께 보여드리는 문서가 아닙니다).

> **현장에서 컴퓨터를 처음 세팅하신다면 → [0. 현장 세팅](#0-현장-세팅-맨바닥에서-시작할-때) 부터 보세요.**
> (이 페이지는 폰에서 github.com/hosungt/vexco-booth 로 열면 그대로 보입니다.)

---

## 0. 현장 세팅 (맨바닥에서 시작할 때)

**소요 시간: 15~25분** (인터넷 속도에 따라). Windows 기준.

### 전날 USB에 담아 갈 것

| 담을 것 | 크기 | 왜 |
|---|---|---|
| `vexco-booth` 폴더 **통째로**(`node_modules` 포함) | 3.5MB | 복사만 하면 `npm install` 불필요 |
| Node.js LTS 설치 파일 (`.msi`, nodejs.org에서 다운로드) | 30MB 내외 | 현장 인터넷이 느릴 때 대비 |
| VS Code 설치 파일 (선택) | 100MB 내외 | 터미널만 쓸 거면 없어도 됨 |

> 인터넷이 잘 되면 USB 없이도 됩니다 — GitHub에서 **Code → Download ZIP** 으로 받는 쪽이
> 항상 최신입니다. USB는 **인터넷이 안 될 때의 보험**으로 챙기세요.

### 현장 순서

1. **Node.js 설치** — USB의 `.msi` 실행(또는 nodejs.org LTS). 설치 후 터미널에서 `node --version` 확인.
   (개발 기준 버전: Node 24 / npm 11. LTS면 무엇이든 동작합니다.)
2. **Claude Code 설치 + 로그인** — 둘 중 하나
   - **VS Code 방식**: VS Code 설치 → 확장에서 `Claude Code` 설치 → 로그인(진행자 Max 계정)
   - **터미널 방식**(더 빠름): `npm i -g @anthropic-ai/claude-code` → `claude` 실행 → 로그인
3. **저장소 배치** — USB의 `vexco-booth` 폴더를 **`C:\dev\vexco-booth`** 로 복사.
   **경로를 반드시 이대로** 두세요(대본에 절대경로가 들어 있습니다).
   인터넷이 되면 대신: `git clone https://github.com/hosungt/vexco-booth C:/dev/vexco-booth` → `npm --prefix C:/dev/vexco-booth install`
4. **동작 확인 3가지**
   ```bash
   npx --prefix C:/dev/vexco-booth qrcode --small "test"    # QR이 그려지면 OK
   cmd //c start "" "C:\dev\vexco-booth\gallery.html"        # 슬라이드가 열리면 OK
   ```
   그리고 `C:\dev\vexco-booth` 에서 Claude Code를 열어 **`/booth 테스트`** 1회.
5. **전역 설정 확인** — 남의 컴퓨터라면 대개 없지만, 있으면 치웁니다.
   ```bash
   ls ~/.claude/CLAUDE.md                       # 있으면 → mv ~/.claude/CLAUDE.md ~/.claude/CLAUDE.md.bak
   ls ~/.claude/plugins/cache                   # superpowers 등이 있으면 /plugin 으로 비활성화
   ```

### 세팅이 끝내 안 될 때 (축소 운영)

Claude Code를 못 띄우면 **기획 체험만으로도 세션은 성립합니다.**

1. `gallery.html` 을 브라우저로 열어 슬라이드 1~2를 함께 봅니다.
2. 기획 카드 네 조각을 **말과 종이로** 채웁니다(유인물 앞면과 같은 틀).
3. `templates/` 의 완성작을 열어 "이런 게 만들어집니다"를 보여드립니다.
4. 그 파일을 그대로 선생님 GitHub에 업로드해 **배포까지** 경험시켜 드립니다.
5. 유인물을 드리고, 집에서 웹 채팅으로 재현하시도록 안내합니다.

핵심 메시지("기획이 90%")와 산출물(URL·프롬프트)은 이 경로로도 전달됩니다.

---

## 1. 세션 시작법

1. Claude Code를 `C:\dev\vexco-booth` 에서 열어 실행합니다.
2. `/booth 성함` 입력 — 예: `/booth 김민준`
   - 성함 없이 `/booth` 만 실행하면 스킬이 **한 번만** 성함을 여쭙고, 그래도 없으면 폴더명이 `선생님` 으로 대체됩니다.
3. 선생님이 바뀌면 `/clear` 후 다시 `/booth 성함` 으로 시작합니다.

---

## 2. 4막 개요 (10분 + 리셋 2분 = 슬롯 12분)

| 막 | 시간 | 내용 |
|---|---|---|
| ① 아이디어 | ~3분 | **`gallery.html`(만들 수 있는 것 예시 8종, 직접 만져보는 데모 포함)을 먼저 띄우고** "만들고 싶은 것 있으세요?" — 있으면 구체화 질문 3~5개, 없으면 갤러리에서 형태를 고르거나 학교급·단원·취향 확인 후 씨앗 제안 |
| ② 기획 카드 | ~2분 | 대화를 4조각 기획 카드로 정리해 화면에 출력(세션의 클라이맥스). 선생님 확인 1회, 수정 있으면 반영 |
| ③ 제작 | ~3분 | Claude가 쉘(quiz/speed/pair/order 중 하나) 또는 자유 생성으로 `index.html` 제작 + 자체 검증 + 교사 15초 확인. **이때 진행자는 병렬로 크롬 시크릿 창에서 GitHub 가입·로그인 + 리포 생성**, 완료되면 **사용자명·저장소명을 Claude에게 전달**(④막 URL·QR에 필요) |
| ④ 배포 | ~2분 | README 생성 → 워크스페이스 폴더 열기 → GitHub 웹 업로드·Pages 설정 안내 → QR 출력 → 폰 스캔 |

### ③막 — 진행자가 병렬로 할 일

- 크롬 **시크릿 창 1개만** 사용.
- github.com → **Sign up**(기존 계정 있으면 Sign in) → 이메일 인증까지 완료(이메일 인증은 선생님 폰에서 확인).
- **New repository** → 저장소명(영문·간단히) → **Public** → **Add a README file 체크**(main 브랜치가 만들어져 웹 업로드·Pages 설정이 바로 가능) → **Create repository**.
- 비밀번호·인증코드는 **선생님이 직접 입력**하고, 진행자는 개입하지 않습니다.
- 리포가 만들어지면 **사용자명과 저장소명을 Claude에게 알려주세요** — 체험 주소와 QR에 그대로 쓰입니다. (아직 못 받았으면 Claude가 QR 출력 전 먼저 물어봅니다.)

---

## 3. 세션 종료 체크리스트 (④막 끝에 Claude가 출력)

1. GitHub **명시적 로그아웃** → **모든 시크릿 창 닫기** → 시크릿 창 **0개** 확인(자격증명 제거)
2. 유인물(`handout.html` 출력본) 1장 전달
3. `/clear` 후 다음 세션 대기 (workspace 폴더는 로컬에 그대로 보존)

---

## 4. 노트북 세팅 목록 (오늘 밤, 세션 전용 노트북)

1. Node.js LTS, git 설치 확인 / VS Code + Claude Code 확장 설치, **진행자 계정 로그인**(Max 요금제 — 세션 모델 Opus 계열 확인)
2. `vexco-booth` clone → `npm install`(`qrcode` 패키지 포함) → `npx qrcode --small` 동작 확인 — 전역 `~/.claude/CLAUDE.md`·superpowers 플러그인은 **설치하지 않음**(전역 규칙이 라이브 세션을 방해하지 않도록), 부스 리포의 프로젝트 `CLAUDE.md`가 규칙을 담당
3. `.claude/settings.json` 권한 allowlist 확인 — workspace 파일 편집, 미리보기 열기(`cmd //c start`·`explorer.exe`), QR(`npx qrcode`) 등 세션에 쓰는 것만 allow 되어 있는지 점검(라이브 중 승인 프롬프트 제거). 리허설에서 승인 프롬프트가 뜨는 명령이 있으면 그 패턴을 allowlist에 추가
4. 브라우저: 선생님 GitHub 작업은 **시크릿 창 1개**에서만. 세션 간 시크릿 창 0개 확인
   - **GitHub CLI(`gh`)는 설치하지 않거나 로그아웃 상태로 둔다** — 진행자 계정으로 방문
     교사의 작품이 올라가는 사고를 원천 차단. 리포 clone은 public이라 인증이 필요 없다
     (`git clone https://github.com/hosungt/vexco-booth`). `.claude/settings.json` 도
     `git`·`gh` 를 deny 한다.
5. 노트북 상태: 전원 연결, 절전 해제, 알림/집중 모드, OS·브라우저 자동 업데이트 일시 중지, 화면 배율 고정
6. **리허설**: `/booth 테스트`로 전체 흐름 실측 — 쉘 경로 최소 2회 + 자유 생성 1회 + 실제 GitHub 업로드·Pages·QR 폰 스캔까지. 10분 완주 확인, 어긋나면 SKILL.md 수정. **리허설 종료 시점에 지원 쉘 목록 확정**
7. 폰 핫스팟 연결 테스트(네트워크 이상 시 전환용)

---

## 4-1. 리허설 체크리스트 (라이브에서만 증명되는 것들)

- [ ] `/booth 테스트` 로드 실측 — 스킬이 정상 진입하는지
- [ ] 해피 패스 완주하며 **승인 프롬프트 전수 기록** — 뜨는 명령 패턴은 즉시 `.claude/settings.json` allow에 추가 (특히 `cmd //c start` 미리보기, `npx qrcode` — repo 루트에서)
- [ ] **재시작 시나리오 1회**: ③막 도중 강제 종료 → `/booth 같은이름` → 진행자가 "이어서" → 기존 폴더에서 이어가는지
- [ ] **가입 지연 시나리오 1회**: 사용자명·저장소명 없이 ④막 진입 → QR·README에 플레이스홀더 산출물 0건인지
- [ ] CONFIG 수정 후 미리보기 재실행 시 새 내용이 보이는지 (캐시로 구버전이면 대본에 "F5" 추가)
- [ ] **인쇄된 유인물 QR 폰 실스캔** (화면 검증은 완료, 종이 인쇄 품질 확인)
- [ ] 핫스팟 전환 상태에서 Claude 응답 + QR + GitHub 업로드 1회
- [ ] Pages 실배포 시간 실측 + 실접속 ("1분 내" 문구 현실성)
- [ ] order 쉘 실기기 판정 — 실패 시 SKILL.md 2곳(결정표·장애 분기 표) + seeds.md 씨앗 2개에서 order 제거

---

## 5. 스킬 고치기 (리허설·현장에서 손볼 때)

### 5-1. 어디를 고쳐야 하나

| 고치고 싶은 것 | 파일 |
|---|---|
| Claude가 하는 **말·질문·순서·타이밍** | `.claude/skills/booth/SKILL.md` ← 대부분 여기 |
| **시작 슬라이드**(예시·3질문·진행 방식·가져갈 것) | `gallery.html` |
| **콘텐츠 엔진**(문제 화면 동작·디자인) | `templates/quiz.html` 등 5개 |
| 아이디어 **씨앗 목록** | `seeds.md` |
| **기능 요청 대응·제안** 목록 | `extras.md` |
| **인쇄 유인물** | `handout.html` |
| **승인 프롬프트가 뜰 때** 허용 목록 | `.claude/settings.json` 의 `allow` |

자주 있는 경우:

- *"대사가 어색하다 / 질문이 많다 / 시간이 밀린다"* → **SKILL.md** 해당 막
- *"쉘 하나를 빼고 싶다"* → SKILL.md 결정표 + 장애 분기 표 + `seeds.md` + `gallery.html` 카드
- *"○○ 명령에서 승인 프롬프트가 뜬다"* → settings.json `allow` 에 그 패턴 추가

### 5-2. 어디서 세션을 여나 — **상위 폴더에서 여세요**

`C:\dev\vexco-booth` 에서 Claude Code를 열면 부스용 잠금이 걸려 **`templates/` 수정과
git 명령이 차단**됩니다(방문 교사 세션을 지키려고 일부러 막아둔 것). 개발할 때는 한 단계 위
**`C:\dev`** 를 열어 작업하세요. 잠금이 적용되지 않아 그대로 고치고 커밋할 수 있고,
부스용 설정을 건드리지 않아 되돌릴 것도 없습니다.

> `C:\dev\vexco-booth` 에서 꼭 작업해야 한다면 `.claude\settings.json` 을 잠시 다른 이름으로
> 바꿨다가 **반드시 원래 이름으로 되돌리세요.** 되돌리지 않으면 세션 중 방어가 사라집니다.

### 5-3. 새 세션에 맥락 주기 (그대로 복사)

> 이 저장소는 컨퍼런스 부스에서 쓰는 1:1 바이브코딩 체험 스킬이야.
> `C:\dev\vexco-booth\README.md` 와 `C:\dev\vexco-booth\.claude\skills\booth\SKILL.md` 를 먼저 읽어줘.
> 설계 문서는 `C:\dev\2026-vexco-math\docs\superpowers\` 에 있어.
> 고칠 것: (여기에 고칠 내용)

### 5-4. 고친 뒤 반드시

1. **확인** — 슬라이드·쉘을 고쳤으면 브라우저로 열어 눈으로 본다.
   ```bash
   cmd //c start "" "C:\dev\vexco-booth\gallery.html"
   ```
2. **`/booth 테스트` 1회** — 대본을 고쳤으면 실제로 한 바퀴 돌려본다.
3. **커밋·push** — `git -C C:\dev\vexco-booth add -A` → `commit` → `push`
4. **현장 반영** — 현장 컴퓨터에서 `git -C C:\dev\vexco-booth pull`.
   인터넷이 안 되면 **USB에 폴더를 다시 담아** 간다(push만 하고 USB를 안 바꾸면 옛 버전이 간다).

### 5-5. 하지 말 것

- **선생님이 앞에 계실 때 고치지 않는다.** 세션과 세션 사이에만.
- 여러 기기에서 동시에 고치지 않는다(충돌). 노트북·현장 중 **한 곳만** 기준으로.
- 급하지 않은 것은 손대지 않는다. 행사 당일에는 **작동하는 것을 지키는 쪽**이 낫다.

---

## 6. 리스크와 대응

| 리스크 | 대응 |
|---|---|
| 네트워크 이상 | 상시 연결 전제. 이상 시 폰 핫스팟으로 전환(Claude·GitHub 모두 핫스팟 경유) |
| GitHub 가입 지연(이메일 인증 등) | ③막 병렬 진행으로 흡수. 미완이면 파일 보존 + README·유인물 안내로 마무리 |
| Claude 응답 지연·장애 | 쉘 기본 CONFIG(=완성 예제)로 시연·완성하는 분기. 기획 카드 체험은 그대로 유효 |
| 자유 생성 품질 편차 | 결정표로 진입 제한 + 가드레일 + 검증 2단. 시간 초과 조짐이면 진행자 큐로 쉘 유도 |
| Pages 빌드 지연 | QR 먼저 발급, "잠시 후 열려요" |
| CONFIG 오류로 백지 화면 | JSON 블록 방식 + 엔진의 CONFIG 오류 화면 표시로 방지 |
| order-game 모바일 드래그 불안정 | 위/아래 버튼 폴백 기본 제공, 리허설 실패 시 3종으로 축소 |
