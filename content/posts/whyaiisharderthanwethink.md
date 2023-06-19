---
title: "Why AI is Harder Than We Think - Melanie Mitchell"
date: 2023-06-16T16:23:40+09:00
draft: false
---

멜라니 미첼의 논문 [Why AI is Harder Than We Think](https://arxiv.org/pdf/2104.12871.pdf) 를 번역해 보았습니다. 정확한 publish 날짜는 모르겠으나 arxiv 주소를 보면 2021년 4월쯤 공개된 것으로 보입니다.

---

## AI가 생각보다 어려운 이유

멜라니 미첼, 산타 페 연구소, mm@santafe.edu

### 초록

인공지능 분야는 그것이 태동한 1950년대 이래로 "AI spring" 이라 불리는 낙관적인 예측과 대규모 투자의 시기, 그리고 "AI winter" 라 불리는 실망과 신뢰 상실, 투자 축소의 시기를 반복적으로 겪어 왔습니다. 오늘날 매우 빨라 보이는 AI 혁신의 속도에도 불구하고, 자율주행차, 가사 로봇, 대화형 동반자 등 오랫동안 약속된 기술의 개발은 많은 사람들의 예상보다 훨씬 더 어려운 것으로 밝혀졌습니다. 이러한 현상이 반복되는 이유 중 하나는 지능 자체의 본질과 복잡성에 대한 우리의 이해가 제한되어 있기 때문입니다. 이 논문에서는 인공지능 연구자들이 이 분야의 미래에 대해 과도한 예측을 하게 만드는 네 가지 오류에 대해 설명합니다. 그리고 이러한 오류가 촉발한 여러 열린 문제들에 대해 (기계에 인간의 상식을 부여하는 오래된 문제를 포함하여) 논의하며 글을 마무리합니다.

### 서론

예정대로였다면 2020년은 자율 주행 자동차의 도래를 알리는 해여야 했습니다. 5년 전 가디언의 헤드라인은 "2020년부터 당신은 영구적인 뒷좌석 운전자가 될 것" 이라고 예측했습니다[1]. 2016년 비즈니스 인사이더는 "2020년까지 1,000만 대의 자율주행차가 도로를 달릴 것" 이라고 확언했습니다[2]. 테슬라 모터스의 CEO Elon Musk는 2019년에 "지금부터 1년 뒤에는 완전 자율 주행, 소프트웨어... 모든 것을 갖춘 자동차가 100만 대 이상 보급될 것" 이라고 약속했습니다[3]. 그리고 2020년은 여러 자동차 회사들이 자율 주행 자동차를 시장에 출시하기로 공표한 년도였습니다[4, 5, 6].

"완전 자율 주행" 을 재정의하려는 시도들에도 불구하고[7], 앞선 예측들 중 실현된 것은 하나도 없습니다. AI 시스템, 특히 자율주행차에 대한 지나친 낙관론이 잘못된 것으로 판명될 때 어떤 일이 벌어질 수 있는지에 대해 AI 전문가 Drew McDermott의 말을 인용해 볼 필요가 있습니다:

> 어쩌면 현재 기대치들은 너무 높고... 이건 결국 재앙을 초래할 수도 있습니다. 지금부터 5년 후 자율주행차가 출시되지 못하면서 투자 자금이 비참하게 무너진다고 가정해 봅시다. 모든 스타트업 기업이 실패할 것입니다. 그리고 이에 대한 반동으로 AI 와 연결된 그 어떤 것이든 투자를 받지 못하게 될 것입니다. 모두가 자신의 연구 프로젝트의 이름을 다른 것으로 바꿀 것입니다. 이런 상황을 "AI winter" 라고 부릅니다[8].

가장 주목할 만한 점은 McDermott의 경고가 1984년에 이루어졌다는 점입니다. 당시엔 지금처럼 기계 지능의 미래에 대한 확신에 찬 낙관론이 넘쳐나고 있었습니다. McDermott 은 이 분야의 주기적 패턴에 대해 글을 쓰고 있었습니다. 새롭고 명백한 돌파구가 나타나면 AI 실무자들은 빠른 발전과 성공적인 상용화, 그리고 "진정한 AI" 의 단기적인 전망을 예측하게 됩니다. 정부와 기업은 열기에 휩싸여 이 분야에 연구 개발 자금을 쏟아 붓게 되고, AI의 봄이 피어나게 됩니다. 그러다 진전이 멈추면 열정과 자금, 일자리가 모두 말라버리고, AI 겨울이 찾아옵니다. 실제로 McDermott의 경고가 있은지 약 5년 후, 새로운 AI 겨울이 시작되었습니다.

이 장에서는 인공지능의 미래에 대한 과신과 실망이 반복되는 이유를 살펴봅니다. 저는 대중, 미디어 , 심지어 전문가들 사이의 지나친 낙관론이 우리가 AI 에 대해 말하는 방식과 지능의 본질에 대한 우리의 직관에 내재되어 있는 몇가지 오류로부터 비롯한다고 주장합니다. 이러한 오류와 그 미묘한 영향을 이해하면 보다 강력하고 신뢰할 수 있으며 실제로 *지능적인* AI 시스템을 만들기 위한 방향을 제시할 수 있습니다.

### 봄과 겨울

AI의 미래에 대한 과신은 이 분야만큼이나 오래되었습니다. 예를 들어, 1958년 뉴욕 타임즈는 미 해군이 프랭크 로젠블랫의 "퍼셉트론" (오늘날 심층 신경망의 초보적인 선구자) 을 시연했다고 보도한 바 있습니다: "해군은 오늘 걷고, 말하고, 보고, 쓰고, 스스로 재생산하고, 자신의 존재를 의식할 수 있을 것으로 기대되는 전자 컴퓨터의 배아를 공개했습니다." [9]. 이러한 낙관적인 전망은 곧이어 AI 선구자들의 비슷한 선언으로 이어졌는데, 이번에는 논리 기반의 "symbolic" AI의 가능성에 관한 것이었습니다. 1960년 허버트 사이먼은 "기계는 20년 안에 사람이 할 수 있는 모든 일을 할 수 있게 될 것" 이라고 선언했습니다[10]. 이듬해 클로드 섀넌은 이 예측을 반복했습니다: "10년 또는 15년 이내에 공상 과학 소설의 로봇과 크게 다르지 않은 무언가가 실험실에서 나올 것이라고 자신 있게 예상합니다." [11]. 그리고 몇 년 후 마빈 민스키는 "한 세대 안에... '인공 지능'을 만드는 문제는 상당 부분 해결될 것" 이라고 예측했습니다[12].

이러한 예측들에서 엿볼 수 있는 1960년대와 1970년대 초의 낙관적인 인공지능의 봄은 곧 첫 번째 인공지능의 겨울로 이어졌습니다. 민스키와 페퍼트의 1969년 저서 [13]은 로젠블랫의 퍼셉트론으로 해결할 수 있는 문제가 매우 제한적이라는 것을 보여주었습니다. 1973년 영국의 라이트힐 보고서[14]와 미국 국방부의 "미국 연구 그룹" 보고서는 각국 정부의 의뢰를 받아 가까운 미래의 인공지능에 대한 전망을 평가했는데, 두 보고서 모두 이러한 전망에 대해 극도로 부정적이었습니다. 이로 인해 두 나라 모두 AI에 대한 자금이 급격히 감소 하고 열기가 식었습니다.

1980년대 초부터 분야에 새로운 희망을 불러온 여러 이니셔티브와 함께 AI에 대한 열기가 다시 한번 고조되기 시작했습니다: 산업계에서 "전문가 시스템"[15]의 부상, 차세대 컴퓨팅 시스템의 핵심이라는 야심찬 AI 능력을 목표로 한 일본의 "5세대" 프로젝트[16]에 대한 막대한 투자, 일반 AI로의 발전을 위해 대규모 자금을 제공한 미국의 "전략적 컴퓨팅 이니셔티브"[17], 신경망에 대한 새로운 노력[18, 19] 등등.

1980년대 후반에 이르러 이러한 낙관적인 기대는 모두 무너져 내렸고, 그 어떤 기술도 장밋빛 약속을 달성 하지 못했습니다. 특정 도메인에 대한 전문 지식을 포착하는 규칙들을 만들기 위해 인간에 의존하는 전문가 시스템은 취약한 것으로 드러났습니다. 새로운 상황에 직면했을 때 일반화하거나 적응하지 못했기 때문입니다. 문제는 규칙을 작성하는 인간 전문가가 실제로는 시스템 프로그래밍의 일부가 아닌 무의식적 지식, 즉 우리가 "상식" 이라고 부르는 지식에 의존한다는 것이었습니다. 5세대 프로젝트와 전략적 컴퓨팅 이니셔티브에서 추구한 AI 접근 방식 또한 취약성과 일반성 부족이라는 유사한 문제에 직면했습니다. 1980년대와 1990년 대의 신경망 접근법 역시 비교적 단순한 예제에서는 잘 작동했지만 복잡한 문제로 확장할 수 있는 능력이 부족했습니다. 실제로 1980년대 후반은 새로운 AI의 겨울이 시작되는 시기였고, 이 분야의 명성은 추락했습니다. 제가 1990년에 박사 학위를 받았을 때 입사 지원서에 "인공 지능" 이라는 용어를 사용하지 말라는 조언을 받았습니다.

1956년 다트머스 회의는 인공지능 분야가 잉태된 곳입니다. "Artificial Intelligence" 라는 용어를 처음 만든 AI의 선구자 존 매카시는 다트머스 회의의 50주년을 기념하는 자리에서 이 문제를 간결하게 설명했습니다: "인공지능은 우리가 생각했던 것보다 더 어려웠다" [20].

1990년대와 2000년대에는 데이터로부터 예측 모델을 생성하는 알고리즘을 개발하는 *머신러닝*이 급부상했습 니다. 이러한 접근 방식은 일반적으로 신경 과학이나 심리학보다는 통계학에서 영감을 얻었으며, 일반적인 지능을 포착하기보다는 특정 작업을 수행하는 데 목적이 있었습니다. 머신러닝 실무자들은 당시에는 불신받던 AI 분야와 자신의 분야를 빠르게 구분했습니다.

그러다 2010년경, 뇌에서 영감을 받은 다층 신경망에 데이터를 학습시키는 *딥러닝*이 비주류에서 벗어나 머신러닝의 슈퍼스타로 떠올랐습니다. 심층 신경망은 1970년대부터 사용되어 왔지만, 웹에서 스크랩한 방대한 데이터셋, 고속 병렬 컴퓨팅 칩, 학습 방법의 혁신 등의 등장 덕분에 스케일 업 될 수 있었고 이전에 풀리지 않은 많은 AI 문제들을 풀기 위해 사용될 수 있었습니다. 심층 신경망은 음성 인식, 기계 번역, 챗봇, 이미지 인식, 게임 플레이, 단백질 접기 등 지난 10년간 우리가 목격했던 모든 주요 AI 발전의 원동력입니다.

갑자기 "AI" 라는 용어가 모든 곳에 등장하기 시작했고, 이전에 "일반 AI", "진정한 AI" 또는 "인간 수준의 AI" 등으로 불려온 것의 전망에 대한 새로운 낙관론이 한꺼번에 쏟아졌습니다.

2016년과 2018년에 실시된 AI 연구자 대상 설문조사에서 응답자의 평균 예측은 2040~2060년까지 인간 수준의 AI가 탄생할 확률을 50%로 내다봤지만, 양쪽 방향으로 편차가 컸습니다[21, 22]. 가장 잘 알려진 AI 전문가와 기업가들도 이에 동의하고 있습니다. 널리 사용되는 AI 교과서의 공동 저자인 스튜어트 러셀은 "초지능 AI" 가 "아마도 우리 아이들의 생애에 일어날 것" 이라고 예측하고[23], AI 회사 OpenAI의 CEO인 샘 알트만은 수십 년 안에 컴퓨터 프로그램이 "거의 '모든 것'을 할 것인데, 그것이 포함하는 새로운 과학적 발견에 의해 '모든 것'에 대한 우리의 개념조차 확장될 것" 이라고 예측합니다. [24] 구글 딥마인드의 공동 창업자인 셰인 레그는 2008년에 "2020년대 중반에는 인간 수준의 AI가 등장할 것" 이라고 예측했고[25], 페이스북의 CEO인 마크 주커버그는 2015년에 "페이스북의 향후 5~10년 목표 중 하나는 기본적으로 시각, 청각, 언어, 일반 인지 등 인간의 모든 주요 감각에서 인간 수준을 뛰어넘는 것" 이라고 선언한 바 있습니다[26].

그러나 모든 낙관론에도 불구하고 딥러닝의 지능이라는 외관에 균열이 생기는 데는 그리 오랜 시간이 걸리지 않았습니다. 과거의 모든 AI 시스템과 마찬가지로 딥러닝 시스템도 학습 데이터와 다른 상황에 직면했을 때 예측할 수 없는 오류가 발생하는 취약성을 보일 수 있다는 사실이 밝혀졌습니다. 이러한 시스템은 훈련 데이터에서 통계적 연관성을 학습한 기계가 정답을 맞추기는 해도 잘못된 이유로 정답을 맞추는 *shortcut learning*[27, 28]에 취약하기 때문입니다. 즉, 이러한 기계는 우리가 가르치려는 개념을 학습하는 것이 아니라 훈련 데이터에서 정답에 대한 지름길을 학습하며, 이러한 지름길은 좋은 일반화로 이어지지 않습니다. 실제로 딥러닝 시스템은 학습한 내용을 새로운 상황이나 작업에 적용할 수 있는 추상적인 개념을 학습하지 못하는 경우가 많습니다[29]. 또한 이러한 시스템은 "adversarial perturbation"[30] 공격에 취약하며, 특히 사람에겐 인지될 수 없거나 관련이 없지만 시스템은 오류를 일으키도록 유도하는 가공된 입력 변경에 취약합니다.

심층 신경망의 한계에 대한 광범위한 연구에도 불구하고, 심층 신경망의 취약성과 취약성의 원인은 아직
완전히 밝혀지지 않았습니다. 수많은 매개변수가 있는 이러한 네트워크는 의사 결정 메커니즘이 매우 불투명할 수 있는 복잡한 시스템입니다. 그러나 오류와 적대적 교란에 대한 인간에겐 없는 취약성을 보면
이러한 시스템이 처리하는 데이터를 실제로 *이해*하지 못한다는 것, 적어도 인간적인 의미의 "이해" 는 하지 못한다는 것은 분명해 보입니다. 더 많은 레이어와 학습 데이터를 추가함으로써 이러한 이해를 달성할 수 있는지, 아니면 더 근본적인 문제가 있는지의 여부는 여전히 AI 커뮤니티에서 논쟁의 여지가 있습니다 .

이 글을 쓰는 시점(2021년 중반)에 몇 가지 새로운 딥러닝 접근 방식이 다시 한 번 AI 커뮤니티에서 상당한 낙관론을 불러일으키고 있습니다. 가장 인기 있는 새로운 분야로는 self-supervised (또는 "predictive") 학습[31], meta-learning[32], deep reinforcement learning[33] 을 사용하는 Transformer 아키텍처가 있으며, 이들 각각은 보다 일반적이고 인간과 유사한 AI를 향한 진전으로 언급되고 있습니다. 이러한 혁신과 기타 새로운 혁신이 예비적인 가능성을 보여주었지만, AI의 봄과 겨울의 사이클은 계속될 것으로 보입니다. 이 분야는 비교적 좁은 영역에서 지속적으로 발전하고 있지만 인간 수준의 AI를 향한 길은 여전히 불투명합니다.

다음 섹션들에서는 인간 수준의 AI가 등장할 가능성에 대한 예측이 지능의 본질에 대한 우리 자신의 편견과 이해 부족을 반영한다고 주장할 것입니다. 특히 AI에 대한 우리의 생각에서 가장 핵심적인 네 가지 오류에 대해 설명합니다. 이러한 오류는 AI 커뮤니티에서 잘 알려져 있지만, 전문가들이 내리는 많은 가정들은 여전히 이러한 오류의 희생양이 되고 있으며, "진정한" 지능형 기계의 단기적 전망에 대해 잘못된 자신감을 심어주고 있습니다.

### 오류 1: 좁은 지능은 일반 지능과 연속선상에 있으리라

특정 AI 작업에 대한 발전은 종종 더 일반적인 AI를 향한 "첫 걸음" 으로 묘사됩니다. 체스를 두는 컴퓨터 딥블루는 "AI 혁명의 첫걸음" 으로 환영받았습니다[34]. IBM은 왓슨 시스템을 "컴퓨팅의 새로운 시대인 인지 시스템으로의 첫걸음" 이라고 설명했습니다[35]. OpenAI의 GPT-3 언어 생성기는 "일반 지능을 향한 한 걸음" 이라고 불렸습니다[36].

실제로 사람들은 비록 좁은 영역이긴 하지만 기계가 놀라운 일을 해내는 것을 보면 그 분야가 일반 AI에 한 걸음 더 다가섰다고 생각하는 경우가 많습니다. 철학자 휴버트 드레이퍼스는 이를 여호수아 바 힐렐이 만든 용어를 빌려 "첫걸음 오류" 라고 불렀습니다. 드레이퍼스는 "첫걸음 오류란 컴퓨터 지능에 대한 첫 번째 연구 이래로 우리는 AI라는 연속체를 따라 조금씩 나아가고 있으며, 그 끝에는 AI가 있기 때문에 아무리 사소한 프로그램 개선도 진보로 간주한다는 주장입니다." 라고 설명합니다. 드레이퍼스는 그의 형제이자 엔지니어인 스튜어트 드레이퍼스의 비유를 인용하며 "나무에 올라간 최초의 원숭이가 달 착륙을 향해 나아가고 있다고 주장하는 것과 같다" 고 말합니다[37].

드레이퍼스는 그 전후의 많은 인공지능 전문가들과 마찬가지로 인공지능 발전의 연속선상에서 "예상치 못한 장애물" 은 항상 *상식*의 문제라고 지적했습니다. 이 상식의 장벽에 대해서는 마지막 섹션에서 더 자세히 설명 하겠습니다.

### 오류 2: 쉬운 것은 쉽고 어려운 것은 어려우리라

존 매카시는 "인공지능은 생각보다 어렵다" 고 한탄했지만, 마빈 민스키는 이것이 "쉬운 일이 어렵기 때문" 이라고 설명했습니다[38]. 즉, 인간이 별 생각 없이 하는 일들, 즉 세상을 바라보고 보이는 것을 이해하고, 대화를 계속하고, 붐비는 인도를 다른 사람과 부딪히지 않고 걷는 것 등이 기계에게는 가장 어려운 과제인 것으로 드러났습니다. 반대로 복잡한 수학 문제 풀기, 체스나 바둑과 같은 게임 마스터하기, 수백 개의 언어 간 문장 번역하기 등 인간에게는 매우 어려운 일을 기계에게 맡기는 것이 상대적으로 더 쉬운 경우가 많습니다. 이는 "컴퓨터가 지능 테스트나 체커 게임에서 성인 수준의 성능을 발휘하도록 만드는 것은 비교적 쉬운 반면, 지각과 기동성에 있어서 한 살짜리 아이의 능력을 부여하는 것은 어렵거나 불가능하다" 고 말한 로봇공학자 한스 모라벡의 이름을 딴 "모라벡의 역설" 의 한 형태입니다[39].

이 오류는 이 분야가 태동할 때부터 AI에 대한 사고에 영향을 미쳤습니다. AI의 선구자인 허버트 사이먼은 "인지 에서 흥미로운 모든 것은 100밀리초 수준 (어머니를 인식하는 데 걸리는 시간) 이상에서 일어난다" 고 주장했습니다[40]. 사이먼은 인지를 이해하기 위해 무의식적인 지각 과정에 대해 걱정할 필요가 없다고 말하고 있는 것입니다. 이러한 가정은 이미 인지된 입력에 대한 추론 과정에 초점을 맞추는 대부분의 Symbolic AI 전통에 반영되어 있습니다.

지난 몇십 년 동안 심볼릭 AI 접근 방식은 AI 커뮤니티에서 딥러닝에게 인기를 내어주었는데, 딥러닝은 인지의 문제를 다루긴 합니다. 그러나 이러한 오류의 근간이 되는 가정은 최근 AI에 대한 주장에서 여전히 나타나고 있습니다. 예를 들어, 2016년 기사에서 인용하듯이, 딥러닝의 선구자인 앤드류 응은 무의식적 지각과 사고의 복잡성을 크게 과소평가하는 사이먼의 가정을 그대로 반복한 바 있습니다: "일반적인 사람이 1초도 안 되는 생각으로 정신적 작업을 수행할 수 있다면, 현재 또는 가까운 미래에 AI를 사용하여 이를 자동화할 수 있을 것입니다." [41].

구글 딥마인드의 연구원들은 알파고의 승리에 대해 이야기하면서 바둑을 "가장 도전적인 영역 중 하나" 라고 설명했습니다[42]. 과연 누구에게 도전적이란 말인가요? 심리학자 게리 마커스가 지적했듯이, 인간에게는 쉽지만 AI 시스템에게는 바둑보다 훨씬 더 어려운 영역이 게임을 포함한 여러 분야에 존재합니다. 한 가지 예로 "연기력과 언어 능력과 마음 이론이 필요한" [43] 제스처 게임을 들 수 있는데, 이는 현재 AI가 달성할 수 있는 수준을 훨씬 뛰어넘는 능력입니다.

인공지능은 우리가 생각하는 것보다 어렵습니다. 왜냐하면 우리는 우리 자신의 사고 과정의 복잡성을 거의 의식하지 못하기 때문입니다. 한스 모라벡은 이 역설을 이렇게 설명합니다: "인간 뇌의 크고 고도로 진화한 감각 및 운동 부분에는 세상의 본질과 그 속에서 생존하는 방법에 대한 수십억 년의 경험이 코드화되어 있습니다. 우리가 추론이라고 부르는 고의적인 과정은 인간의 가장 얇은 표면일 뿐이라고 생각합니다. 그것이 효과적인 것은 오직 훨씬 더 오래되고 훨씬 더 강력하지만 일반적으로 무의식적인 감각 운동 지식에 의해 뒷받침되기 때문입니다. 우리는 모두 지각과 운동 영역에서 탁월한 올림픽 선수이며, 너무 탁월한 나머지 어려운 것이 쉽게 보이게 됩니다." [44]. 마빈 민스키는 이를 더 간결하게 표현합니다. "일반적으로 우리는 우리 마음이 가장 잘하는 것이 무엇인지 가장 잘 알지 못한다" [45].

### 오류 3: 희망적인 이름 붙이기의 유혹

"희망적인 이름 붙이기 (wishful mnemonic)" 이라는 용어는 1976년 컴퓨터 과학자 Drew McDermott의 AI에 대한 비평에서 처음 사용되었습니다:

> 인공지능 프로그램에서 단순한 사고방식의 주요 원인은 프로그램과 데이터 구조를 참조하기 위해 "UNDERSTAND" 또는 "GOAL" 과 같은 이름을 사용하는 것입니다. ...만약 어떤 연구자가 프로그램의 메인 루프를 "UNDERSTAND" 라고 부른다면, 그는 (무죄가 입증되기 전까지는) 그저 논점을 피하는 것에 불과합니다. 그는 많은 사람들, 특히 자기 자신을 오도할 수 있습니다. 대신 그는 이 메인 루프를 "G0034" 따위로 이름 붙이고, G0034가 "이해"의 일부를 구현한다는 것을 자신이나 다른 사람에게 확신시킬 수 있는지 확인해 보아야 마땅할 것입니다. 이 요점을 이해하고 나면 다른 AI 연구자들이 사용하는 희망적인 이름들의 비슷한 예시들을 많이 떠올릴 수 있습니다[46].


수십 년이 지난 지금, 인공지능에 관한 연구는 인공지능 프로그램의 동작과 평가를 설명하는 데 사용되는 인간 지능과 관련된 희망적인 용어들로 가득 차 있습니다. 신경망은 뇌에서 느슨하게 영감을 받았지만 큰 차이가 있습니다. 머신러닝 또는 딥러닝 방법은 인간 (또는 인간이 아닌 동물) 의 학습과 실제로 유사하지 않습니다. 실제로 기계가 인간적 의미의 학습을 했다면, 우리는 기계가 학습한 내용을 다른 맥락에서 사용 할 수 있을 것으로 기대할 수 있습니다. 그러나 실제로는 그렇지 않은 경우가 많습니다. 기계 학습에는 transfer learning 이라는 하위 분야가 있는데, 이 분야는 기계가 학습한 내용을 새로운 상황에 어떻게 적용할 수 있는지에 대한 아직 해결되지 않은 문제, 즉 인간 학습에 기본이 되는 능력에 초점을 맞추고 있습니다.

실제로 우리가 기계의 능력에 대해 이야기하는 방식은 그 능력이 실제로 얼마나 일반적인지에 대한 우리의 개념에 영향을 미칩니다. McDermott의 경고에 대한 실제 사례를 의도치 않게 제공한 IBM의 최고 경영진 중 한 명은 "왓슨은 전 세계의 모든 의료 관련 텍스트를 몇 초 만에 *읽을* 수 있다" 고 선언했고[47], IBM 웹 사이트에서는 왓슨 프로그램이 "7개 언어의 문맥과 뉘앙스를 *이해한다*" 고 주장하고 있습니다[48]. DeepMind의 공동 창립자 데미스 하사비스는 "알파고의 *목표*는 최고의 인간 플레이어를 고작 모방하는 것이 아니라 이기는 것" 이라고 말합니다[49]. 그리고 알파고의 수석 연구원인 데이비드 실버는 알파고의 대국 중 하나를 이렇게 설명했습니다: "우리는 알파고가 대국 중에 자신이 얼마나 잘하고 있다고 *생각*하는지 언제든지 물어볼 수 있다. ... 오직 게임의 후반부에 가서야 *알파고는 자신이 이길 것이라고 생각했다*." [50]. (위 인용문들에서 강조는 필자의 것입니다). 

이러한 의인화된 용어는 단순히 약칭일 뿐이라고 주장할 수도 있습니다: IBM 과학자들은 왓슨이 인간처럼 글을 읽거나 이해하지 못한다는 것을 알고 있고, 딥마인드 과학자들은 알파고가 인간처럼 목표나 생각이 없으며, "게임" 또는 "승리" 에 대한 인간과 같은 개념이 없다는 것을 알고 있습니다. 그러나 이러한 약칭은 이러한 결과를 이해하려는 대중과 이를 보도하는 언론에 오해를 불러일으킬 수 있으며, AI 전문가들이 그들의 시스템과 인간 지능의 유사성을 생각하는 방식에 무의식적인 영향을 끼칠 수 있습니다.

McDermott 의 "희망적인 이름 붙이기" 는 우리가 AI 프로그램을 설명하는 데 사용하는 용어를 의미했지만, 연구 커뮤니티에서 AI 평가 벤치마크에 이름을 붙일 때도 똑같은 문제가 반복됩니다. 그 벤치마크가 검출하기를 "바라는" 능력을 본따 이름을 붙이는 것입니다. 예를 들어, 현재 "자연어 처리(NLP)" 라는 AI 하위 영역에서 가장 널리 인용되는 벤치마크는 "스탠포드 질문 답변 데이터세트" [51], "RACE 독해력 데이터세트" [52], "일반 언어 이해 평가" [53] 등 몇 가지입니다. 이러한 모든 벤치마크에서 최고의 기계의 성능은 이미 인간 (일반적으로 Amazon Mechanical Turk 작업자) 을 대상으로 측정한 성능을 넘어섰습니다. 이로 인해 "새로운 AI 모델이 질문 답변에서 인간의 성능을 뛰어넘다" [54], "컴퓨터가 읽기에서 인간보다 더 좋아지고 있다" [55], "Microsoft의 AI 모델이 자연어 이해에서 인간을 능가했다" [56] 등의 헤드라인이 등장했습니다. 이러한 벤치마크 평가의 이름을 감안할 때 사람들이 이러한 결론을 내리는 것은 놀라운 일이 아닙니다. 문제는 이러한 벤치마크가 실제로 질문 답변, 독해력 또는 자연어 이해에 대한 일반적인 능력을 측정하지 않는다는 것입니다. 벤치마크는 이러한 능력의 매우 제한된 버전만 테스트합니다. 게다가 이러한 벤치마크 중 상당수는 위에서 설명한 것처럼 기계가 지름길, 즉 검출하려는 실제 능력 대신 통계적 상관관계만을 학습하여 테스트에서 높은 성능을 달성하는 것을 허락합니다[57, 58]. 이러한 특정 벤치마크에서 기계가 인간을 능가할 수는 있지만, AI 시스템은 벤치마크 이름과 연관된 보다 일반적인 인간의 능력에는 아직 미치지 못합니다.

### 오류 4: 지능은 모두 뇌와 관련되어 있으리라

지능이 신체에서 분리될 수 있을 것이라는 생각 (비물리적 실체로서든, 온전히 뇌 안에 존재하는 것으로서든) 은 철학과 인지 과학에서 오랜 역사를 가지고 있습니다.

20세기 중반 심리학에서 이른바 "마음의 정보 처리 모델" 이 등장했습니다. 이 모델은 마음을 정보를 입력, 저장, 처리 및 출력하는 일종의 컴퓨터로 간주합니다. 신체는 입력(지각)과 출력(행동) 단계를 제외하고는 큰 역할을 하지 않습니다. 이 견해에 따르면 인지는 전적으로 뇌에서 이루어지며 이론적으로 신체의 나머지 부분과 분리될 수 있습니다. 이 견해의 극단적인 결론은 미래에는 우리의 뇌, 즉 인지와 의식을 컴퓨터에 "업로드" 할 수 있게 될 것이라는 것입니다[59].

지능이 원칙적으로 "비신체화" 될 수 있다는 가정은 인공지능 연구의 역사 내내 암묵적으로 존재합니다. 초기 AI 연구에서 가장 영향력 있는 아이디어 중 하나는 뉴웰과 사이먼의 "물리적 기호 체계 가설" (PSSH)이었습니다: "물리적 기호 체계는 일반적인 지능적 행동에 필요하고 충분한 수단을 가지고 있다." [60]. "물리적 기호 시스템" 이라는 용어는 디지털 컴퓨터와 유사한 것을 의미합니다. PSSH는 디지털 컴퓨터에서 뇌나 신체의 비기호적 과정을 통합하지 않고도 일반 지능을 달성할 수 있다고 가정합니다. (기호적 과정과 비기호적 과정에 대한 통찰력 있는 논의는 호프스태더의 "부울 꿈에서 깨어나기" [61]를 참조하세요).

뉴웰과 사이먼의 PSSH는 1990년대와 2000년대 통계적 및 신경학적 기반 머신러닝이 등장하기 전까지 이 분야를 지배했던 AI에 대한 상징적 접근 방식의 기초 원리였습니다. 그러나 그렇게 등장한 비기호적 접근법 역시 신체를 지능과 관련이 있는 것으로 보지 않았습니다. 대신 1980년대 연결주의에서 오늘날의 심층 신경망에 이르기까지 신경학에서 영감을 받은 접근 방식들은 일반적으로 지능이 오로지 뇌 구조와 역학에서만 발생한다고 가정합니다. 오늘날의 심층 신경망은 세상과 능동적으로 상호작용하지 않고 수동적으로 세상으로부터 데이터를 받아들이고 행동 지침을 출력하는, 그 유명한 "통 속의 뇌" 와 비슷합니다. 물론 로봇과 자율 주행 차량은 세상에 물리적으로 존재한다는 점에서 다르지만, 현재까지 로봇과 자율 주행 차량의 물리적 상호 작용의 종류와 "지능" 에 대한 피드백은 매우 제한적입니다.

지능이 모두 뇌에 있다는 가정은 인간 수준의 AI를 달성하기 위해서는 뇌의 "컴퓨팅 능력" 의 수준으로 기계를 확장한 다음, 이러한 뇌-같은 "하드웨어" 에 적합한 "소프트웨어" 를 개발하면 된다는 추측으로 이어졌습니다. 예를 들어, 한 철학자는 그의 보고서에서 "${10}^{15}$ FLOP/s는 (만들기가 매우 어려울 수 있는 적절한 소프트웨어가 제공된다면) 인간의 두뇌만큼의 작업을 수행하기에 충분할 가능성이 높다고 생각합니다." 라는 결론을 내립니다[62]. 신체가 필요 없다는 것입니다!

최고의 AI 연구자들은 하드웨어를 두뇌에 맞게 확장하면 인간 수준의 인공 지능을 구현할 수 있을 것이라고 입을 모았습니다. 예를 들어 딥러닝의 선구자인 제프리 힌튼은 "인간 수준으로 문서를 이해하려면 아마도 인간 수준의 리소스가 필요할 것이고, 우리 뇌에는 수조 개의 연결이 있습니다. 하지만 지금까지 우리가 구축한 가장 큰 네트워크는 수십억 개의 연결에 불과합니다. 그래서 우리는 몇 배나 뒤처져 있지만 하드웨어 전문가들이 이를 해결할 것이라고 확신합니다." [63]. 다른 사람들은 이러한 "하드웨어적 해결", 즉 인간 수준의 AI를 구현할 수 있는 속도와 메모리 용량이 양자 컴퓨터의 형태로 제공될 것이라고 예측하기도 합니다[64].

그러나 점점 더 많은 연구자들이 지능을 이해하고 AI를 만들기 위한 "모든 것이 뇌 안에 있다" 는 정보 처리 모델의 기초에 의문을 제기하고 있습니다. 컴퓨터 과학자 로드니 브룩스는 "계산 은유의 막다른 골목" 이라는 글을 통해 "우리가 오랫동안 이 막다른 골목에 갇혀 있었던 이유는 무어의 법칙이 계속 우리를 먹여 살렸기 때문이며, 우리는 '아, 우리는 진전을 이루고 있다, 진전을 이루고 있다, 진전을 이루고 있다'고 계속 생각했기 때문입니다. 그러나 어쩌면 진전이 없었을 수 있는 것입니다" 라고 주장합니다[65]. 사실 수많은 인지 과학자들이 수십 년 동안 모든 인지 활동에서 신체가 중심이 된다고 주장해 왔습니다. 이러한 아이디어를 지지하는 저명한 심리학자 중 한 명인 마크 존슨은 1970년대 중반에 힘을 얻으며 "우리가 경험하고, 생각하고, 행동하는 모든 것에서 뇌와 신체의 중심적인 역할에 대한 통합적인 증거를 제공하기 시작한" [66] 체화주의 (embodied cognition) 에 관한 연구 프로그램에 대해 썼습니다. 심리학자 레베카 핀처-키퍼는 체화주의 패러다임을 다음과 같이 정의합니다: "체화된 인지란 개념적 지식의 표상이 신체에 의존한다는 것을 의미하며, 이는 amodal 하거나 symbolic 하거나 abstract 한 대신에 multimodal 합니다. 이 이론은 우리의 생각이 지각, 행동, 감정에 근거하거나 불가분의 관계에 있으며, 뇌와 신체가 함께 작용하여 인지를 한다는 것을 시사합니다." [67].

체화주의에 대한 증거는 다양한 학문 분야에서 나옵니다. 예를 들어, 신경과학 연구에 따르면 인지를 제어하는 신경 구조는 감각 및 운동 시스템을 제어하는 신경 구조와 밀접하게 연결되어 있으며, 추상적 사고는 신체 기반의 신경 "지도" 를 활용한다고 합니다[68]. 신경과학자 돈 터커는 "신체로부터 분리된 인지를 위한 뇌 부위는 존재하지 않는다" 고 언급했습니다[69]. 인지심리학 및 언어학 연구 결과에 따르면 추상적 개념의 많은 부분이 물리적 신체 기반 내부 모델에 기반을 두고 있으며[70], 이는 일상 언어에서 발견되는 물리 기반 은유 체계에 의해 부분적으로 밝혀졌습니다[71].

발달 심리학 등 여러 다른 분야에서도 체화주의에 대한 증거를 추가하고 있습니다. "embodied AI", "developmental robotics", "grounded language understanding" 등으로 알려진 AI의 하위 영역에서 이러한 아이디어를 탐구하는 소수의 연구자 그룹이 있기는 하지만, AI 연구는 대부분 이러한 결과를 무시해 왔습니다.

체화주의와 관련된 개념으로, 일반적으로 지능과 분리되어 있거나 합리성을 방해하는 것으로 여겨지는 우리의 깊은 사회생활에 수반되는 감정과 "비합리적인" 편견이 실제로는 지능을 가능하게 하는 핵심 요소라는 생각을 들 수 있습니다. 인공지능은 흔히 감정, 비합리성, 식욕과 수면 욕구 같은 신체의 제약으로부터 독립된 일종의 "순수한 지능" 을 목표로 하는 것으로 여겨집니다. 순수하게 이성적인 지능의 가능성에 대한 이러한 가정은 미래의 "초지능" 기계로 인해 우리가 직면하게 될 위험에 대한 엉뚱한 예측으로 이어질 수 있습니다.

예를 들어, 철학자 닉 보스트롬은 시스템의 지능과 목표가 서로 독립적일 수 있다고 주장하며 "모든 수준의 지능은 그 어떤 최종 목표와도 결합될 수 있다" 고 주장합니다[72]. 예를 들어, 보스트롬은 페이퍼 클립을 생산하는 것이 유일한 목표인 가상의 초지능 AI 시스템을 상상합니다. 이 가상의 시스템은 초지능을 통해 클립을 생산하는 독창적인 방법을 발명할 수 있고, 이를 위해 지구의 모든 자원을 사용할 수 있습니다.

AI 연구원 스튜어트 러셀도 지능과 목표의 독립성에 대해 보스트롬의 의견에 동의합니다. "범용 지능형 시스템에게 페이퍼 클립의 개수나 알려진 파이의 자릿수를 최대화하는 것을 포함한 그 어떤 목표든 주어질 수 있다고 상상하기는 쉽습니다." [73]. 러셀은 인류의 문제를 해결하기 위해 이러한 초지능을 사용할 때 발생할 수 있는 결과에 대해 걱정합니다: "이산화탄소 농도를 산업화 이전 수준으로 회복하는 일을 맡은 초지능 기후 제어 시스템이 인류 인구를 0으로 줄이는 것이 해결책이라고 믿는다면 어떻게 될까요?... 기계에 잘못된 목표를 입력해 기계가 우리보다 더 지능적이라면 우리는 패배합니다." [74].

보스트롬과 러셀이 제안한 사고 실험은 AI 시스템이 인간과 같은 기본적인 상식 없이도 컴퓨터의 속도, 정확성, 프로그래밍 가능성을 완벽하게 유지하면서 "초지능" 이 될 수 있다고 가정하는 것 같습니다. 하지만 초인적인 AI에 대한 이러한 추측에는 지능의 본질에 대한 잘못된 직관이 깔려 있습니다. 심리학이나 신경 과학에 대한 우리의 지식은 "순수한 이성" 이 우리의 인식과 목표를 형성하는 감정과 문화적 편견으로부터 분리될 수 있다는 가능성을 뒷받침하지 못합니다. 오히려 우리가 체화주의 연구를 통해 알게 된 것은 인간의 지능은 감정, 욕구, 강한 자아감과 자율성, 세상에 대한 상식적인 이해 등 서로 밀접하게 연결된 속성을 가진 강력하게 통합된 시스템이라는 것입니다. 이러한 속성을 분리할 수 있다는 것은 전혀 분명하지 않습니다.

### 결론

제가 설명한 네 가지 오류는 현재 AI의 상태에 대한 우리의 개념화와 지능의 본질에 대한 우리의 제한된 직관에 결함이 있음을 드러냅니다. 저는 이러한 오류들이 인간 수준의 지능을 가진 기계를 만들어내는 일이 항상 생각보다 어려운 이유의 일부분이라고 주장했습니다.

이러한 오류는 AI 연구자들에게 몇 가지 질문을 제기합니다. "일반" 또는 "인간 수준" 의 AI를 향한 실제 진전을 어떻게 평가할 수 있을까요? 특정 도메인에서 인간과 AI의 능력을 비교할 때 그 난이도를 어떻게 평가할 수 있을까요? 희망적인 이름 붙이기로 우리 자신과 다른 사람들을 속이지 않고 AI 시스템의 실제 능력을 어떻게 설명해야 할까요? 인지 편향, 감정, 목표, 체화 등 인간 인지의 다양한 차원을 어느 정도까지 분리할 수 있을까요? 지능이 무엇인지에 대한 우리의 직관을 어떻게 개선할 수 있을까요?

이러한 질문은 여전히 열려 있습니다. AI의 진보를 더 효과적으로 만들고 평가하기 위해서는 기계가 할 수 있는 일에 대해 더 나은 어휘를 개발해야 한다는 것은 분명합니다. 그리고 더 일반적으로는 자연계의 다양한 시스템에서 나타나는 지능에 대한 더 나은 과학적 이해가 필요할 것입니다. 이를 위해서는 AI 연구자들이 지능을 연구하는 다른 과학 분야와 더 깊이 교류해야 합니다.

*상식*이라는 개념은 최근 AI 연구자와 다른 여러 분야, 특히 인지 발달 분야의 인지 과학자 간의 협업을 주도하고 있는 지능의 한 측면입니다(가령 [75] 참조). AI의 역사에서 기계에 인간과 같은 상식을 부여하려는 시도는 많이 있었습니다. 존 매카시 [76]와 더글라스 르낫 [77]의 논리 기반 접근 방식부터 오늘날의 딥러닝 기반 접근 방식 (가령 [78]) 에 이르기까지 다양한 시도가 있었습니다. 인공지능 연구자 오렌 에치오니는 "상식" 을 "인공지능의 암흑 물질" 이라고 부르며 "상식은 쉽게 말로 표현할 수는 없지만 모든 것에 영향을 미친다" 고 언급한 바 있습니다[79]. 이 용어는 오늘날의 최첨단 AI 시스템에서 누락된 부분을 가리키는 일종의 포괄적인 용어가 되었습니다[80, 81]. 상식은 인간이 세상에 대해 가지고 있는 방대한 양의 지식을 포함하지만, 또한 이러한 지식을 사용하여 우리가 직면하는 상황을 인식하고 예측하고 그러한 상황에서 우리의 행동을 안내하는 능력을 포함해야 합니다. 기계에 상식을 부여하려면 공간, 시간, 인과관계, 무생물 및 기타 생명체의 본질[82], 세부적인 것에서 일반 개념으로 추상화하고 이전 경험에서 유추할 수 있는 능력 등 인간 유아가 가지고 있는 아주 기본적인 '핵심', 어쩌면 타고난 지식을 기계에 심어주어야 할 것입니다. 이러한 지식이나 능력을 기계에 담는 방법은 아직 아무도 모릅니다. 이것이 현재 AI 연구의 최전선이며, 한 가지 고무적인 방법은 어린아이의 이러한 능력 발달에 대해 알려진 것을 활용하는 것입니다. 흥미롭게도 이 접근법은 앨런 튜링이 튜링 테스트를 소개한 1950년 논문에서 권장한 방식입니다. 튜링은 "성인의 마음을 시뮬레이션하는 프로그램을 만들려고 노력하는 대신 어린이의 마음을 시뮬레이션하는 프로그램을 만들어 보는 것은 어떨까요?" 라고 물었습니다[83].

1892년에 심리학자 윌리엄 제임스는 당시 심리학에 대해 "이것은 과학이 아니라 과학에 대한 희망일 뿐이다" 라 고 말했습니다[84]. 이는 오늘날의 AI를 완벽하게 표현한 말입니다. 실제로 몇몇 연구자들은 AI와 중세의 연금술을 비유한 바 있습니다. 1977년 AI 연구자인 테리 위노그라드는 "어떤 면에서 AI는 중세의 연금술과 비슷합니다. 우리는 아직 만족스러운 이론을 개발하지 못한 채 여러 가지 물질 조합을 조합하여 어떤 일이 일어나는지 보는 단계에 있지만... 과학적 화학 이론을 개발할 수 있는 풍부한 데이터를 제공한 것은 연금술사들의 실제 경험과 호기심이었습니다" [85]라고 말했습니다. 40년 후, Microsoft Research의 이사인 에릭 호르비츠도 이에 동의했습니다: "지금 우리가 하고 있는 일은 과학이 아니라 일종의 연금술입니다." [86]. AI의 진정한 진보의 본질, 특히 그것이 생각보다 어려운 이유를 이해하려면 연금술에서 지능에 대한 과학적 이해로 나아가야 합니다.

### References

[1] T. Adams. Self-driving cars: From 2020 you will become a permanent backseat driver. The Guardian, 2015. URL https://tinyurl.com/zepc5spz.  
[2] 10 million self-driving cars will be on the road by 2020. Business Insider Intelligence, 2020. URL https://tinyurl.com/9mr6x8zk.  
[3] A. J. Hawkins. Here are Elon Musk’s wildest predictions about Tesla’s self-driving cars. The Verge, 2019. URL https://tinyurl.com/4ap2rm2n.  
[4] R. McCormick. NVIDIA is working with Audi to get you a self-driving car by 2020. The Verge, 2017. URL https://tinyurl.com/3epbpaz3.  
[5] Y. Kageyama. CEO: Nissan will be ready with autonomous driving by 2020. Phys.Org, 2015. URL https://phys.org/news/2015-05-ceo-nissan-ready-autonomous.html.  
[6] S. Eagell. https://tinyurl.com/psxwhhnb, 2020.  
[7] R. Baldwin. Tesla tells California DMV that FSD is not capable of autonomous driving. Car and  
Driver, 2021. URL https://tinyurl.com/p8aw9jke.  
[8] McDermott D., M. M. Waldrop, B. Chandrasekaran, J. McDermott, and R. Schank. The dark ages of  
AI: A panel discussion at AAAI-84. AI Magazine, 6(3):122–134, 1985.  
[9] New Navy Device Learns by Doing. New York Times, 1958. URL https://tinyurl.com/yjh3eae8.  
[10] H. A. Simon. The Ford Distinguished Lectures, Volume 3: The New Science of Management Decision. Harper and Brothers, p. 38, 1960.  
[11] The Shannon Centennial: 1100100 years of bits. https://www.youtube.com/watch?v=pHSRHi17RKM, 1961.  
[12] M. L. Minsky. Computation: Finite and Infinite Machines. Prentice-Hall, p. 2, 1967.  
[13] M. L. Minsky and S. A Papert. Perceptrons: An Introduction to Computational Geometry. MIT press,  
1969.  
[14] J. Lighthill. Artificial intelligence: A general survey. In Artificial Intelligence: A Paper Symposium. Science Research Council, 1973.  
[15] J. Durkin. Expert systems: A view of the field. IEEE Annals of the History of Computing, 11(02): 56–63, 1996.  
[16] B. R. Gaines. Perspectives on Fifth Generation computing. Oxford Surveys in Information Technology, 1:1–53, 1984.  
[17] M. Stefik. Strategic computing at DARPA: Overview and assessment. Communications of the ACM, 28(7):690–704, 1985.  
[18] J. L. McClelland, D. E. Rumelhart, and the PDP Research Group. Parallel Distributed Processing, Vol.1: Foundations. MIT press Cambridge, MA, 1986.  
[19] J. L. McClelland, D. E. Rumelhart, and the PDP Research Group. Parallel Distributed Processing, Vol.2: Psychological and Biological Models. MIT press Cambridge, MA, 1986.  
[20] C. Moewes and A. Nu ̈rnberger. Computational Intelligence in Intelligent Data Analysis. Springer, p. 135, 2013.  
[21] V. C. Mu ̈ller and N. Bostrom. Future progress in artificial intelligence: A survey of expert opinion. In V. C. Mu ̈ller, editor, Fundamental Issues of Artificial Intelligence, pages 555–572. Springer, 2016.  
[22] K. Grace, J. Salvatier, A. Dafoe, B. Zhang, and O. Evans. When will AI exceed human performance? Evidence from AI experts. Journal of Artificial Intelligence Research, 62:729–754, 2018.  
[23] S. Russell. Human Compatible: Artificial Intelligence and the Problem of Control. Penguin, p. 77, 2019.  
[24] https://moores.samaltman.com, 2021.  
[25] J. Despres. Scenario: Shane Legg. Future, 2008. URL https://tinyurl.com/hwzna364.  
[26] H. McCracken. Inside Mark Zuckerberg’s bold plan for the future of Facebook. Fast Company, 2015. URL www.fastcompany.com/3052885/mark-zuckerberg-facebook.  
[27] R. Geirhos, J.-H. Jacobsen, C. Michaelis, R. Zemel, W. Brendel, M. Bethge, and F. A. Wichmann. Shortcut learning in deep neural networks. Nature Machine Intelligence, 2(11):665–673, 2020.  
[28] S. Lapuschkin, S. W ̈aldchen, A. Binder, G. Montavon, W. Samek, and K.-R. Mu ̈ller. Unmasking Clever Hans predictors and assessing what machines really learn. Nature Communications, 10(1):1–8, 2019.  
[29] M. Mitchell. Abstraction and analogy-making in artificial intelligence. arXiv:2102.10717, 2021.  
[30] S. M. Moosavi-Dezfooli, A. Fawzi, O. Fawzi, and P. Frossard. Universal adversarial perturbations. In Proceedings of the IEEE Conference on Computer Vision and Pattern Recognition (CVPR), pages 1765–1773, 2017.  
[31] J. Devlin, M.-W. Chang, K. Lee, and K. Toutanova. BERT: Pre-training of deep bidirectional trans- formers for language understanding. arXiv:1810.04805, 2018.  
[32] C. Finn, P. Abbeel, and S. Levine. Model-agnostic meta-learning for fast adaptation of deep networks. In Proceedings of the International Conference on Machine Learning (ICML), pages 1126–1135, 2017.  
[33] K. Arulkumaran, M. P. Deisenroth, M. Brundage, and A. A. Bharath. A brief survey of deep reinforce- ment learning. IEEE Signal Processing Magazine, 34, 2017.  
[34] J. Aron. AI landmark as Googlebot beats top human at ancient game of Go. New Scientist, 2016. URL https://tinyurl.com/2ztrk6hv.  
[35] https://tinyurl.com/45fs8a7e, 2013.  
[36] https://tinyurl.com/36rkhzrz, 2019.  
[37] Hubert L Dreyfus. A history of first step fallacies. Minds and Machines, 22(2):87–99, 2012.  
[38] M. Minsky. The Society of Mind. Simon and Schuster, p. 29, 1987.  
[39] H. Moravec. Mind Children: The Future of Robot and Human Intelligence. Harvard University Press, p. 15, 1988.  
[40] D. R. Hofstadter. Metamagical Themas: Questing for the Essence of Mind and Pattern. Basic Books, p. 632, 1985.  
[41] A. Ng. What artificial intelligence can and can’t do right now. Harvard Business Review, 2016. URL https://tinyurl.com/udnzhuk.  
[42] D. Silver et al. Mastering the game of Go without human knowledge. Nature, 550(7676):354–359, 2017.  
[43] G. Marcus. Innateness, Alphazero, and artificial intelligence. arXiv:1801.05667, 2018.  
[44] H. Moravec. Mind Children: The Future of Robot and Human Intelligence. Harvard University Press, pp. 15–16, 1988.  
[45] Minsky, M. Decentralized minds. Behavioral and Brain Sciences, 3(3):439–440, 1980.  
[46] D. McDermott. Artificial intelligence meets natural stupidity. ACM SIGART Bulletin, (57):4–9, 1976.  
[47] S. Gustin. IBM’s Watson supercomputer wins practice Jeopardy round. Wired, 2011.  
[48] https://www.ibm.com/cognitive/stories/bots/stories/sciencefact/.  
[49] Mixed outlook for human-versus-AI match. Korea Herald, 2016. URL https://tinyurl.com/zb3ywabe.  
[50] S. Shead. Google DeepMind is edging towards a 3-0 victory against world Go champion Ke Jie. Business Insider, 2017. URL https://tinyurl.com/svb7x7e6.  
[51] https://rajpurkar.github.io/SQuAD-explorer/.  
[52] https://www.cs.cmu.edu/~glai1/data/race/.  
[53] https://gluebenchmark.com/.  
[54] D. Costenaro. New AI model exceeds human performance at question answering. BecomingHuman.ai, 2018. URL https://tinyurl.com/ujwp6s95.  
[55] S. Pham. Computers are getting better than humans at reading. CNN Business, 2018. URL https://tinyurl.com/k48xa8nj.  
[56] U. Jawad. Microsoft’s AI model has outperformed humans in natural language understanding. Neowin, 2021. URL https://tinyurl.com/2x4r54ad.  
[57] T. McCoy, E. Pavlick, and T. Linzen. Right for the wrong reasons: Diagnosing syntactic heuristics in natural language inference. In Proceedings of the 57th Annual Meeting of the Association for Computa- tional Linguistics (ACL), pages 3428–3448, 2019.  
[58] T. Linzen. How can we accelerate progress towards human-like linguistic generalization? In Proceedings of the 58th Annual Meeting of the Association for Computational Linguistics (ACL), pages 5210–5217, 2020.  
[59] V. Woollaston. We’ll be uploading our entire minds to computers by 2045 and our bodies will be replaced by machines within 90 years, Google expert claims. Daily Mail, 2013. URL https://tinyurl.com/ht44uxzv.  
[60] A. Newell and H. A. Simon. Computer science as empirical inquiry: Symbols and search. Communica- tions of the ACM, 19:113–126, 1976.  
[61] D. R. Hofstadter. Waking up from the Boolean dream, or, subcognition as computation, 1985. Chapter 26 of Metamagical Themas, Basic Books.  
[62] J. Carlsmith. New report on how much computational power it takes to match the human brain. https://www.openphilanthropy.org/blog/new-report-brain-computation, 2020.  
[63] J. Patterson and A. Gibson. Deep Learning: A Practitioner’s Approach. O’Reilly Media, p. 231, 2017.  
[64] G. Musser. Job one for quantum computers: Boost artificial intelligence. Quanta Magazine, 2018. URL https://tinyurl.com/2k8fw628.  
[65] The cul-de-sac of the computational metaphor. https://tinyurl.com/rjtumkca, 2019.  
[66] Mark Johnson. Embodied Mind, Meaning, and Reason: How Our Bodies Give Rise to Understanding.  
University of Chicago Press, 2017.  
[67] Rebecca Fincher-Kiefer. How the Body Shapes Knowledge: Empirical Support for Embodied Cognition. American Psychological Association, 2019.  
[68] R. A. Epstein, E. Z. Patai, J. B. Julian, and H. J. Spiers. The cognitive map in humans: spatial navigation and beyond. Nature Neuroscience, 20(11):1504, 2017.  
[69] D. M. Tucker. Mind from Body: Experience from Neural Structure. Oxford University Press, 2007.  
[70] L. W. Barsalou and K. Wiemer-Hastings. Situating abstract concepts. In Grounding Cognition: The Role of Perception and Action in Memory, Language, and Thought, pages 129–163. Cambridge University Press, 2005.  
[71] G. Lakoff and M. Johnson. Metaphors We Live By. University of Chicago Press, 2008.  
[72] N. Bostrom. Superintelligence: Paths, Dangers, Strategies. Oxford University Press, p. 105, 2014.  
[73] S. Russell. Human Compatible: Artificial Intelligence and the Problem of Control. Penguin, p. 167, 2019.  
[74] S. Russell. How to stop superhuman A.I. before it stops us. New York Times, 2019. URL https://www.nytimes.com/2019/10/08/opinion/artificial-intelligence.html.  
[75] M. Turek. Machine common sense. https://www.darpa.mil/program/machine-common-sense.  
[76] J. McCarthy. Programs with common sense. In Readings In Knowledge Representation. Morgan Kauf-  
mann, 1986.  
[77] D. B. Lenat, R. V. Guha, K. Pittman, D. Pratt, and M. Shepherd. CYC: Toward programs with  
common sense. Communications of the ACM, 33(8):30–49, 1990.  
[78] R. Zellers, Y. Bisk, A. Farhadi, and Y. Choi. From recognition to cognition: Visual commonsense rea- soning. In Proceedings of the IEEE Conference on Computer Vision and Pattern Recognition (CVPR), pages 6720–6731, 2019.  
[79] W. Knight. The US military wants to teach AI some basic common sense. Technology Review, 2018. URL https://tinyurl.com/2xuvxefe.  
[80] E. Davis and G. Marcus. Commonsense reasoning and commonsense knowledge in artificial intelligence. Communications of the ACM, 58(9):92–103, 2015.  
[81] H. J. Levesque. Common Sense, the Turing Test, and the Quest for Real AI. MIT Press, 2017.  
[82] E. S. Spelke and K. D. Kinzler. Core knowledge. Developmental Science, 10(1):89–96, 2007.  
[83] A. M. Turing. Computing machinery and intelligence. Mind, 59(236):433–460, 1950.  
[84] W. James. Psychology: The Briefer Course. Henry Holt, p. 335, 1892.  
[85] T. Winograd. On some contested suppositions of generative linguistics about the scientific study of language. Cognition, 5(2):151–79, 1977.  
[86] C. Metz. A new way for machines to see, taking shape in Toronto. New York Times, 2017. URL https://tinyurl.com/5bwd3u9n.  
