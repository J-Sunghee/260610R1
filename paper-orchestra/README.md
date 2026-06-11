# Paper Orchestra - Reviewer Edition

학술논문을 4개 전문 서브에이전트로 자동 검토·수정하는 Claude Code 워크플로우입니다.

## 빠른 시작

### 1. 폴더 구조 확인
```
paper-orchestra/
├── .claude/
│   ├── agents/
│   │   ├── outline-checker.md     ✅ 구조 검토
│   │   ├── literature-checker.md  ✅ 인용 검증
│   │   ├── writer.md              ✅ 문장 다듬기
│   │   └── reviewer.md            ✅ 동료심사
│   └── reports/                    (검토 보고서 자동 저장)
├── manuscript/                     (원고 넣는 곳)
├── references/                     (인용할 논문 PDF 넣는 곳)
├── guidelines/                     (학술지 가이드라인 넣는 곳)
├── CLAUDE.md                       (오케스트레이터 지시사항)
└── README.md                       (이 파일)
```

### 2. 자료 준비
- `manuscript/` 폴더에 원고를 `draft.md` 또는 `draft.docx`로 저장
- `references/` 폴더에 인용할 논문 PDF/MD 파일 복사
- `guidelines/` 폴더에 투고 학술지의 author guidelines 저장 (선택)

### 3. Claude Code 실행
```bash
cd paper-orchestra
claude
```

### 4. 명령 예시

**전체 검토 (병렬 실행)**:
```
> outline-checker, literature-checker, reviewer를 병렬로 실행해서 종합 검토해줘. 
  학술지는 ETR&D 기준으로.
```

**개별 검토**:
```
> literature-checker로 references/ 기준 인용 검증해줘
> reviewer 에이전트로 JCAL 기준 peer review 보고서 작성해줘
```

**수정 적용**:
```
> 위 검토 보고서들 반영해서 writer가 원고 수정해줘
```

**2차 라운드**:
```
> 수정된 원고를 reviewer에게 다시 검토 받아줘
```

## 추천 워크플로우

```
[Round 1]
  └─ 병렬: outline-checker + literature-checker + reviewer
  └─ writer가 통합 수정
  └─ git commit

[Round 2]  
  └─ reviewer 재검토
  └─ 미해결 항목만 writer가 추가 수정
  └─ git commit
```

## 팁

- **Git 사용 권장**: `git init` 후 매 라운드마다 commit하면 변경 이력 추적 가능
- **백업 자동 생성**: writer는 첫 수정 전 `draft_original.md`로 자동 백업
- **로그 확인**: `manuscript/changelog.md`에서 모든 수정 이력 확인 가능
