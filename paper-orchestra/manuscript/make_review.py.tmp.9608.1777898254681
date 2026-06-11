import sys, io
from docx import Document
from docx.shared import Pt, Cm, RGBColor
from docx.enum.text import WD_ALIGN_PARAGRAPH
from docx.oxml.ns import qn

doc = Document()

section = doc.sections[0]
section.top_margin = Cm(2.5)
section.bottom_margin = Cm(2.5)
section.left_margin = Cm(3.0)
section.right_margin = Cm(3.0)

style = doc.styles['Normal']
style.font.name = '맑은 고딕'
style.font.size = Pt(10.5)
style.element.rPr.rFonts.set(qn('w:eastAsia'), '맑은 고딕')

def set_run(run, bold=False, size=10.5, color=None, italic=False):
    run.font.name = '맑은 고딕'
    run.font.size = Pt(size)
    run.font.element.rPr.rFonts.set(qn('w:eastAsia'), '맑은 고딕')
    run.bold = bold
    run.italic = italic
    if color:
        run.font.color.rgb = RGBColor(*color)

def h(doc, text, level=1, color=None):
    p = doc.add_paragraph()
    run = p.add_run(text)
    sizes = {1: 14, 2: 12, 3: 11}
    set_run(run, bold=True, size=sizes.get(level, 11), color=color)
    p.paragraph_format.space_before = Pt(12)
    p.paragraph_format.space_after = Pt(5)
    return p

def para(doc, text, bold=False, indent=False, size=10.5, color=None, italic=False):
    p = doc.add_paragraph()
    run = p.add_run(text)
    set_run(run, bold=bold, size=size, color=color, italic=italic)
    if indent:
        p.paragraph_format.left_indent = Cm(0.8)
    p.paragraph_format.space_after = Pt(4)
    return p

def bullet(doc, text, sub=False):
    prefix = '    • ' if sub else '• '
    p = doc.add_paragraph()
    run = p.add_run(prefix + text)
    set_run(run, size=10.5)
    p.paragraph_format.left_indent = Cm(0.5 if not sub else 1.0)
    p.paragraph_format.space_after = Pt(3)
    return p

# ===== 제목 =====
t = doc.add_paragraph()
t.alignment = WD_ALIGN_PARAGRAPH.CENTER
r = t.add_run('논문 심사 보고서')
set_run(r, bold=True, size=18)
t.paragraph_format.space_after = Pt(4)

s = doc.add_paragraph()
s.alignment = WD_ALIGN_PARAGRAPH.CENTER
r2 = s.add_run('Peer Review Report')
set_run(r2, size=11, color=(100, 100, 100))
s.paragraph_format.space_after = Pt(14)

doc.add_paragraph('─' * 58)

# ===== 기본 정보 표 =====
tbl = doc.add_table(rows=5, cols=2)
tbl.style = 'Table Grid'
rows_data = [
    ('논문 제목', 'AI추출 생리심리 시계열 데이터 기반 학습자 유형별 자기조절학습의 차이 분석'),
    ('심사일', '2026년 5월 4일'),
    ('심사자', '익명 심사자 (Blind Review)'),
    ('투고 학술지', '교육심리연구 (Korean Journal of Educational Psychology)'),
    ('최종 판정', '수정 후 재심 (Major Revision Required)'),
]
for i, (lbl, val) in enumerate(rows_data):
    c0, c1 = tbl.rows[i].cells[0], tbl.rows[i].cells[1]
    p0 = c0.paragraphs[0]
    r0 = p0.add_run(lbl)
    set_run(r0, bold=True, size=10)
    p1 = c1.paragraphs[0]
    r1 = p1.add_run(val)
    clr = (180, 50, 50) if i == 4 else None
    set_run(r1, bold=(i == 4), size=10, color=clr)

doc.add_paragraph('')

# ===== 1. 총평 =====
h(doc, '1. 논문 개요 및 심사자 총평', level=1)
para(doc,
    '본 논문은 AI 기반 학습 케어 플랫폼에서 추출된 생리심리 시계열 데이터(정서, 스트레스, 집중도)를 활용하여, '
    '선행연구에서 도출된 학습자 유형에 따른 자기조절학습(SRL)의 차이를 분석한 연구이다. '
    'AI 추출 생리심리 데이터와 전통적 교육심리 변인을 연결하려는 시도는 학습분석학 분야에서 시의성이 높고, '
    '실제 학습 환경에서 수집된 과정 중심 데이터를 교육심리학적으로 해석하려는 점에서 가치 있는 방향을 제시한다.')
para(doc,
    '연구 결과, 정서·스트레스 기반 유형에서는 자기조절학습의 유의한 차이가 나타나지 않은 반면, '
    '집중도 기반 유형에서는 유형 주효과 및 시기×유형 상호작용 효과가 일부 변인에서 유의하게 나타났다. '
    '이는 집중도의 시계열적 패턴이 자기조절학습 수준과 변화 방향을 설명하는 핵심 변인임을 시사하는 흥미로운 발견이다.')
para(doc,
    '그러나 본 심사자는 아래와 같은 주요 방법론적·개념적 문제들이 논문의 결론 신뢰도를 제한한다고 판단하며, '
    '이를 충분히 해결한 후 재심을 권고한다.')

# ===== 2. 주요 수정 =====
h(doc, '2. 주요 수정 요구사항 (Major Revisions)', level=1)

h(doc, '2-1. 표본 크기의 심각한 제한 [최우선 수정 사항]', level=2)
para(doc,
    '본 연구의 최대 문제는 최종 분석 대상이 N=23에 불과하다는 점이다. '
    '집중도 기반 분석에서 고집중 변동형(코난형) N=9, 저집중 유지형(짱구형) N=14로 분할되어 '
    '집단별 사례 수가 통계적 추론의 신뢰성을 확보하기 어려운 수준이다.')
bullet(doc, '반복측정 분산분석에서 집단간·집단내 요인이 동시에 포함될 경우 각 셀당 최소 10~15명 이상이 권고된다. N=9는 이 기준에 크게 미치지 못한다.')
bullet(doc, '정서·스트레스 기반 유형에서 비유의 결과가 실제 효과 없음 때문인지, 통계적 검정력 부족(Type II error) 때문인지 구분할 수 없다.')
bullet(doc, '보고된 효과크기(예: 행동전략 ηp²=.427)는 소표본에서 과대추정될 가능성이 높다. 사후 검정력 분석(post-hoc power analysis)을 추가해야 한다.')
bullet(doc, '초기 50명 중 27명 탈락(54%)에 대한 탈락자 특성 분석(attrition analysis)이 없어 선택 편향 여부를 확인할 수 없다.')
para(doc,
    '수정 방향: 사후 검정력 분석 결과 제시, 소표본 한계를 제한점에서 구체적으로 기술, 탈락자 특성 분석을 본문 또는 부록에 추가할 것.',
    indent=True)

h(doc, '2-2. 스트레스 지표와 정서 지표 간 구인 독립성 문제', level=2)
para(doc,
    '스트레스 지수와 "화(anger)" 간 상관이 r=.896(p<.001)으로 나타났다. '
    '이는 두 변인이 사실상 동일한 구인을 측정하고 있음을 강하게 시사한다.')
bullet(doc, '스트레스 지수는 Valence < 0이고 Arousal ≥ 0인 상태로 정의되는데, 이는 "화(고각성-부정)" 범주의 정의와 거의 동일하다. 즉 스트레스 지표는 정서 지표의 부분집합(subset)에 해당한다.')
bullet(doc, '동일한 Valence-Arousal 좌표계에서 파생된 두 변인을 별도 독립 변인처럼 취급하는 것은 구인 독립성(construct independence)을 위반할 수 있다.')
bullet(doc, '저자들이 제한점에서 이 문제를 간략히 언급하고 있으나, 그 심각성에 비해 논의가 지나치게 짧다.')
para(doc,
    '수정 방향: 정서와 스트레스 지표의 측정 독립성 문제를 이론적 배경 또는 연구방법 섹션에서 명시적으로 다루고, '
    '이것이 결과 해석에 미치는 영향을 심층 논의할 것.',
    indent=True)

h(doc, '2-3. 선행연구(박사학위논문) 기반 유형 적용의 방법론적 명확성 부족', level=2)
para(doc,
    '본 연구는 선행연구("제1저자, 2026")에서 도출된 학습자 유형을 그대로 적용하였다. '
    '이 선행연구는 동일 저자의 박사학위논문으로 추정된다.')
bullet(doc, '선행연구와 본 연구의 데이터셋이 동일한지, 일부 중첩되는지, 완전히 별개인지 명시해야 한다. 동일 데이터셋 사용 시 유형 도출과 차이 분석을 동일 데이터에서 수행하는 것은 확인적 연구와 탐색적 연구의 경계를 흐린다.')
bullet(doc, '유형이 이미 도출된 데이터에서 해당 유형과 종속변인의 관계를 분석하면 과적합(overfitting)의 위험이 있어 외적 타당도가 저해된다.')
bullet(doc, '표 4에서 학습자 유형 기준 기술이 표 안팎에 혼재되어 가독성이 크게 저하된다. 표를 재구성할 것.')
para(doc,
    '수정 방향: 선행연구와 본 연구의 데이터 관계를 연구방법 섹션에서 명확히 기술하고, 동일 데이터셋 사용 시 한계를 명시할 것.',
    indent=True)

h(doc, '2-4. 반직관적 상관관계에 대한 해석 부재', level=2)
para(doc,
    '기초통계 결과(표 5)에서 이론적 설명이 필요한 반직관적 상관이 나타났으나 저자들이 이를 논의하지 않았다.')
bullet(doc, '집중도 × 지루함: r=.610(p<.01) — 집중도가 높을수록 지루함도 높다는 결과는 직관에 반한다. 고집중 상태와 지루함 상태의 공존 가능성, 또는 측정 방식의 문제에 대한 해석이 필요하다.')
bullet(doc, '지루함 × 자기조절학습: r=.490(p<.05) — 지루함이 높을수록 SRL 수준도 높다는 결과는 Pekrun(2006)의 성취정서이론과 상충한다. 이에 대한 이론적 설명이 요구된다.')
bullet(doc, '집중도와 즐거움(r=.591)보다 집중도와 지루함(r=.610)의 상관이 더 크게 나타난 이유를 설명해야 한다.')
para(doc,
    '수정 방향: 논의 섹션에서 이러한 반직관적 상관의 가능한 원인(AI 모델 특성, 집단 이질성, 측정 오류 가능성 등)을 체계적으로 논의할 것.',
    indent=True)

# ===== 3. 보통 수정 =====
h(doc, '3. 보통 수정 요구사항 (Moderate Revisions)', level=1)

h(doc, '3-1. 통계 보고의 완전성', level=2)
bullet(doc, '반복측정 분산분석에서 구형성 가정(sphericity)에 대한 Mauchly 검정 결과를 보고할 것(본 연구는 시기가 2수준이므로 해당 없을 수 있으나, 원칙상 언급 필요).')
bullet(doc, '유의한 주효과 확인 후 사후 검정(post-hoc test, 예: Bonferroni) 결과를 체계적으로 보고해야 한다.')
bullet(doc, '대응표본 t검정 보고 시 Cohen\'s d 효과크기를 함께 제시하면 결과 해석이 강화된다.')

h(doc, '3-2. 집중 가중 지수(focus_weighted_index) 가중치 근거', level=2)
para(doc,
    '집중 시간 70%, 학습 시간 30%의 특정 가중치(7:3)가 어떻게 도출되었는지에 대한 경험적·이론적 근거가 불충분하다. '
    '가중치 결정 과정을 더 명확히 설명하거나, 다른 가중치 조합에 대한 민감도 분석(sensitivity analysis) 결과를 제시할 것.')

h(doc, '3-3. AI 측정 도구의 타당도 정보 부족', level=2)
para(doc,
    '대교씨엔에스 AI 모델의 신뢰도·타당도 정보가 충분히 제시되지 않았다. '
    '학습 맥락에서의 얼굴 영상 기반 정서 인식 정확도, 한국 청소년 집단에서의 검증 여부, '
    '환경 변인(조명, 마스크, 카메라 각도 등)의 영향에 관한 정보를 추가해야 한다.')

h(doc, '3-4. 탈락자 분석(Attrition Analysis) 부재', level=2)
para(doc,
    '초기 등록자 50명 중 27명(54%)이 분석에서 제외되었다. '
    '탈락자(N=27)와 최종 분석 대상(N=23) 간 주요 변인(사전 SRL, 성별, 사용일수 등)의 차이를 분석·보고해야 한다. '
    '선택 편향이 확인될 경우 연구 결론의 일반화 범위를 더욱 제한해야 한다.')

# ===== 4. 소수정 =====
h(doc, '4. 소수정 사항 (Minor Revisions)', level=1)

minor = [
    ('표 4 레이아웃 전면 수정',
     '표 4에서 학습자 유형 기준과 시계열 패턴 기술이 표 안팎에 혼재되어 가독성이 크게 떨어진다. '
     '각 유형의 기준·특성이 한눈에 파악될 수 있도록 표를 재구성할 것.'),
    ('학습자 유형 명칭의 학술적 적절성 검토',
     '만화 캐릭터 명칭 사용은 기억술로서 장점이 있으나, 국제 독자 및 학술지 표준 표기와의 적합성을 검토할 것. '
     '공식 명칭(예: 유형 A, B)과 별칭을 병기하는 방식을 권장한다.'),
    ('영문 초록 원어민 교정',
     '영문 초록의 내용은 적절하나 일부 표현이 어색하다. 영어 원어민 또는 전문 교정 서비스 검토를 권장한다.'),
    ('참고문헌 완전성 확인',
     'Pekrun(2017) 참고문헌 정보가 불완전하다("Emotions in late modernity, 142"는 서지정보 불명확). '
     '모든 참고문헌의 완전한 정보를 APA 7판 기준으로 재확인할 것.'),
    ('본문 띄어쓰기 교정',
     '일부 페이지(pp.2-6)에서 어간 띄어쓰기가 누락되어 가독성이 심각하게 저하된다. '
     'PDF 변환 문제일 수 있으나 원고 파일에서 확인하고 교정할 것.'),
    ('연구문제별 결론 명시적 대응',
     '연구문제 1-1(정서), 1-2(스트레스), 1-3(집중도)에 대한 답변을 논의 서두에서 구조적으로 제시할 것.'),
]

for title_m, body_m in minor:
    para(doc, f'▶ {title_m}', bold=True)
    para(doc, body_m, indent=True)
    doc.add_paragraph('')

# ===== 5. 강점 =====
h(doc, '5. 논문의 강점', level=1)
bullet(doc, '주제의 시의성: AI 기반 교육 환경에서 생리심리 데이터를 교육심리 변인과 연결하는 연구 방향은 학습분석학 분야에서 중요한 기여 가능성을 지닌다.')
bullet(doc, '과정 중심 데이터 활용: 시계열 패턴 기반 학습자 유형 적용은 학습의 동적 특성을 포착하는 방법론적 혁신이다.')
bullet(doc, '생태학적 타당도: 실험실이 아닌 실제 AI 자습실에서 수집된 데이터를 활용하여 현장 적용 가능성이 높다.')
bullet(doc, '교육적 함의의 구체성: 저집중 학습자에 대한 맞춤형 개입 설계 방향을 구체적으로 논의하였다.')
bullet(doc, '이론적 근거: Zimmerman(2002), Pekrun(2006), Järvelä & Hadwin(2024) 등 핵심 이론과의 연결이 적절하다.')

# ===== 6. 최종 판정 =====
h(doc, '6. 최종 판정 및 권고사항', level=1)

pv = doc.add_paragraph()
rv = pv.add_run('최종 판정: 수정 후 재심 (Major Revision Required)')
set_run(rv, bold=True, size=12, color=(180, 50, 50))
pv.paragraph_format.space_after = Pt(8)

para(doc,
    '본 논문은 연구 주제의 참신성과 학문적 기여 가능성을 인정하나, 표본 크기의 심각한 제한, '
    '스트레스-정서 지표의 구인 독립성 문제, 선행연구 데이터 관계의 불명확성, '
    '반직관적 상관관계에 대한 해석 부족 등 핵심적인 방법론적 문제가 해결되어야 '
    '게재 적합성을 갖출 수 있다고 판단된다.')

para(doc, '저자에게 다음을 권고한다:', bold=True)
bullet(doc, '[필수] 사후 검정력 분석 실시 및 결과 보고')
bullet(doc, '[필수] 스트레스-정서 지표 간 측정 독립성 문제에 대한 심층 논의')
bullet(doc, '[필수] 선행연구(박사논문)와 본 연구의 데이터 관계 명확히 기술')
bullet(doc, '[필수] 반직관적 상관 결과(지루함↑→SRL↑, 집중도↑→지루함↑)에 대한 해석 제시')
bullet(doc, '[필수] 탈락자 특성 분석(attrition analysis) 추가')
bullet(doc, '[권고] 표 4 레이아웃 전면 수정')
bullet(doc, '[권고] 영문 초록 원어민 교정')
bullet(doc, '[권고] 연구문제별 결론 명시적 대응 서술')

doc.add_paragraph('')
para(doc, '수정 원고 제출 시 심사자 코멘트에 대한 응답서(Response to Reviewers)를 함께 제출해 주시기 바랍니다.')

doc.add_paragraph('')
doc.add_paragraph('─' * 58)
pe = doc.add_paragraph()
pe.alignment = WD_ALIGN_PARAGRAPH.RIGHT
re = pe.add_run('심사 완료일: 2026년 5월 4일  |  익명 심사자')
set_run(re, size=9, color=(120, 120, 120))

out = 'E:/Dropbox/100 Claude/paper-orchestra/manuscript/심사보고서_20260504.docx'
doc.save(out)
print('저장 완료:', out)
