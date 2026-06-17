# 🍵 엄마들의 시간 · 누적 기록 사이트

키즈러닝랩 「엄마들의 시간」 모임을 매 회차마다 쌓아가는 정적 웹사이트.

---

## 📁 폴더 구조

```
site/
├── index.html                  ← 메인 (회차 목록)
├── README.md                   ← 이 파일
├── assets/
│   ├── style.css               ← 모든 페이지가 공통으로 쓰는 스타일
│   ├── krl-logo.png            ← 키즈러닝랩 로고
│   └── tea-photo.jpg           ← 찻자리 분위기 사진
├── sessions/
│   └── session-01.html         ← 1회 모임 상세
├── recordings/
│   ├── session-01-part1.m4a    ← 1회 녹음 (도입부 14분)
│   └── session-01-part2.m4a    ← 1회 녹음 (본격 71분)
└── pdfs/
    ├── The_Thinking_Game_찻자리자료.pdf
    ├── The_Thinking_Game_아이용만화.pdf
    ├── AI_특이점_5년안에_찻자리자료.pdf
    └── AI_특이점_5년안에_아이용만화.pdf
```

---

## 🚀 빠르게 보는 법

### 1) 로컬에서 바로 열기
- Finder에서 `site/index.html` 더블클릭 → 사파리나 크롬에서 열림
- 단, 오디오는 일부 브라우저에서 로컬 파일 정책 때문에 안 들릴 수 있음

### 2) 로컬 서버 띄우기 (오디오 재생까지 확실히)
터미널에서:
```bash
cd "/Users/nangmanwife/Library/Mobile Documents/iCloud~md~obsidian/Documents/키즈러닝랩/연차 엄마들의 시간/site"
python3 -m http.server 8000
```
브라우저에서 `http://localhost:8000` 접속.

### 3) 인터넷에 올리기 (참고 사이트와 같은 방식)
**Cloudflare Pages** 또는 **Vercel** 추천:

#### Cloudflare Pages
1. https://pages.cloudflare.com → "Create a project" → "Upload assets"
2. `site` 폴더를 zip으로 압축해서 업로드
3. 5초 만에 배포 완료, `something.pages.dev` 도메인 자동 발급
4. 무료, 트래픽 제한 거의 없음

#### Vercel
1. https://vercel.com → "Add New" → "Project" → "Browse all templates" → "Other"
2. `site` 폴더를 드래그
3. Deploy 클릭 → 배포 끝

⚠️ `session-01-part2.m4a`가 34 MB이라 Cloudflare Pages의 25 MB 단일 파일 제한에 걸립니다.
필요하면 다음 중 한 가지로:
- **분할**: 파일을 30분씩 나눠서 업로드 (예: `ffmpeg -i in.m4a -f segment -segment_time 1800 -c copy out_%02d.m4a`)
- **외부 호스팅**: 녹음 파일만 Cloudflare R2 / Google Drive / Dropbox 직링크로 빼기
- **Vercel 사용**: Vercel은 단일 파일 100 MB까지 OK

---

## ➕ 새 회차 추가하는 법 (3 STEP)

### STEP 1. 새 세션 페이지 만들기

`sessions/session-01.html`을 복사해서 `sessions/session-02.html`로:

```bash
cp sessions/session-01.html sessions/session-02.html
```

`session-02.html`에서 다음 부분 수정:
- `<title>` — 회차 번호와 주제 바꾸기
- `SESSION 01 · 2026. 06. 17` → 새 회차 번호와 날짜
- `<h1>` 안의 메인 카피
- 본문 12개 섹션 내용 갈아끼우기
- `recordings/session-02-*.m4a`, `pdfs/...` 등 파일 경로

### STEP 2. 녹음 파일 추가

음성 메모를 `recordings/`에 넣고 압축:

```bash
ffmpeg -i "원본녹음.m4a" -ac 1 -b:a 64k -movflags +faststart recordings/session-02-part1.m4a
```

### STEP 3. 메인 페이지에 카드 추가

`index.html`의 `<div class="session-list">` 안에 새 카드를 **맨 위에** 추가:

```html
<a class="session-card" href="sessions/session-02.html">
  <div class="num-block">
    <div class="num">02</div>
    <div class="label">SECOND</div>
  </div>
  <div>
    <div class="date">2026. 07. 01  ·  화요일  ·  을지로 카인드오브썸머</div>
    <div class="title">두 번째 모임 제목</div>
    <div class="summary">한 줄 요약…</div>
    <div class="meta">
      <span class="badge recording">🎙 녹음 90분</span>
      <span class="badge recap">📋 회고</span>
      <span class="badge pdf">📄 PDF 2개</span>
      <span class="badge">🍵 5잔의 차</span>
      <span class="badge">👥 8명</span>
    </div>
  </div>
</a>
```

그리고 상단 통계도 같이 업데이트:
- `<div class="num">1</div>` → `<div class="num">2</div>` (회차 수)
- `90+` → `180+` (분 누적)
- 등등

---

## 🎨 디자인 노트

- **컬러**: 키즈러닝랩 브랜드 컬러 (네이비 · 코랄 · 스카이 · 핑크 · 민트 · 옐로우)
- **폰트**: Pretendard (한글) · Helvetica (영문 라벨)
- **반응형**: 모바일 · 태블릿 · 데스크톱 모두 OK
- **테마**: 차 한 잔의 따뜻함 + 어른의 진지함

---

## 🔒 저작권 / 개인정보

- 본 사이트의 콘텐츠는 키즈러닝랩(Kids Running Lab)에 저작권이 있어요
- 참석자 본인의 학습 목적으로만 사용
- 외부 공유 · 재배포 · 상업적 이용 · SNS 무단 게시 금지
- **개인정보 유의**: 자기소개에 실명이 들어있어요. 외부에 공개할 거면 익명화하거나
  참석자 동의를 받은 후 게시해 주세요.

---

📩 문의 · 다음 자리 알림: **Instagram @kidsrunning_lab DM**
