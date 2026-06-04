# 연차 계산기 — GitHub Pages 배포 가이드

근로기준법 기준 연차 계산기 (입사일 기준). 단일 HTML 파일, 의존성 없음.

---

## 배포 방법 (gh CLI)

```bash
cd annual-leave-calculator

git init
git add index.html DEPLOY.md
git commit -m "feat: 연차 계산기 초기 배포 + 퍼펙트 근태관리 배너"

gh repo create annual-leave-calculator --public --source=. --push

# GitHub Pages 활성화 (main 브랜치 루트)
gh api -X POST repos/khj873-hub/annual-leave-calculator/pages \
  -f "source[branch]=main" -f "source[path]=/"
```

배포 URL: `https://khj873-hub.github.io/annual-leave-calculator/`
(반영까지 1~3분 소요)

---

## 계산 로직

입력: 입사일(joinDate), 기준일(baseDate, 기본=오늘)

- **1년 미만**: 1개월 개근당 1일, 최대 11일
- **1년 이상**: 기본 15일 + 3년 이상부터 2년마다 1일 가산 (상한 25일)
  - 가산일 = `floor((근속연수 - 1) / 2)`

검증:
- 입사일/기준일 미입력 → 에러
- 기준일 < 입사일 → 에러
- 회계연도 기준 산정 시 결과가 달라질 수 있다는 주의문구 포함

---

## 디자인 컨셉

- 톤: 차분한 프리미엄 (이끼색 + 골드 포인트, 종이 질감)
- 폰트: Gowun Batang(제목) + IBM Plex Sans KR(본문)
- CSS 변수: `--moss #2d5a3d`, `--gold #c9a24b`, `--paper #f5f2e9`, `--ink #1a1f1c`
- 카드 중앙 정렬, max-width 540px

---

## 퍼펙트 근태관리 배너

카드 하단에 다크 톤 배너로 https://alba-calculator-production.up.railway.app 유입.
구성: 태그 / 제목 / 설명 / 기능 3개(✓ 연차 자동 산정 · ✓ 출퇴근 기록 · ✓ 급여 자동 정산) / 골드 CTA 버튼
