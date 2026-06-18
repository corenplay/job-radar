# 건설 경영·전략·공무 채용 레이더 (신재호)

중견 건설사 경영기획·전략·감사, 본사 공무, 디벨로퍼·PMC 사업관리 포지션을 한 보드에서 추적하는 정적 웹 대시보드입니다. 외부 서버 없이 동작하며(오프라인), 지원 여부·메모는 브라우저(localStorage)에 저장됩니다.

## 구성
- `index.html` — 레이더 본체 (단일 파일, 외부 의존 없음)
- 공고 데이터는 `index.html` 안 `<script id="jobs-data">` JSON에 내장

## 공고 추가/교체 방법
1. `26.06.17_채용레이더 웹검색 프롬프트_sung.md` 를 웹 검색 가능한 Claude에 붙여넣어 공고를 수집합니다.
2. 돌려받은 JSON 배열을 `index.html` 의 `<script id="jobs-data">` 안 `"jobs": [ ... ]` 에 붙여넣습니다.
3. `"meta": { "last_collected": "YYYY-MM-DD" }` 날짜를 갱신합니다.
4. 저장 후 새로고침하면 적합도순으로 정렬·집계됩니다. (현재 3건은 예시/실제 혼합 — `sample-developer-pm` 등 예시는 교체하세요.)

## 적합도(fit_score) 산정 기준 — 0~100
| 항목 | 배점 |
|---|---|
| 직무 정합 (전략·기획·감사 35 / 공무·원가 30 / 사업관리·PM·CM 28 / BD 25) | 35 |
| 도메인 정합 (중견+ 건설사 20 / 디벨로퍼·PMC 17 / 건설테크 14) | 20 |
| 자격요건 매핑 (경력연수·전공·MBA 부합) | 15 |
| 안정성·규모 (모회사·재무, 자본잠식 회피) | 10 |
| 지역 (인천·경기남서부 10 / 서울남서부 8 / 경기·서울 6) | 10 |
| 리스크 조정 (과스펙·단기재직·직급 불일치) | -10~0 |

- 티어: 최상 85+ / 상 75~84 / 중 65~74 / 하 65 미만
- 전략 티어(`tier`): **A** 건설 본사 공무·감사 / **B** 디벨로퍼·PMC·전략기획 / **C** 스타트업·건설테크

## GitHub Pages 배포

### 방법 A — 웹 UI (가장 간단)
1. GitHub에서 새 저장소 생성 (예: `job-radar`, Public).
2. 이 폴더의 `index.html` (선택사항 `README.md`) 을 업로드(Add file → Upload files) 후 Commit.
3. 저장소 **Settings → Pages → Build and deployment → Source: Deploy from a branch → Branch: `main` / `/ (root)`** 저장.
4. 1~2분 후 `https://<사용자명>.github.io/job-radar/` 에서 열립니다.

### 방법 B — git CLI
```bash
cd "이 폴더"
git init
git add index.html README.md
git commit -m "build: 채용 레이더 v1"
git branch -M main
git remote add origin https://github.com/<사용자명>/job-radar.git
git push -u origin main
# 이후 Settings → Pages 에서 main / root 지정
```

### 참고
- 단일 HTML 파일이라 Pages가 별도 빌드 없이 그대로 서빙합니다.
- 공고를 갱신할 때마다 `index.html` 만 다시 커밋·푸시하면 됩니다.
- 공개 저장소이면 URL을 아는 사람은 누구나 볼 수 있습니다. 회사·연봉 메모 등 민감 정보는 공고 데이터에 넣지 마세요. 비공개로 두려면 Private 저장소 + GitHub Pages(Pro) 또는 로컬에서 `index.html` 더블클릭으로 사용하세요.
