# 세젤귀 정책 페이지 외부 호스팅

앱인토스 콘솔 등록 시 외부 URL 로 약관·개인정보처리방침을 입력해야 해요. 이 폴더의 4개 정적 HTML 을 어디든 올리면 즉시 사용 가능합니다.

## 가장 빠른 방법 3가지

| 방법 | 비용 | 소요시간 | URL 모양 |
| --- | --- | --- | --- |
| **GitHub Pages** (추천) | 무료 | 5분 | `https://username.github.io/repo/terms.html` |
| Supabase Storage | 무료 | 3분 | `https://xxx.supabase.co/storage/v1/object/public/policies/terms.html` |
| Netlify Drop | 무료 | 2분 | `https://random-name.netlify.app/terms.html` |

## 옵션 A. GitHub Pages (추천)

**왜 추천**: 깔끔한 URL, 검수 통과율 높음, 운영자가 git push 로 갱신.

### 1) 새 GitHub 레포 만들기

브라우저에서 https://github.com/new
- Repository name: `sezelg-policies`
- Public ✅
- README 추가 ✅
- **Create repository**

### 2) public-pages 폴더 푸시

PowerShell:
```powershell
cd d:\oneshotcalorie\sezelg\public-pages
git init
git add .
git commit -m "policy pages v1"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/sezelg-policies.git
git push -u origin main
```

### 3) GitHub Pages 활성화

레포 페이지에서:
- **Settings → Pages**
- **Source: Deploy from a branch**
- **Branch: main / (root)** → **Save**
- 1~2분 후 상단에 `Your site is live at https://YOUR_USERNAME.github.io/sezelg-policies/` 표시

### 4) URL 확인

| 페이지 | 콘솔 입력 URL |
| --- | --- |
| 메인 | `https://YOUR_USERNAME.github.io/sezelg-policies/` |
| 이용약관 | `https://YOUR_USERNAME.github.io/sezelg-policies/terms.html` |
| 개인정보처리방침 | `https://YOUR_USERNAME.github.io/sezelg-policies/privacy.html` |
| 커뮤니티 정책 | `https://YOUR_USERNAME.github.io/sezelg-policies/community-rules.html` |

콘솔의 약관 / 개인정보처리방침 URL 입력란에 위 URL 복붙.

### 갱신 방법

내용 수정하면:
```powershell
cd d:\oneshotcalorie\sezelg\public-pages
git add .
git commit -m "약관 v1.1 — XX 조항 변경"
git push
```
GitHub Pages 가 1~2분 내 자동 반영.

---

## 옵션 B. Supabase Storage

이미 만들어둔 `diary-photos` 와 별도로 `policies` 버킷 새로 만들어 사용.

### 1) Storage 버킷 생성

대시보드 → **Storage → New bucket**:
- Name: `policies`
- Public ✅
- File size limit: 500 KB

### 2) 4개 HTML 업로드

대시보드 → **Storage → policies → Upload file**:
- `index.html`
- `terms.html`
- `privacy.html`
- `community-rules.html`
- `_shared.css`

### 3) URL 확인

각 파일의 **public URL** 은:
```
https://cialcumbuajaqiyaiyxm.supabase.co/storage/v1/object/public/policies/terms.html
```

> ⚠️ 단점: Supabase URL 이 기억하기 어렵고 길어요. 검수자에게 깔끔한 인상이 덜함.

---

## 옵션 C. Netlify Drop (가장 빠름)

git 도 안 쓰고 드래그-앤-드롭으로 즉시 호스팅.

1. https://app.netlify.com/drop 에 접속
2. `public-pages` 폴더 통째로 드래그
3. 30초 후 `https://random-name.netlify.app/` URL 받음
4. 무료 계정 가입 후 **Site settings → Change site name** 으로 도메인 변경 (예: `sezelg-policies.netlify.app`)

---

## 검수 직전 체크리스트

업로드 완료하면 한 번씩 직접 열어 확인:

- [ ] `index.html` 메인 페이지가 떠야 함
- [ ] 3개 정책 페이지 링크가 모두 동작
- [ ] 모바일 화면에서 가독성 OK
- [ ] 한글 폰트 깨지지 않음 (Pretendard CDN 정상 로드)
- [ ] 페이지 하단에 사업자 정보 (216-27-02162) 표시
- [ ] 개인정보처리방침 §10 보호책임자 정보 정확

---

## 운영자 정보 일관성

`public-pages/*.html` 안에 다음 정보가 박혀 있어요. 변경할 일 있으면 4개 HTML 파일에서 모두 갱신:

- `한컷칼로` (사업자명)
- `오세진` (대표자명)
- `216-27-02162` (사업자등록번호)
- `sejinoh127@gmail.com` (지원 이메일)
- `danieloh2006@naver.com` (개인정보 보호책임자 이메일)

앱 측 [`src/lib/operatorInfo.ts`](../src/lib/operatorInfo.ts) 와 동일한 값. 둘 다 갱신해야 검수 시 정합성 통과.

---

## 향후 개선 (선택)

- 도메인 연결 (예: `policies.sezelg.com`) → CNAME 설정
- 다국어 지원 (영어 버전)
- 변경 이력 페이지 추가
