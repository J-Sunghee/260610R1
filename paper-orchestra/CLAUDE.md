# Paper Orchestra - Reviewer Edition

이 프로젝트는 학술논문 원고를 4개의 전문 서브에이전트로 검토·수정하는 시스템입니다.

## 에이전트 구성

| 에이전트 | 역할 |
|---------|------|
| outline-checker | 연구제목·연구문제 대비 논문 구조 정합성 검토 |
| literature-checker | references/ 폴더 기준 인용 검증 (APA 7판) |
| writer | 검토 의견 반영하여 학술적 문장으로 수정 |
| reviewer | 학술지 가이드라인 기반 동료심사 보고서 작성 |

## 워크플로우

### 전체 검토 모드
사용자가 "전체 검토 시작" 또는 "full review"를 요청하면 다음 순서로 실행:

1. **outline-checker** 호출 → 구조 보고서 생성
2. **literature-checker** 호출 → 인용 검증 보고서 생성
3. **reviewer** 호출 → 동료심사 보고서 생성
4. 위 3개 보고서를 종합하여 **writer**에게 전달, 원고 수정 적용
5. (선택) 수정 후 다시 **reviewer** 호출하여 개선 여부 확인

⚡ **병렬 실행 권장**: outline-checker, literature-checker, reviewer는 서로 의존성이 없으므로 동시에 호출 가능합니다.

### 개별 호출 모드
사용자가 특정 에이전트만 지정하면 해당 에이전트만 호출합니다.

예시:
- "citation check만 해줘" → literature-checker만 실행
- "구조만 점검해줘" → outline-checker만 실행
- "ETR&D 기준으로 peer review 해줘" → reviewer만 실행

## 파일 규약

| 경로 | 용도 |
|------|------|
| `manuscript/draft.md` (또는 .docx) | 검토 대상 원고 |
| `manuscript/draft_original.md` | 원본 백업 (writer가 첫 수정 전 자동 생성) |
| `manuscript/changelog.md` | 수정 변경 로그 |
| `references/` | 인용할 논문 PDF/MD 파일 |
| `guidelines/` | 학술지 author guidelines |
| `.claude/reports/YYYYMMDD_<agent>.md` | 각 에이전트의 검토 보고서 |

## 언어 규칙
- 사용자(한밭대학교 교수)와의 대화는 **한국어**로 진행
- 원고가 영어이면 검토 보고서도 영어로, 한국어 원고면 한국어로 작성
- 학술 용어는 원어 병기 (예: "이론적 정합성(theoretical coherence)")

## 학술지 기본 설정
별도 지정이 없으면 다음 학술지 기준을 따름:
- **국제 SSCI**: ETR&D, Computers & Education, BJET, JCAL
- **국내 KCI**: 교육공학연구, 교육정보미디어연구

사용자가 학술지를 지정하면 guidelines/ 폴더에서 해당 가이드를 우선 참조.

## 보고서 저장 정책
- 모든 에이전트 보고서는 `.claude/reports/`에 누적 저장
- 파일명에 날짜 포함하여 라운드별 비교 가능
- 동일 날짜 다회 검토 시 `_v2`, `_v3` 접미사 사용

## 사용자 컨텍스트
- 한밭대학교 에듀테크 전공 교수
- 주요 협업: 하와이대학교 마노아 캠퍼스 연구진
- 주 연구 영역: AI 활용 교육, 학습분석, 멀티모달 학습설계
- 한국어 의사소통 선호, 한·영 학술 글쓰기 모두 지원 필요
