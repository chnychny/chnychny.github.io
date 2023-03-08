# [Paper] ICLR22_Anomaly Transformer

Category: Topics4Study
Memo: ICLR 2022 spotlight paper

Time series anomaly detection with association discrepency

torch>1.4 github 공유됨 (github.com/thuml/Anomaly-Transformer)

고려대학교 산업경영공학부 DSBA연구실 유튜브 강의를 많이 참조함()

### 비지도 시계열 이상치 탐지의 목적

- 수많은 이상치 중에 일부가 이상치. 그 일부의 criteria를 학습할 수 있어야한다
- informative representations 가능

## Limitations on 시계열 AD

기존 시계열AD 일반화 어려움, temporal information을 고려하지 못한다  

그래서 딥러닝을 사용하면서 temporal정보를 넣는데(RNN)  

이상치가 매우 적어서 정보가 묻히는 경향을 충분히 고려하기 어려움. 

Unupervised AD 중 RNN(LSTM)의 point-wise representation: less informative, can be dominated by normal patterns

- Density estimation
- Clustering based
- Reconstruction based

## Association discrepancy?

- point-wise를 점이 아니라 distribution으로 고려한다
- 이상치는 전반 시계열(global)에는 약한 상관관계를 가지지만, 인접영역(local)에는 강한 상관관계를 갖는다고 가정
- Association Discrepancy(연관불일치값) = 각 time point의 prior association distance (인접) + Series association distance (글로벌)

Why Transformer?

- 시간정보가 한번에 들어갈 수 있다. (long range temporal dependencies학습가능)

## Method

- prior association → 시그마
    - 각 시점에 대해 가우시안 분포 통과시켜 파라미터 학습: 이상치point에서 낮은 sigma
    - row-wise scaling
    - 주변시점에 가중치를 둔 att. 값을 scaling
- series-association → Q, K, V
    - Q,K에 대해 multi-head att.
    - 기존 transformer와 동일한 att.값
- AssDis(prior, series;X) = 레이어별로 (KL(prior | series) + KL(series | prior)) 평균
- 이상치인 경우 series, prior association이 모양이 비슷함. KLD값이 작음 → 이상치
- 정상일때는 KLD가 크다 (prior association은 분포가 있는데, series association은 값이 다 작을 것이므로)
- Minmax Association learning사용
    - Ltotal = reconst.Loss -lambda(Minimax.AssDis)
    - 그냥 ASSDIS를 크게 만들면, 가우시안 커널만 엄청 커지게되서 local association이 의미없게 된다 (분산이 넓어짐)
    - 그래서 local association이 series(global)의 temporal feature를 따라 배울수 있게끔 Minimize phase에서 진행. 그리고나서 maximize phase는 reconstruction error낮춰야하니까 series association이 그렇게 커질수는 없음. 그 상태에서 ASSDISS가 커지도록(S와 P가 많이 달라지도록) 학습
    - Mini phase를 사용해서 series ass. 와 temporal part가 조금 더 유사하게 되도록 만든다. Assdiss는 크게 만들어서
    - 

TS 알고리즘 중에 90%를 넘길수 있는 데이터는 많지 않다. 

Experiment에서 Table windowsize = 100, non-overlapping

official code로 잘 돌아감. 

- loss가 minus를 찍음→ 학습방식

Total loss = reconstruction loss(RMSE) + lambda[Assdis_loss]

Assidis_loss = KLD(P|S) + KLD(S|P) [이상치일 때, 큼]

Minimax 방식으로 학습

- Mini: series를 고정시키고, prior의 gaussian kernel이 너무 뾰족해지지 않고 글로벌한 신호의 성향을 닮을수 있도록(KLD(P|S)가 작아져야되니까 마이너스) 학습하기 위해
- Max: gausian kernel값 고정된 상태에서, KLD를 키운다(고정된 P와 S분포의 차이가 커지게끔 S를 조정한다. 그럼 S는 global한 부분에 초점을 맞추게 되고, 전체적으로 reconst 잘되는 방향으로 학습하게 된다. 근데 anomaly상태에서는 reconst error도 크고, KLD도 크니까 Ltotal 이 크게 나온다.  → 어렵다….