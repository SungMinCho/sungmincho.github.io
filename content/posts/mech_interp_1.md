---
title: "Mechanistic Interpretability (1)"
date: 2023-05-18T17:13:44+09:00
draft: false
---


### 0. 소여

**2023년 1월 중순.**

몇 달 전에 작성한 [회사 기술 블로그 글](https://blog.getliner.com/sequential-recommenders/)에서도 밝힌 바 있지만, 당시 저는 Transformer 를 마냥 성능 좋은 Black Box 취급하는 접근법들에 대해서 조금씩 거부감을 느끼기 시작하고 있었습니다. 모델이 **왜 잘 되는지**에 대한 실증적인 이해가 부재한 상태에서 모델을 **더 잘 되게** 만드는 작업을 한다는 것은, 마치 두 사람이 서로가 서로의 몸을 디디고 점프하여 하늘 높이 올라간다는 어릴 적의 모순적인 상상과 흡사하게 느껴졌습니다. Ground (땅/근거) 가 없기 때문입니다. 그러나 딥러닝 기계는 분명히 비행을 하고 있었고, 따라서 Ground 가 부재한 것은 실재가 아닌 오직 사태를 해석하고 설명하는 사고의 틀 안에서였을 뿐입니다.

다른 한편, 양질의 데이터를 더 많이 넣어 줘서 모델의 예측력을 개선하는 작업은, 이번엔 반대로 너무 땅에 두 발 딛고 서 있는 느낌이라 개인적으로 의미를 찾기가 어려웠습니다. 비행기가 이륙하는 원리를 이해하지 못한다고 해도, 비행기에게 연료를 더 넣어주면 넣어줄수록 비행에 도움이 되리라는 것은 당연한 사실입니다. 새로운 피쳐들을 고안하여 사용자들이 뿜어내는 데이터 배기가스[^surplus] 를 더욱 철저하게 수집하는 작업은, 추천 시스템 기계를 위한 고품질 연료를 마련해주는 작업처럼 느껴졌기에, 물론 그 나름의 사업적 의미가 뚜렷함에도 불구하고 적어도 이륙의 신비와 관련하여서는 어쩐지 자꾸만 겉도는 느낌이 드는 것이었습니다.

그 사이에 경이로운 비행의 사례들이 자꾸만 등장하기 시작했습니다. OpenAI 는 ChatGPT, GPT4 등을 통해 언어모델의 역량을 새로운 높이로 올려보내는 듯했고, 그 고도에서 사람들은 자꾸만 새로운 지평을 보기 시작했습니다. 온갖 프로덕트와 논문들이 쏟아져 나왔고, 분명 그 중엔 hype 에 불과한 것이 없지 않았으나, 거스를 수 없는 바람이 불고 있다는 것은 모두가 동의하는 사실이었습니다.

이렇듯 저의 안팎으로 부는 바람 때문에 저는 자꾸만 새로운 것을 찾아 나서게 되었습니다. 마음에 두고 있던 것은 크게 두 가지였는데 하나는 **설명가능성**이었고, 다른 하나는 이 일이 꼭 추천 시스템에 국한된 일이 되지는 않게 하자는 것이었습니다. 전자는 내포 (intension) 와 관련이 있었고 후자는 외연 (extension) 과 관련이 있었는데, 둘 모두와 관련하여 개인적인 답답함을 느끼고 있던 까닭이었습니다. 스스로 "실력"이 있다고 확신할 수 있으려면, 땅에서 연료를 마련하거나 공중에서 근거도 없는 소설을 써내는 일에 몰두할 것이 아니라, 이륙과 비행의 원리와 의미들을 정확히 설명할 줄 알게 되는 일에 몰두해야 하는 것이 아닌가... 그리고 기왕 할거면 신비의 경계가 모호한 추천 시스템보다는[^recwow] 신비의 순간을 정확히 지목할 수 있는 언어 모델[^sparksofagi] 같은 것들이 더 뜯어볼 맛이 있지 않겠나... 말하자면 그런 생각들을 한 것입니다.

Anthropic 의 [Transformer Circuits Thread](https://transformer-circuits.pub/) 를 보게 되었습니다. 예전에 [Distill.pub](https://distill.pub/2020/circuits/) 이 학습된 비전 모델 안에서 회로들을 (Circuits) 찾으려고 했던 것과 유사하게, Anthropic 은 언어 모델 안에서 회로들을 찾아내려고 하고 있었습니다. 인공 신경망들은 결코 이해 불가능한 방식으로 작동하지 않는다는 가설이 이러한 접근법들의 근간이 됩니다. 다시 말해, 신경망 내부에서 의미를 구성하는 **Feature** 들이 독립된 형태로 존재하고, 그 **Feature** 들을 연결하고 운반하는 **Circuit** 들 또한 식별 가능한 형태로 존재하기에, 우리는 신경망을 분해할 수 있다는 이야기입니다. 만약 반대로 의미가 고차원 공간에 안개처럼 흩뿌려진 형태로 구성되어 있다면, 우리는 차원의 저주 때문에 결코 신경망을 분해할 수 없을 것입니다.

Anthropic 이 Induction Head 등의 유의미한 회로들을 찾아내는 것에 고무된 저는 비슷한 시도를 Sequential Recommender 에 살짝 적용해 보았지만 유의미한 회로들을 찾아낼 수 없었습니다. 물론 거칠게 말하면 어텐션 패턴 조금 살펴본 게 전부이기에 당시에 충분한 노력을 투입했다고 할 수는 없겠지만, 또 한편으로는 자연어의 단어들이 의미의 촘촘한 관계망을 구성하고 있는 것과 반대로 추천 시스템이 다루는 아이템들은 그 즉자적/대자적 의미가 다소 희박하고 희미한 것이 아닐까 하는 생각 또한 들었습니다. "I" 와 "am" 이라는 단어가 맺고 있는 결연관계의 강력함에 비해, 아이템과 아이템, 또는 웹문서와 웹문서가 맺고 있는 결연관계라고 하는 것은 과연 얼마나 강력할까? 추천 모델들은 과연 Memorization 을 한 것일까 Generalization 을 한 것일까 (그걸 구분하고 검증하려는 시도를 하고 있긴 할까)? 좋은 추천의 본질은 무엇인가에 대한 막연한 논의가 사무실 한 구석에서 끊임 없이 피어오르고, 집에서 릴스와 틱톡을 보면서 "추천의 클릭률을 올리는 방법은 유저가 좋아할만한 아이템을 예측하는 것이 아니라 오히려 유저 자체가 더 예측가능해지도록 길들이는 방법이 아닐까 (중독의 문제)" 하는 생각도 피어오르는 가운데, 저는 추천 기계의 비행 메커니즘은커녕 점점 위아래도 구분하기가 어려운 무중력 상태에 가끔씩 빠져들기도 했던 것입니다[^gravitynlp].

그 이후 당분간은 인턴쉽을 잘 마무리 하기 위한 일련의 작업들로 넘어갔지만, Mechanistic Interpretability 에 대한 관심은 사실 이때부터 시작되고 있었다고 볼 수 있습니다.


### 1. AI Alignment

**2023년 4월 말.**

이제 회사를 나가면 무엇을 할 것이냐는 누군가의 질문에, 저는 "세상을 읽어보려 한다"고 답했습니다. 어울리지 않게 거창한 이 답변은 그 이후로 하나의 입버릇이 되었고, 들어줄 준비가 되어 있는 사람이라면 누구든지 붙잡아 앉혀 놓고 이 시도의 중간결과들을 줄줄이 읊는 지경에 이르렀습니다. 세상을 **왜** 읽어야 하는지에 대한 질문은 대답하기가 너무 쉽습니다. 내가 앞으로 10년 동안 미친 듯이 일하고 성장하지 않으면 도태될 것이라는 사실은 너무 분명한데, 도대체 **무엇**을 미친 듯이 해야 하는지에 대해 단서를 얻기 위해서는 세상을 살펴볼 필요가 있기 때문입니다. 그러나 세상을 **어떻게** 읽어야 하는지에 대한 질문은 대답하기가 너무 어렵습니다. 트위터와 링크드인과 유튜브와 팟캐스트를 수시로 주시해 보아도 신호와 소음을 분간해내기가 쉽지 않았습니다.

아무튼 세상 사람들이 무엇을 떠들고 있나 나름대로 귀를 기울여보니, 곧 도래할 AGI 에 대한 얘기가 많이 들려오는 듯 했습니다[^biased]. 고등학교 2학년 추석 즈음이었나, 안양외고 도서관에서 레이 커즈와일의 <특이점이 온다> 를 읽은 기억이 어렴풋합니다. 그 당시만해도 특이점은 지능 증가의 지수함수적 그래프를 커즈와일이 외삽해서 연장해놓은 선 위의 어디쯤에 막연히 위치하는 듯 보였습니다. 그런데 10년 정도 지난 지금, 많은 유명인사들이 AGI 가 바로 코앞에 보인다는 듯이 (그것도 진지한 얼굴로) 이야기하고 있는 것입니다. 게다가 적지 않은 사람들이 곧 도래할 AGI 가 인류에게 심각한 위협을 초래할 것이라고 (더욱 더 진지한 얼굴로) 이야기하고 있었습니다[^oppenheimer].

Geoffrey Hinton 은 얼마 전 구글에서 나오면서, 자신의 예전 믿음과 달리 이젠 인간보다 뛰어난 지능이 정말로 도래할 것처럼 보인다고 밝혔습니다 [^hinton_agi]. Max Tegmark 는 제가 군대에서 <맥스 테그마크의 유니버스> 를 읽을 때만 해도 물리학 하는 교수님인줄로만 알았는데, 어느새 보니 AI safety 문제에 대해 꾸준히 목소리를 내는 인물로 자리하고 있었고 [^lex_max], 제자 Ziming Liu[^zimingliu] 와 함께 Mechanistic Interpretability 논문들을 내고 있기도 하였습니다. 마지막으로 Eliezer Yudkowsky[^lex_yud] 는 이쪽 진영의 가장 극단적이고 상징적인 인물인 것처럼 보였는데, 오래전부터 AI 에 의한 인류 멸망 가능성에 대해서 아주 과격한 주장들을 해오던 사람인 것 같았습니다 [^yud_times]. 이 외에도 Nick Bostrom, Stuart Russell 등의 이름들을 언급할 수 있을 것 같습니다.

제가 놀랐던 건 이 사람들이 모두 진심인 것처럼 보였기 때문입니다. 물론 사람이 진심이라고 해서 자동으로 설득력이 생기는 것은 아닙니다[^sincere]. 그래도 나름 잃을 것이 많은 사람들이 이렇게나 진지한 얼굴로 이런 얘기들을 하면서 돌아다닌다는 것은, 아무리 비밀스런 이해타산과 꿍꿍이속이 존재한다고 해도, 적어도 그들의 눈에 무언가가 분명히 보였기 때문이 아닐까 하는 생각이 들었습니다. 더불어, OpenAI 의 내부 연구들을 본 사람들 중에 AI safety 에 대해 걱정하지 않는 사람들은 한 명도 존재하지 않는다는 소문도 (출처를 기억할 수 없음) 들려왔고요.

AI safety 문제의 하위분야가 AI alignment 문제입니다[^wiki_alignment]. 사람이든 인공지능이든, 일련의 가치와 목표들을 따르도록 정렬시키는 것은 상당히 어려운 문제입니다[^education]. 충분히 똑똑할 경우 자기만의 목표를 만들어낼 수 있고, 그렇지 않더라도 이미 주어진 목표를 수행하기 위해서 다른 위험한 우회로나 하위목표들을 찾아낼 수 있기 때문입니다 (Hinton 은 power seeking 이 거의 언제나 유용한 하위 목표라는 점을 지적한 바 있습니다). 인간보다 뛰어난 지능체의 영향력과 파괴력을 감안한다면, 또 현재 AI 역량의 증가에 가속도가 붙어 가는 형국을 고려한다면, 인간보다 뛰어난 지능체가 만들어지기 이전에 AI alignment 문제를 풀어야 한다는 논리가 문제의 시급성과 중요성을 구성합니다.

AI alignment 문제의 하위분야가 Mechanistic Interpretability 라고 보아도 될 것 같습니다[^seri_mats_mech_interp]. AI alignment 를 풀기 위해선 ChatGPT 처럼 학습된 신경망이 정확히 어떻게 작동하길래 그렇게 좋은 성능을 내는지 이해할 필요가 있습니다. 따라서 마치 바이너리 프로그램을 리버스 엔지니어링 하거나 사람의 뇌를 해부하듯이, 인공 신경망을 해부하여 그 작동 과정에 대한 실증적인 이해를 얻어내고자 하는 것입니다. 기존 Interpretability 연구들과 다른 점은 바로 그 "실증성" 인데, 예를 들어 고작 어텐션 패턴을 시각화 해놓고 말을 지어내는 수준이 아니라, 실존하는 Feature 들과 Circuit 들을 통해 그 작동을 설명할 수 있어야 한다는 것입니다.

[Lesswrong](https://www.lesswrong.com/) 은 오래 전에 Yudkowsky 가 만든 사이트입니다. 원래는 합리적인 사고를 지향하는 사람들의 모임이었다고 전해지지만, 현재로써는 AI 로 인한 인류 멸망의 문제를 종교적인 진지함을 가지고 대하는 사람들이 모여있는 듯 했습니다 (파생: [Alignment Forum](https://www.alignmentforum.org/)). 여기서 저는 우연히 Mechanistic Interpretability 와 두번째로 조우하게 됐는데, 특히 Anthropic 의 Circuit 연구에 참여했던 Neel Nanda 의 최근 작업들을 보게 되었습니다[^neel_nanda_mech_interp]. 거기서 출발하여 소박하게 공부해 본 바를 아래 "2. Reading List" 섹션에서 소개해 볼 예정입니다.

### 2. Reading List

저는 현재로써는 아래 리스트 중 일부만 직접 읽었습니다. 읽은 것들에 대해서는 가볍게 요약 및 코멘트를 해놓았습니다. 나머지 글들을 같이 읽어나갈 마음이 있다면 연락을 달라고 하고 싶지만, 저조차도 이 공부를 계속 해나갈지 확신이 없기에 삼갑니다.

- [A Comprehensive Mechanistic Interpretability Explainer & Glossary](https://dynalist.io/d/n2ZWtnoYHrU1s4vnFSAQ519J)
	- Mechanistic Interpretability 를 둘러싼 개념, 용어, 가설, 작업 등을 훌륭하게 정리해놓은 문서입니다.
- [200 Concrete Open Problems in Mechanistic Interpretability: Introduction](https://www.alignmentforum.org/posts/LbrPTJ4fmABEdEnLf/200-concrete-open-problems-in-mechanistic-interpretability)
	- Mechanistic Interpretability 에서 풀어볼만한 Open Problem 들을 제시합니다.
- [Transformer Circuits Thread](https://transformer-circuits.pub/)
	- [A Mathematical Framework for Transformer Circuits](https://transformer-circuits.pub/2021/framework/index.html)
		- Residual Network 를 중심에 두는 수학적 재형식화를 통해 Transformer 를 Circuit 들의 종합으로 바라볼 수 있는 기반을 마련합니다. Attention layer 만 남긴 Toy Model 에서 Induction Head 의 존재를 발견합니다.
	- [In-context Learning and Induction Heads](https://transformer-circuits.pub/2022/in-context-learning-and-induction-heads/index.html)
		- 모델 학습 과정에서 Induction Head 의 생성 전후에 일어나는 일련의 상전이 (phase change) 들을 추적함으로써 Induction Head 가 In-context Learning 의 중추적 역할을 한다는 것을 보이려 합니다.
	- [Softmax Linear Units](https://transformer-circuits.pub/2022/solu/index.html)
		- 이전 작업들에서 Attention-only 모델들만 조사한 이유는 MLP 레이어가 해석하기 너무 어렵기 때문입니다. 어려운 이유 중 하나는 중첩 (superposition) 때문인데, 뉴런의 개수보다 표현하고 싶은 Feature 의 개수가 많을 때 Feature 들을 나타내는 차원들이 살짝씩 겹쳐지는 현상을 지칭합니다. 이를 방지하여 더 설명하기 쉬운 모델을 만들기 위해 ReLU 대신 SoLU 레이어를 넣는 방법을 제안합니다.
	- [Toy Models of Superposition](https://transformer-circuits.pub/2022/toy_model/index.html)
		- 중첩 현상이 가설에 불과한 것이 아니라 실존하는 현상임을 Toy Model 을 통해 보입니다.
	- [Superposition, Memorization, and Double Descent](https://transformer-circuits.pub/2023/toy-double-descent/index.html)
	- [Privileged Bases in the Transformer Residual Stream](https://transformer-circuits.pub/2023/privileged-basis/index.html)
	- ...
- [Interpretability in the Wild: a Circuit for Indirect Object Identification in GPT-2 Small](https://openreview.net/forum?id=NpsVSN6o4ul)
- [ROME](https://rome.baulab.info/)
	- 대규모 언어 모델들은 세상에 대한 사실들을 기억하고 있는 것처럼 보입니다. 이때 그 사실들이 모델 내부에 정확히 어디에 어떻게 기록되는지를 밝혀 내고, 적절한 개입을 통해 언어 모델이 에펠탑이 로마에 있는 것처럼 착각하게 만들 수 있음을 보여줍니다.
- [TransformerLens](https://github.com/neelnanda-io/TransformerLens)
	- 모델의 중간 단계의 결과들이 각각 최종 logit 에 어떤 영향을 끼치는지 시각화 하는 방법을 제공합니다.
- **Grokking**
	- Grokking: 모델이 오버피팅을 하는 것처럼 보이다가 일순간에 무언가를 깨우친 듯 일반화 해버리는 현상을 지칭합니다.
	- [A Mechanistic Interpretability Analysis of Grokking](https://www.alignmentforum.org/posts/N6WM6hs7RQMKDhYjB/a-mechanistic-interpretability-analysis-of-grokking)
		- $x+y$ 을 113 으로 나눈 나머지를 맞추도록 학습시킨 모델에서 Grokking 현상이 발생하는데, 이는 모델이 퓨리에 변환을 활용하는 기괴한 알고리즘을 붙잡아내는 순간과 관련이 있음을 밝혀냅니다. 실증적 회로!
	- [A Toy Model of Universality:Reverse Engineering How Networks Learn Group Operations](https://arxiv.org/pdf/2302.03025.pdf)
		- Representation theory 라는 수학의 한 분야를 활용하여, 위의 "(x+y)%113 <=> 퓨리에 회로" 현상의 일반적인 경우를 밝혀냅니다. 또 이를 통해 Universality 가설 (서로 다른 모델들이 결국은 같은 회로를 배우게 된다는 가설) 의 약한 버전만이 성립한다는 증거를 제시합니다.
	- [Towards Understanding Grokking: An Effective Theory of Representation Learning](https://arxiv.org/pdf/2205.10343.pdf)
		- NeurIPS 2022
		- 모델 학습을 4가지 phase 로 분류할 때 (Comprehension, Grokking, Memorization, Confusion), learning rate 와 weight decay 등을 변수로 하는 공간 위에서 물리학의 상평형과 비슷한 그림이 그려짐을 보입니다. 또한 모델의 일반화는 representation 들이 "올바른 구조로 자리잡힐 때" 발생함을 밝힙니다.
	- [Unifying Grokking and Double Descent](https://arxiv.org/pdf/2303.06173.pdf)
		- NeurIPS 2022 ML Safety Workshop
	- [OMNIGROK: GROKKING BEYOND ALGORITHMIC DATA](https://arxiv.org/pdf/2210.01117.pdf)
		- ICLR 2023
		- (당시 읽고 남긴 코멘트: 이쯤 되니 grokking 이 왜 주목할만한 현상인지 잘 모르겠다. 그냥 generalization 이 늦게 되는 것 뿐 아닌가?)
	- [Hidden Progress in Deep Learning:SGD Learns Parities Near the Computational Limit](https://arxiv.org/pdf/2207.08799.pdf)
		- NeurIPS 2022
	- [PROGRESS MEASURES FOR GROKKING VIA MECHANISTIC INTERPRETABILITY](https://arxiv.org/pdf/2301.05217.pdf)
		- (당시 남긴 코멘트): [Hidden Progress in Deep Learning:SGD Learns Parities Near the Computational Limit](https://arxiv.org/pdf/2207.08799.pdf) 은 이론적으로 training 이 amplify 할 특정 메커니즘을 증명하고 이를 통해 progress measure 를 heuristic 하게 잡아낸 반면에 이 논문은 Mechanistic Interpretability 를 통해 progress measure 를 직접 지정했다.
		- (당시 남긴 코멘트): emergent 한 것처럼 보이는 역량들 밑에서 continuous 하게 증가하는 progress measure 를 잡아내는 일의 중요성... 다른 분야에서는?
	- ...
- [EMERGENT WORLD REPRESENTATIONS: EXPLORING ASEQUENCE MODEL TRAINED ON A SYNTHETIC TASK](https://arxiv.org/pdf/2210.13382.pdf)
	- ICLR 2023
	- [이 아티클](https://thegradient.pub/othello/)이 잘 정리해 놨습니다.
	- 오셀로 게임에 대해 전혀 알려주지 않은 채로 오셀로의 기보만 가지고 학습시킨 GPT 내부를 probing 해본 결과 이미 게임에 대한 이해가 생겨나고 있었다는 스토리입니다. Next word prediction 이라는 일견 단순한 task 로만 학습된 언어 모델들 안에서도 어쩌면 세상에 대한 이해가 이미 생겨나고 있을지도 모른다는 추측에 대한 좋은 비유가 됩니다.
	- 원작에서는 non-linear probing 을 사용했지만, Neel Nanda 의 [최근 작업](https://www.neelnanda.io/mechanistic-interpretability/othello)에서는 심지어 probing 이 linear 해도 된다는 점을 밝힙니다 (핵심: 특정 위치의 돌이 "무슨 색이냐" 를 묻지 말고 "지금 플레이 할 색이냐" 를 물을 것).
- [FINDING NEURONS IN A HAYSTACK: CASE STUDIES WITH SPARSE PROBING](https://arxiv.org/pdf/2305.01610.pdf)
- [Causal Scrubbing: a method for rigorously testing interpretability hypotheses](https://www.lesswrong.com/posts/JvZhhzycHu2Yd57RN/causal-scrubbing-a-method-for-rigorously-testing)
	- Redwood Research 에서 밀고 있는 Causal Scrubbing 방법론입니다.
- [Towards Automated Circuit Discovery for Mechanistic Interpretability](https://arxiv.org/pdf/2304.14997.pdf)
- [Language models can explain neurons in language models](https://openaipublic.blob.core.windows.net/neuron-explainer/paper/index.html)
	- GPT-4 로 GPT-2 에 대한 interpretation 을 시도합니다. 자세히 보지는 못했지만, interpretabilitiy 의 자동화를 시도했다는 점과 그간 이 분야에 부재했던 정량적인 metric 을 제시했다는 점에서 칭찬을 받았지만, 설명하고자 하는 바로 그 언어 모델을 설명에 사용한다는 재귀성 때문에 비판도 받은 것으로 기억합니다.
- [Interpretability at Scale:Identifying Causal Mechanisms in Alpaca](https://arxiv.org/pdf/2305.08809.pdf)
	- 개인적으로 가장 미래가 있는 접근법이라고 생각합니다. 기존 Mechanistic Interpretability 작업들에서는 아주 작은 Toy Model 을 놓고 사람이 직접 회로들을 찾았습니다. 그런 방식으로 과연 최종적으로 이해하고 싶은 대규모 언어 모델의 크기까지 도달할 수 있을지에 대해 비판이 있어 왔는데, 이 논문처럼 Causal Abstraction 을 활용하는 Atticus Geiger et al 의 접근법이 하나의 해답이 될 수 있을 거라고 생각합니다.
	- 하지만 Causal Abstraction 을 공부할 엄두가 아직은 안 납니다...
- ...


### 3. MU(無)-GPT ?

MU puzzle 은 더글러스 호프스태터의 책 <괴델, 에셔, 바흐> 에 나오는 퍼즐입니다[^wiki_mu]. 

**Axiom**: $MI \in S$  
**Rule 1:** $\forall x: xI \in S \rightarrow xIU \in S$  
**Rule 2**: $\forall x: Mx \in S \rightarrow Mxx \in S$  
**Rule 3**: $\forall x \forall y: xIIIy \in S \rightarrow xUy \in S$  
**Rule 4**: $\forall x \forall y: xUUy \in S \rightarrow xy \in S$  
**Question**: $MU \in S ?$  

MI 라는 string 에서 시작해서 주어진 string rewrite 규칙들만 가지고 MU 라는 string 에 도달할 수 있는지 묻는 이 퍼즐은, 형식 체계 (formal system) 에 대한 하나의 우화입니다. 정답부터 말하자면 MU 는 결코 얻어질 수 없는데, 모든 얻어질 수 있는 string 에 들어 있는 "I" 의 개수는 3으로 나누어 떨어지지 않기 때문입니다[^decidability].

이는 또한 지능에 대한 하나의 우화이기도 합니다. MU 가 나올 때까지 규칙들을 반복해서 적용해보는 접근법은 기계-모드 (Mechanical-mode) 라고 할 수 있고, 시스템 바깥으로 뛰쳐나와서 "I"의 개수에 대해 생각해보는 접근법은 지능-모드 (Intelligence-mode) 라고 할 수 있습니다. 애초에 시스템이 사용하는 세 개의 문자 M, I, U 는 이에 대한 암시인데, 마지막 U 는 선불교적 Un-mode 를 지칭한다고 합니다. 또 한편, 공리 MI 를 "Mechanical Intelligence" 의 약자라고 생각해 본다면[^mi_mech_interp], MI 가 MU(無) 에 도달할 수 있는지 묻는 질문은 꽤나 시적 (poetic) 으로 다가오기도 합니다.

위의 "2. Reading List" 섹션에서 소개한 [Othello GPT](https://thegradient.pub/othello/) 에선 오셀로 게임의 기보만 가지고 학습된 GPT 가 오셀로라는 게임을 이해하게 된다는 점을 보였습니다[^othello_intelligent?]. 이것은 언어 모델이 세계를 이해하고 있을 수도 있다는 하나의 암시가 됩니다[^analogy_valid]. 얼마 전 저는 비슷한 작업을 MU puzzle 위에서 반복해보면 재밌지 않을까 하는 생각을 했습니다. Othello GPT 가 정당한 오셀로 기보만 예측하도록 학습된 것과 마찬가지로, MU GPT 는 MI 로부터 얻어질 수 있는 정당한 string 만 인식하도록 학습시키면 어떨까... 학습되던 MU GPT 가 시스템 밖으로 뛰쳐나와 (말하자면 "grokking" 을 해서) "MU" 가 얻어질 확률이 0%에 가깝다고 진술하는 순간, 학습을 멈추고 모델을 해부하여 어떤 회로들이 형성되었는지 확인해보면 재밌지 않을까... 무엇보다 이 작업 전체가 역시나 너무나도 시적이지 (poetic) 않은가... 이런 생각들이었습니다.

하여, 코딩을 진행하다가, 코딩을 끝내기도 전에 이건 너무나도 당연히 잘 될 것이라는 생각이 들어서 코딩을 중단하고 말았습니다. 위에서 제시된 MIU 시스템은, 생각해보면 촘스키 위계[^chomsky_hierarchy] 에서 가장 쉬운 언어인 regular language 에 해당됩니다. 정당한 MIU-string 을 인식하는 finite state automata 를 구성할 수 있기 때문입니다. 처음엔 M만 인식하게 하고, 그 후엔 M이 들어오면 바로 실패하도록 하고, U가 들어왔을 땐 state를 바꾸지 않고, I가 들어왔을 때만 state를 바꾸도록 하는데, cycle을 이루는 state 3개를 둬서 I의 개수를 3으로 나눈 나머지를 인식하게 할 수 있습니다. 최종적으로 멈춘 state 가 I의 개수가 3으로 나누어 떨어지지 않는 state 일 때만 "yes" 를 출력하면 됩니다.

이러한 regular languague 를 Transformer 가 배우지 못할 것 같지 않았습니다. 학습된 Transformer 안에 어떤 회로들이 생길지도 대략 알 것 같았습니다. M과 U에 대한 임베딩은 attention 에서 무시될 수 있는 값으로 학습되고, 오직 I 에 해당하는 토큰들에만 attention 을 해서 그 개수를 3으로 나눈 나머지를 구하는 회로가 생기면 정답을 맞출 수 있습니다.

그리고 아니나 다를까, 최근 논문 [Transformers Learn Shortcuts to Automata](https://arxiv.org/pdf/2210.10749.pdf) 은 shallow 한 Transformer 가 임의의 finite state automata 를 배워낼 수 있다는 점을 보입니다. 특히 recurrent 한 state transition 에 대한 shortcut 이 언제나 존재해서 길이 T 인 transition 에 대해서 깊이 $O(log(T))$ 인 Transformer 로도 충분하다는 점을 보입니다 (특정 조건에서는 $O(1)$ 도 가능).

이 논문의 내포에 대해서 짚고 넘어갈 필요가 있습니다. 앞선 reading list 에서 소개 된 [A Mechanistic Interpretability Analysis of Grokking](https://www.alignmentforum.org/posts/N6WM6hs7RQMKDhYjB/a-mechanistic-interpretability-analysis-of-grokking) 의 modular addition ((x+y)%113) 같은 경우도 finite state automata 의 한 사례에 해당됩니다. 그러나 이 논문이 모든 finite state automata 에 대해서 그것을 학습한 Transformer 내부에 생길 circuit 들을 예견하는지, 즉 증명이 **구성적**[^constructive_math]인지는 제가 아직 충분히 살펴보지 못했습니다.

Transformer 가 가지는 계산-기계적 역능을 가늠해보려 한 시도는 이 논문이 처음이 아닙니다. [Thinking Like Transformers](http://proceedings.mlr.press/v139/weiss21a/weiss21a.pdf) 는 RASP 라는 Sequence 처리 언어를 고안한 후에, RASP 로 짤 수 있는 프로그램들이라면 (이론적으로) Transformer 가 배워낼 수 있음을 보입니다. DeepMind 의 [Tracr: Compiled Transformers as a Laboratory for Interpretability](https://arxiv.org/pdf/2301.05062.pdf) 는 이를 이어받아 RASP 프로그램을 Transformer weights 로 직접 컴파일 하는 파이프라인을 제안합니다. 이렇게 만들어진 Transformer 모델은, 평소와 달리 그 동작 논리를 미리 알고 있기 때문에 Interpretability 연구에 도움이 될 수 있습니다.

[Looped Transformers as Programmable Computers](https://arxiv.org/abs/2301.13196) 에서는 13개의 레이어를 가지는 Transformer 를 정교하게 설계하여 Universal Computer 로 만들 수 있음을 보입니다. 튜링-완전과 관련한 좀 더 이전 연구로는 [Attention is Turing Complete](https://www.jmlr.org/papers/volume22/20-302/20-302.pdf), [Universal Transformers](https://arxiv.org/pdf/1807.03819.pdf) 등이 있습니다[^another_computation].

etc... etc...

그러나 이 모든 것들이 과연 무엇을 예증하는 것일까요? Transformer 가 충분한 계산적 역능을 가지고 있다는 사실은 날이 갈수록 당연해지고, 그럼에도 거기서 지능이 창발하는 원리는 아직도 묘연한 상황에서, MU(無)-GPT 는 고작 한 편의 시에 불과할 뿐입니다.

> **10.3 What is actually happening?**
> 
> Our study of GPT-4 is entirely phenomenological: We have focused on the surprising things that GPT-4 can do, but we do not address the fundamental questions of why and how it achieves such remarkable intelligence. How does it reason, plan, and create? Why does it exhibit such general and flexible intelligence when it is at its core merely the combination of simple algorithmic components—gradient descent and large-scale transformers with extremely large amounts of data? These questions are part of the mystery and fascination of LLMs, which challenge our understanding of learning and cognition, fuel our curiosity, and motivate deeper research. Key directions include ongoing research on the phenomenon of emergence in LLMs (see [Emergent abilities of large language models](https://arxiv.org/abs/2206.07682) for a recent survey). Yet, despite intense interest in questions about the capabilities of LLMs, progress to date has been quite limited with only toy models where some phenomenon of emergence is proved ([Hidden Progress in Deep Learning:SGD Learns Parities Near the Computational Limit](https://arxiv.org/pdf/2207.08799.pdf), [Learning threshold neurons via the" edge of stability"](https://arxiv.org/abs/2212.07469), [Vision transformers provably learn spatial structure](https://proceedings.neurips.cc/paper_files/paper/2022/hash/f69707de866eb0805683d3521756b73f-Abstract-Conference.html) ). One general hypothesis ([Distill.pub](https://distill.pub/2020/circuits/)) is that the large amount of data (especially the diversity of the content) forces neural networks to learn **generic and useful “neural circuits”**, such as the ones discovered in ([In-context learning and induction heads](https://arxiv.org/abs/2209.11895),  [Unveiling transformers with lego: a synthetic reasoning task](https://arxiv.org/abs/2206.04301), [Transformers Learn Shortcuts to Automata](https://arxiv.org/pdf/2210.10749.pdf)), while the large size of models provide enough redundancy and diversity for the neural circuits to specialize and fine-tune to specific tasks. Proving these hypotheses for large-scale models remains a challenge, and, moreover, it is all but certain that the conjecture is only part of the answer. On another direction of thinking, (...)
> 
> --- From [Sparks of Artificial General Intelligence: Early experiments with GPT-4](https://arxiv.org/abs/2303.12712)


*교차하는 회로들과 고차원 공간의 골짜기들...*

### 4. 약간의 회의와 Personal Unalignment

글을 써야 하는 입장에서 최대한 한쪽 의견을 부각했지만, 실은 대부분의 안건에 대해서 양가적인 감정이 듭니다. 미처 쓰지 못한 생각의 잔여물들을 아래에 풀어 보았는데, 잔여물인만큼 엄청 깊게 생각해본 내용은 아니기에 제가 잘못 생각한 부분이 있다면 댓글 남겨주시면 감사할 것 같습니다.

**Q. AGI 가 올까?**

저는 이 문제에 대해 판단할 깜냥은 되지 못하기에 다른 사람들의 생각에 귀를 기울여 볼 수 밖에 없는데, 듣다 보면 오긴 할 것 같습니다. 위에서 언급한 이름들 외에도 점점 더 많은 인물들이 AGI 가 금방이라도 올 것처럼 얘기하는데 그냥 떠오르는 랜덤한 예시로는 John Carmack 이 있습니다 ([The code for AGI will be simple](https://youtu.be/xLi83prR5fg)). 또, 같은 영상에서 말하듯이, 기계가 의식을 가졌는지의 여부는 점점 덜 중요한 문제가 되어가는 것 같은데, 한때 의식이라는 것을 굉장히 신비롭고 중요하게 바라보았던 저 역시도 요즘엔 그렇게 느낍니다. "지능을 흉내낼 줄 아는 것 자체가 이미 지능이다" 라는 재귀적 정의에 동의하고 (출처를 기억할 수 없음), 또 "Essence 가 아닌 Semblance 가 모든 것이다" 라는 철학적 소견에도 동의하는 바가 있어서, 곧 AGI가 도래한다는 소문을 개인적으로 납득하기가 그다지 어렵지 않게 느껴집니다.

**Q. AI 가 인류에 그렇게 큰 위협이 될까?**

하지만 AI safety 의 중요성을 이야기하는 사람들이 말하듯이, AI 가 일순간에 인류에게 위협적인 존재가 되고 우리가 그것에 대한 통제를 잃는 상황이 일어날 것이라고는 점점 생각되지 않습니다. 일테면 self-aware 해진 AGI 가 재귀발전을 시작하여 인간의 기준에서는 찰나도 안 되는 시간 안에 초지능체가 나타나고 그렇게 되면 이미 모든 것이 돌이킬 수 없게 된다는 식의 시나리오는 ("AI foom") 개인적으로 생각하면 생각할수록 얼치기 공상과학 시나리오에 가깝게 느껴질 뿐입니다 (물론 그들은 지금 상황이 <돈 룩 업> 과 흡사한 이유가 바로 저처럼 생각하는 사람들 때문이라고 말할테지요). 도리어, AI 자체보다는 AI 를 사용하는 소규모의 사람들이 대규모의 사람들에게 지배력을 행사할 수 있게 된다는 식의 이야기는 믿을 수 있습니다.

**Q. AI Alignment 가 의미가 있을까?**

그런 견지에서 AI Alignment 가 과연 얼마나 의미가 있을까 하는 생각도 듭니다. 일단 그들이 말하는 것처럼 초지능체가 나타난다고 할 때, 그것을 Align 하려는 시도가 얼마나 성공적일 수 있을까 저는 잘 모르겠습니다. "우리보다 똑똑한 피조물이라면 무엇을 할 수 있을지 우리가 상상조차 할 수 있겠느냐" 는 식의 폭발하는 논리가 그들의 종말론의 중추를 구성하는 것 같은데, 그렇게 따지면 우리보다 똑똑한데 우리가 통제할 수 있다는 근거는 어디에 있습니까? 또 반대로 초지능체가 나타난다기 보다는 AI를 안 좋게 사용하는 인간들에 의해서 파멸적인 결과가 나오는 시나리오에서는, AI Alignment 의 의미가 더욱 더 퇴색되는 것 같은데, 모델의 내부 회로들을 다 파악하여 좋은 쪽으로 정렬하는 법을 안다는 것은, 동시에 나쁜 쪽으로 정렬하는 법을 알게 되는 것과 동전의 양면처럼 뗄 수 없는 것처럼 느껴지기 때문입니다.

**Q. Mechanistic Interpretability 가 의미가 있을까?**

AGI 든 인류 멸망이든 다 잊어버리고, Mechanistic Interpretability 만 놓고 보았을 때 이것이 나에게 의미가 있는지 생각해 봅니다. 예를 들어 Toy Model 을 연구실 GPU 위에 올려놓고 몇 달을 해부해서 회로 하나를 발견해 냈다고 생각해 봅시다. 아닙니다, 생각해보지 맙시다[^paralysis].

**회로들**

결국 삶이란 일련의 게임들을 플레이하는 것이고, 각각의 게임은 어떤 가치와 관련이 있습니다. 특정한 가치를 진심으로 믿으면서 자신의 삶을 거기에 Align 시키면서 살아가는 사람은 **Enthusiast** 가 됩니다. 어떠한 가치도 진심으로 믿지 않는 사람은 **Nihilist** 가 됩니다. 어떠한 가치도 믿지 않으면서 여전히 문제 없이 게임들을 즐기면서 살아가는 사람은 **Ironist** 가 됩니다[^ironist]. 마법이 사라지는 순간은 회로들이 파악될 때이고, 회로들을 낱낱이 파악하지 못하더라도 마법이 사라지는 기분은 가끔씩 찾아옵니다. 추천 시스템의 회로들, 자본의 회로들, 지능의 회로들, 연애의 회로들, 삶의 회로들이 불현듯 그 원리를 드러낼 때마다 저는 점점 **Ironist** 가 되어가고, 그 한가운데에서 다시금 회로들이 기계로, 여럿이 하나로, 구체가 추상으로 종합되는 신비를 여전히 이해하지 못합니다.


[^surplus]: https://blog.uvm.edu/aivakhiv/2020/06/29/we-are-surveillance-capital-stock/
[^surplus2]: https://sokionchoi.wordpress.com/2021/06/16/zuboff/
[^recwow]: 클릭률이 몇 % 증가해야 신비로운 능력이 창발했다고 할 수 있는가?
[^sparksofagi]: Sparks of Artificial General Intelligence: Early experiments with GPT-4 (https://arxiv.org/abs/2303.12712)
[^tlt]: http://proceedings.mlr.press/v139/weiss21a/weiss21a.pdf
[^gravitynlp]: 추천 모델보다는 언어 모델을 다루고 싶다는 욕망이 커진 이유 중에는 이런 것도 있습니다.
[^biased]: 혹은 저의 정보 채널이 지극히 편향되어 있었을지도 모릅니다. 세상을 읽는 일의 어려움.
[^lex_yud]: https://youtu.be/AaTRHFaaPG8
[^lex_max]: https://youtu.be/VcVfceTsD0A
[^hinton_agi]: https://www.technologyreview.com/2023/05/02/1072528/geoffrey-hinton-google-why-scared-ai/
[^max_mil]: 여담이지만 군대에서 <맥스 테그마크의 유니버스> 를 참 재밌게 읽었는데, 물리학만 하는 줄 알았던 교수가 
[^zimingliu]: https://kindxiaoming.github.io/
[^oppenheimer]: 그래서인지, 최근에 크리스토퍼 놀란 감독의 신작 <오펜하이머> 의 예고펀을 보다가, 맷 데이먼이 핵폭탄에 대해 "이건 인류 역사상 일어난 가장 중요한 일이야" 라는 대사를 치며 소리 지르는 것을 보면서도 전혀 몰입이 되지 않았습니다.
[^sincere]: 권위주의의 오류와 비슷하게 이를 진정성의 오류라고 부를 수 있을 겁니다.
[^yud_times]: https://time.com/6266923/ai-eliezer-yudkowsky-open-letter-not-enough/
[^neel_nanda_mech_interp]: https://www.neelnanda.io/mechanistic-interpretability
[^wiki_alignment]: https://en.wikipedia.org/wiki/AI_alignment
[^seri_mats_mech_interp]: https://www.serimats.org/interpretability
[^education]: 사실 인류는 아주 오랫동안 alignment 문제를 풀어왔는데 그게 바로 "교육"이라는 말을 들은적이 있습니다...
[^wiki_mu]: https://en.wikipedia.org/wiki/MU_puzzle
[^mi_mech_interp]: 혹은 "Mechanical Interpretability" 의 약자라고 생각해 본다면 (웃음)
[^othello_intelligent?]: 물론 지금 적다보니 그걸 "이해"로 봐야하는지도 점점 의문입니다. 그냥 내부 상태가 "올바르게" 구조 잡혔을 뿐인데, 그렇게 따지면 계산기는 산수를 "이해했다"고 해야 할까요?
[^analogy_valid]: 비유가 얼마나 촘촘하고 정당한지는 차치하고서라도.
[^chomsky_hierarchy]: https://en.wikipedia.org/wiki/Chomsky_hierarchy
[^not_confident]: 솔직히 혼자 생각한 거라 틀렸을 수도 있는데, 만약 그렇다면 지적 바랍니다.
[^decidability]: 사실 더 강력하게는 아예 decidable 한 criterion 을 만들 수 있는데, 자세한 건 [위키피디아](https://en.wikipedia.org/wiki/MU_puzzle) 참고 바랍니다.
[^constructive_math]: https://ncatlab.org/nlab/show/constructive+mathematics
[^another_computation]: 또는 전혀 새로운 종류의 계산-기계가 나왔다고 바라봐야 하지 않을까 하는 생각도 문득 듭니다. [Modern language models refute Chomsky’s approach to language](https://lingbuzz.net/lingbuzz/007180/v1.pdf?_s=dlDoviWbTT9izsrL) 
[^paralysis]: 언제나 생각이 앞서서 행동이 마비되는 문제.
[^ironist]: 저는 이 삼분법을 누군가에게 빚졌는데, 그 누군가는 저에게 내가 자신이 아는 사람 중 가장 큰 Ironist 라는 말을 해주었고, 저는 이상하게 그 말이 참 반가웠습니다.