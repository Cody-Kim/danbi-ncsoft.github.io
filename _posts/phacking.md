---  
layout: post  
title: "p-해킹이란 무엇인가?"  
date: 2018-10-04 10:00:00  
categories: etc  
author : DANBI  
cover: "/assets/statistics.jpg"  
---

# 들어가며 

"재현성"의 위기라고 한다. 재현성이란 정확하게 표현하면 "연구 재현성research reproducibility"이다. 해당 연구를 수행한 연구자 뿐 아니라 다른 사람이 이를 반복해도 같은 혹은  거의 같은 결과가 나와야 한다는 것이다.  자연과학이나 공학에서 연구 재현성은 해당 연구를 수행한 사람 이외에 다른 사람이 실시 실험으로 구현할 수 있다. 누가 하더라도 연구가 제시한 조건 및 세부 사항이 갖춰졌을 때 기본적으로 같은 결과가 나와야 한다.

데이터를 다루는 분야에서 재현성이란 무엇일까? 데이터는 어차피 한번 생성되면 추가되거나 수정되지 않는 이상 고정된다. 이렇게 고정된 데이터를 분석할 때에도 재현성이 문제가 될 수 있을까? 노벨 경제학상을 받은 로널드 코즈가 했다는 유명한 말이 있다. 

> If you torture the data enough, nature will always confess.  데이터를 충분히 고문하면,  자연은 언제나 바른 말을 하게 될 것이다. 

데이터를 고문하다니? 백문이 불여일견! 네이트 실버의 538에서 p-해킹이란 무엇인지를 체험해 볼 수 있는 좋은 웹 서비스를 만들었다. 

[Hack Your Way To Scientific Glory](https://projects.fivethirtyeight.com/p-hacking/)

같은 데이터 셋에 대해서 여러가지 조건의 붙여서(즉 데이터를 고문해서) 당신이 원하는 '과학적' 결론을 찾을 수 있다! 왜 과학적인가? 4번 항목의 유의 확률을 보면 된다. '업계의 표준'에 따라서 이 녀석이 0.05보다 작으면 나의 결론은 과학적이다! 이렇듯 과학의 후광을 빌리기 위해서 데이터를 고문하는 것이 p-해킹이다. 사실 위 고문 사례는 무척 순진한 경우에 해당한다. 빅데이터와 컴퓨팅 자원이 저렴한 오늘날 데이터를 고문하는 데 동원할 수 있는 방법 또한 다양하다. 

# 예측 vs 사실 조건 

p-해킹의 문제를 살펴보기 전에 간단한 분류표 먼저 보자. 기계학습 혹은 통계학을 공부한 사람들이라면 한번 쯤은 봤을 법한 분류다. 이 분류는 혼동행렬(confusion matrix)이라고 부르기도 한다. 

$x$ 라는 현상은 존재하거나 존재하지 않거나 두 가지 상태만 지닌다. 이때 $x$의 상태에 관해 예측을 하고 예측이 맞았는지 여부를 확인하기 위해서는 다음의 네 가지 경우를 살피면 된다. 

|  | POSITIVE | NEGATIVE |
|--|--|--|
| **positive** | true positive | false positive  |
| **negative** | false negative |  true negative |

표에서 열(column)은 일종의 '사실 조건'을 나타낸다. 사실 조건이란 $x$가 실제로 존재하는지 여부를 POSITIVE/NEGATIVE 둘로 구분한 것이다. 행은 예측 결과를 나타낸다. $x$의 존재 유무에 대한 예측을 각각 positive/negative로 나타내자. 사실 조건과 예측에 따라서 네 가지 경우가 생성된다. 
  -  positive, negative 예측이 맞는 경우: true positive, true negative 
  -  positive, negative 예측이 틀리는 경우: false positive, false negative 

예측 결과(가설 검정의 결과)가 있을 때 true positive와 true negative의 비율을 가급적 높이는 것이 당연히 좋다. 예측은 정확할수록 좋은 것이니까. 이 매트릭스에서 예측의 설명력을 측정하는 각종 [성과 지표](https://en.wikipedia.org/wiki/Precision_and_recall)를 도출할 수 있다. 하지만 당장의 관심사는 아니니 일단 넘어가자. 

# 1종 오류와 2종 오류 

통계학을 공부한 사람은 사실 이 혼동 행렬을 한번은 접했다. 통계학에서 제일 안 외워지는 것 중 하나가 1종 오류(type I error), 2종 오류(type II error)다. 


|  | POSITIVE | NEGATIVE |
|--|--|--|
| **positive** | 1-$\alpha$ | $\alpha$ |
| **negative** | $\beta$ |  1-$\beta$ |

($\alpha, \beta \in [0,1]$)

각 행에 대해서, 즉 positive, negative 각 예측은 맞거나 틀리거나 둘 중 하나에 속한다. 표에서 예측이 맞는 경우의 비율은 $(1-\alpha)$, $(1-\beta)$이고 그렇지 않은 비율은 $\alpha$, $\beta$다. 여기서 false positive 의 비율 $\alpha$를 통계학에서는 1종 오류라고 한다. 반대로 false negative의 비율 $\beta$를 2종 오류라고 부른다. 

영가설[^1]을 통해 결과가 유의한 정도를 검정하는 통계학적 절차를 영가설 검정(Null Hypothesis Significance Test: NHST)라고 한다. NHST에서 영가설은 대체로 등호의 형태로 표현된다. 예를 들어, 어떤 회귀식의 한 계수가 $\beta_1 = 0$ 임을 검증하는 것이 NHST다. 해당 영가설이 맞다고 할 때 현재와 같은 결과를 얻을 확률이 p-value이므로, 이 값이 일정한 임계치(대체로 1%, 5%를 많이 쓴다)보다 낮을 때($p < 0.05$) 영가설을 기각하게 된다. 앞으로 영가설은 $H_0$로도 적도록 하자. 

[^1]: 보통 "귀무 가설"로 번역하지만 원어의 의미로 보면 영가설이 더 타당할 듯 싶다. 이 글에서는 영가설로 쓰도록 하자.[↩](#a1)

보통  혼동 행렬에서 존재함(효과있음)을 "POSITIVE"로 본다면 1종 오류와 2종 오류가 바뀌어야 맞을 것이다. 실제로 저자에 따라서는 둘을 바꿔쓰기도 한다. 혼란의 여지가 다소 있지만 중요한 것은 이름이 아니라 취지이므로 일단 우리는 업계의 관행을 존중하도록 하자. 

# NHST 무엇이 문제인가? (기초편)

NHST가 지닌 문제를 제대로 다루려면, 별도의 포스팅을 몇 차례는 해야 할 것이다. 일단 흔하게 저지르기 쉬운 기초적인 오류 하나 짚고, p-해킹으로 넘어가도록 하자. 

통계 패키지를 돌릴 때 $p < 0.05$와 같은 메시지가 뜨면 안도한다. 이때 마음 속에서 이런 목소리가 들린다. "주어진 데이터에서 $H_0$이 참일 확률  $p$..."  이에 솔깃했다면 다시 정신을 차려야 한다. 여기서 $p$ 값의 의미는 오히려 역(reverse)  명제에 가깝다. 

1. 현재의 데이터가 주어졌을 때, $H_0$가 참일 확률 
2. 만일 $H_0$ 참이라면, 현재의 데이터를 얻을 확률

1과 2는 같은 말인가? 고등학교 때 배운 명제를 떠올려보자. 원래 명제와 역 명제의 진리표 결과는 항상 일치할 수 없다. 그런데 우리는 종종 p값을 은근슬쩍 1처럼 해석한다. 이해를 돕기 위해 법의 맥락에서 다른 사례를 들어보겠다. 

1. $x$ 라는 증거가 발견되었을 때, 피고가 범인일 확률 
2. 피고가 범인일 때, $x$ 라는 증거를 발견할 확률 

이 두 주장이 같은 것인가? 아니다! 심지어 2는 무죄추정의 원칙이라는 형법의 원리에 어긋난다. 그리고 증거에 근거를 두고 피고의 유무죄를 판단할 때 1이 2보다는 타당하고 정의로운 접근이 아닐까? 사실 현실의 법정에서는 2를 1처럼 쓰는 경우가 많다.[^2]

[^2]: 이는 베이즈 정리와 관련된 내용이고 이 자체로 흥미로운 주제다. 일단 여기까지만 하자. 개탄할 노릇이지만 미국 법정은 베이즈 정리에 입각한 증거의 확률적 평가를 용인하지 않고 있다. (자세한 내용은 [여기](https://www.sciencenews.org/blog/context/courts%E2%80%99-use-statistics-should-be-put-trial)를 참고하라.)

# NHST 무엇이 문제인가? (p-해킹) 

NHST는 $\alpha$의 임계치를 정해 놓고 구한 p-값이 이보다 작을 경우 영가설을 기각하는 방식으로 진행된다. 잠깐만. 앞서 혼동행렬에서 우리는 네 개의 공간을 봤다. NHST가 $\alpha$를 고려한다면, $\beta$는 어떻게 되나? 효과가 없는 데도 효과가 있다고 예측할 확률, 즉 2종 오류($\beta$)가 제대로 통제되지 않아도 괜찮은 것일까? 

검정력은 $(1-\beta)$로 정의된다. 즉 대립 가설(alternative hypothesis)이 사실일 때 이를 사실로 예측할 확률이다. 즉, negative로 판정한 것 중 true negative의 비율을 의미한다. 사실 NHST는 암묵적으로 상당히 높은 수준의 검정력을 전제한다. 하지만 이 전제를 지키지 않으면 혹은 의도적으로 침묵한다면 어떤 일이 생길까?  

# Ioannidis, the destroyer 

이제 하버드 의대에 재직하는 이오니디스(John P. A. Ioannidis) 선생을 소개해야겠다. 사실 많은 전문가들이 부지불식간에 p-해킹을 저지르고 혹은 써먹고 있었지만, 이에 대해서 정식으로 반성하지 않았다. 대부분이 찜찜하게 여기고 있지만, 문제로 삼기에는 (기둥 뿌리 무너질까) 망설여 지는 그런 것이 p-해킹이 아니었을까 싶다. 

2005년 이오니디스 선생이 [폭탄](https://journals.plos.org/plosmedicine/article?id=10.1371/journal.pmed.0020124) 하나를 투하했다. 일단 제목부터 도발적이다. "왜 대부분의 출판된 학술 연구의 발견이 가짜인가?" 헐! 논문이 나온 뒤 현재까지 논문에 관한 갑론을박이 여전히 진행 중이다. 좌우간 p-해킹 만큼은 이 논문이 제대로 핵심을 보여주었다. 여기서는 [다른 사람](https://scientificallysound.org/2017/10/04/most-published-findings-are-false/)이 보다 이해하기 좋게 도해한 내용을 소개한다.  

인간은 '가시성'의 동물이다. 사람들은 보통 평범한 것보다는 튀는 걸 먼저 보게 된다. "뭘 당연한 걸 연구 씩이나!" 연구자들이 종종 듣게 되는 이야기다. 과학 하는 사람들도 인간이다. 그들 역시 가급적 세상을 놀라게 할 신기한 결과를 찾아 헤맨다. 

어떤 과학 실험을 1,000 번 할 때(보다 정확하게 표현하면 가설검정을 1,000 번 수행할 때), 그중에서 약 10%에서 신박한 결과가 나타난다고 하자. 그림으로 표시하면 아래와 같다.

![true positive](/assets/etc/p-hacking/phack_1.png)

이제 업계의 관행대로 1종 오류를 5%로 두자.($\alpha = 0.05$) 이는 false positive의 비율 즉 효과가 없는데도 효과가 있다는 예측을 얻을 확률을 5%까지 허용한다는 이야기다. 1,000 번의 실험에서 효과가 없는 900번 중에서 약 45번(= 900 X 0.05) 정도는 효과가 있는 것으로 보고된다. 

![false positive](/assets/etc/p-hacking/phack_2.png)

보통의 연구에서 $\beta$, 즉 2종 오류는 명시적으로 표기되지 않는다. 대략 업계의 관행이 20%라고 한다. 즉, false negative를 허용하는 비율이 20%다. 즉 true positive 100 개 중에서 20 개(= 100 X 0.2) 정도가 된다. 이를 역시 그림으로 표시해보자. 

![false negative](/assets/etc/p-hacking/phack_3.png)

## 이오니디스의 한방 

1,000 번의 노가다가 끝나고 나면, 우리는 45 개의 false positive와 80개의 true positive를 얻게 된다. 이오니디스의 제안은 간단하다. 제대로 했는지 알고 싶다면 positive라고 보고한 것 중에서 문제가 있는 경우(false positive)의 비율( false-positive report probability: FPRP)이 얼마나 되는지 계산해보라. 기계학습을 배운 분들이라면 이른바 recall이라는 지표를 1에서 뺀 값과 동일하다. 

$\text{FPRP} = \dfrac{\text{false positive}}{\text{false positive + true positive}}$

$\text{FPRP} = \dfrac{45}{45 + 80} = 0.36$

생각보다 높다! 유의수준 0.05(5%)이 제법 안전해 보였을일지 모르나, 이렇게 살짝 들춰보면 연구에 커다란 금이 보인다. 여기서 신뢰성은 다음과 같이 더 악화될 수도 있다. 

1. 보통 $1-\beta$는 0.8 정도라고 간주한다. 하지만 이를 엄밀하게 확인하는 경우는 많지 않다. 만일 검정력이 별로 높지 않아서 0.2에 불과하다고 해보자. 이 경우 FPRP는 0.69로 올라간다. 
2. 대부분의 가설이 FALSE이고 1% 정도만 TRUE라면? 이 경우 $\beta = 0.6$, $\alpha = 0.05$의 조건에서 FPRP는 무려 0.93이 된다. 즉, 거의 이오니디스 선생의 주장대로 대부분의 연구가 가짜 연구가 된다. 

앞서 과학자들도 신기함을 추구한다고 말했다. 놀라운 것이란 좀처럼 검증되기 힘들다는 의미이기도 하다. 해당 가설을 검정을 할 경우 대부분 negative로 보고 되기 쉽다는 것이다. 이런 분야에서 어찌해서 positive를 발견했다면 "유레카!"를 외치기 앞서 연구가 잘못된 것(false positive)일 확률부터 걱정해야 한다. 우쭐하고 감탄하기에 앞서 의심이 먼저다! 

# 정리 

이오니디스 선생은 엄밀하게 진행된 것처럼 보이는 통계적 연구조차도 상당히 높은 FPRP를 지닐 수 있음을 잘 지적했다. 더구나 2종 오류를 통제하지 않을 경우 신박함을 좇는 과학자의 자연스러운 욕망과 결합해 아주 나쁜 결과를 낳을 수 있음을 보였다. 

과학자는 '객관적 진실'을 추구하지만 연구 과정 그리고 연구를 보고하는 결과물까지 객관적일 수 없다. 여기에 과학자의 욕망이 개입하고 이에 따라서 보다 눈에 띄는 연구를 추구하려는 유인이 자연스럽게 생겨난다. 

이오니디스 선생은 충고는 상식적이다. 보기 힘든 신박한 것을 발견했다면, 먼저 삐딱하게 보라는 것이다. 과학을 회의하라는 것이 아니다. 단지 과학자의 '유인incentive'을 의심해보라는 것이다. 과학이 행해지는 인간적 맥락 너머를 보라는 것이다. p-해킹은 출세욕에 사로 잡힌 과학자의 의도적 왜곡으로 생겨날 수도 있지만, 과학을 향한 '순수한' 열정 그 자체의 산물일 수도 있겠다. 오히려 열정이 의도하지 않은 p-해킹을 낳을 수도 있다. 

자연과학이나 공학이 이렇다면, 인문학이나 사회과학은 오죽하랴. 입수한 자료를 이리저리 비틀고 고문해서 원하는 결론으로 이끄는 일이 그리 어렵지는 않을 터... 그렇다면, p-해킹을 최대한 걸러낼 수 있는 방책은 무엇일까? 이는 다음 기회로 넘기도록 하자. Stay tuned! 

p.s. 아마도 p-해킹에 관한 가장 익살스러운 묘사일 켄달 먼로의 xkcd 만화를 감상하며 글을 접는다. 

![xklcd_significance](/assets/etc/p-hacking/phack_4.png)


<!--stackedit_data:
eyJoaXN0b3J5IjpbLTY1ODE2NTM4Ml19
-->