# 📈 Bitcoin Price Prediction with LSTM  

[![Open Lab Notebook in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/camargokr/TimeSeriesForecastingTest/blob/main/lab_notebook.ipynb)
[![Open Assignment in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/camargokr/TimeSeriesForecastingTest/blob/main/assignment_notebook.ipynb)

- 머신러닝을 활용하여 비트코인의 가격 변화 방향을 예측하고, **수익률을 극대화하는 트레이딩 전략**을 개발하는 실습 프로젝트입니다.


---

## 📄 제출용 보고서 답안 정리

### 1. 모델 아키텍처
- LSTM 기반 시계열 예측 모델  
- 입력 차원: (sequence_length, feature_dim)  
- 주요 구성 요소  
  - `LSTM(hidden_size)`  
  - `Dropout(0.2)`  
  - `Dense(1)` – 상승 확률 출력

---

### 2. 선택 이유
- LSTM은 시계열 데이터의 **장기 의존성(long-term dependency)** 학습에 강함  
- 비트코인 가격 데이터는 연속적 패턴이 많아 LSTM이 적합  
- 예제보다 **더 깊은 상태 표현(hidden state)** 학습 가능

---

### 3. 트레이딩 전략
- 예측 확률 기반 매매  
- threshold 이상일 경우만 진입  
- `position_scaling=True`로 확률 비례 투자  
- 거래 수수료 고려  

---

### 4. 하이퍼파라미터
- **hidden_size:** 64  
- **learning_rate:** 0.001  
- **threshold:** 0.6  
- **position_scaling:** True  

---

### 5. 예제와의 차별점
- threshold 상향 → 더 보수적인 거래  
- position scaling 적용 → 리스크 관리 강화  
- GPU 제한으로 CPU 기반 훈련 → 속도 및 성능 약간 감소 가능  

---

## 💻 CPU 사용 이슈 (중요)
Google Colab은 GPU 사용 시간이 제한되어 있기 때문에  
테스트 과정의 대부분을 **CPU로 실행**했습니다.

CPU 실행 시:
- 훈련 속도 느림  
- 예측 반복 시 지연 가능  
- 일부 테스트에서 최적 수익률이 낮아질 수 있음  

보고서에 명시할 핵심 문장:

> "Google Colab의 GPU 사용 제한으로 인해 모델 테스트는 CPU에서 수행되었으며, 이로 인해 성능이 다소 제한될 수 있습니다."

---

## 📊 결과 분석 및 고찰

### 1. 모델 성능 분석
- **Buy and Hold 대비 수익률:** 트레이딩 결과가 일정 구간에서는 손실이 발생하였으나 threshold 조절과 position_scaling을 통해 손실 폭을 제한하는 효과 확인됨.  
- **모델 예측 정확도:** 훈련 정확도는 높았지만 검증 정확도는 약 50% 내외로 변동성이 큰 시장 특성상 오버피팅 경향 발견  
- **주요 성공/실패 시기:**
   - 시장이 명확한 추세(상승/하락)일 때는 모델이 비교적 정확히 예측
   - 박스권 / 횡보장에서는 확률이 0.5 부근으로 모호해져 거래 기회가 줄고 성능 약화

---

### 2. 트레이딩 전략 분석
- **선택한 전략:** 확률 기반 투자 비중 결정(position_scaling) 및 높은 threshold 기반 보수적 전략
- **장점:**
   - 불필요한 거래 줄임 → 수수료 절감
   - 자신 없는 신호에서는 시장에 참여하지 않아 손실 방지 효과
   - 확률 기반 비례 투자로 리스크 자동 조절

- **단점:**
   - threshold가 높으면 거래 기회가 감소해 수익 기회도 줄어듦
   - 모델 예측 정확도가 높지 않을 경우 수익률의 한계 존재
   
- **수수료 영향:** threshold 높일수록 긍정적  

---

### 3. 모델 설계
- **아키텍처 선택 이유:**
   - GRU는 RNN/LSTM 대비 학습 속도가 빠르고 성능도 우수
   - Dropout + Noise 조합으로 일반화 성능 개선

- **하이퍼파라미터 튜닝:**
   - hidden_size, threshold, learning_rate, dropout 실험
   - threshold 조정이 가장 큰 성능 영향 요인
   
- **예제 모델과의 차이점:**
   - GRU 추가 및 Dropout, Noise 도입
   - 확률 스케일링 전략
   - threshold 기반 보수적 진입 기준

---

### 4. 개선 방향
- **한계점:**
   -  검증 정확도와 수익률 모두 개선 여지 큼
   -  시계열 길이가 1로 고정되어 패턴 학습이 제한적

- **추가 실험 아이디어:**
   -  seq_len=10~30으로 확대해 패턴 학습 강화
   -  CNN+GRU 하이브리드 모델 도입
   -  가격 변화율(returns) 기반 라벨링 재정의
   -  트레이딩 전략에 RSI/EMA 등 기술지표 추가 통합
   
- **실전 고려사항:**
   - 거래 수수료 및 슬리피지 발생 고려 필요
   - 모델이 시장이 예측 불가능한 뉴스 이벤트에 취약
   - 지나친 과적합 방지를 위한 정기 리트레이닝 필요

---

## 🧑‍💻 개발자 Breno,  brca@hufs.ac.kr , 한국외국어대학교 외국어로서의한국어통번역 + AI융합

