---
title: "[Paper] Transformers in Time Series: A survey"
excerpt: "2022 Machine learning"
date: 2023-02-05
categories:
    - PaperReview
tags: [PaperReview]
---

# Transformers in Time Series: A survey

- survey논문들 정리해 놓은 github 페이지 [Link](https://github.com/qingsongedu/time-series-transformers-review): 
    - language, video, medical image등 다른 discipline 서베이 논문들 정리되어있음
    - spatiotemporal data 적용 연구도 있음(예. 지도)  


- time series data의 challenge:
    - capture seasonality, periodicity
    - 데이터의 long-, short- range의 temporal dependency 를 모델이 반영해야한다

## Model structructure 관점

- Positional Encoding 변형
    - learnable positional encoding - vanila가 hand-crafted인 반면, LSTM으로 sequential ordering info. 방법을 한번 더 배운다
    - Timestamp encoding - learnable embedding layer하나 추가해서 timestamp정보도 encoding에 추가한다.
- Attention module: 시간, 메모리 단축

## Application 관점

- Forecasting
    - time - series, spatial-temporal, event forcasting
- Anomaly detection
    - reconstruction model이 주로 차용됨. reconstruction error를 anomaly score로 사용.
- classification
    - Large scale transformer모델을 데려와서 fine tuning하여 높은 정확도 확보
