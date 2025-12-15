# 📈 KOSPI Stock Price Prediction using RNN & LSTM

딥러닝 모델(RNN, LSTM)을 활용하여 KOSPI 종가(Close Price)를 예측하는 프로젝트입니다. 시계열 데이터의 특성을 반영하여 과거 데이터를 기반으로 미래의 주가를 예측하는 실험을 진행했습니다.

## 📝 Project Overview
* **Target:** KOSPI Daily Close Price
* **Data Source:** `kospi.csv` (Historical Data)
* **Models:** Vanilla RNN, LSTM
* **Goal:** Compare the performance of RNN and LSTM in time-series forecasting.
* **Reference:** [Data Science Blog](https://data-science-hi.tistory.com/190)

## 🛠️ Tech Stack
* **Language:** Python
* **Libraries:** PyTorch (or TensorFlow), Pandas, Numpy, Matplotlib
* **Environment:** Google Colab / Local

## 📊 Experiment Results

### 1. RNN Model
* **Epochs:** 5000
* **Structure:** Simple RNN layer
* **Observation:** 학습 데이터에 대해서는 패턴을 잘 따라가는 듯 보이나, 테스트 데이터에서는 실제 값보다 한 박자 늦게 따라가는 경향이 뚜렷함.

| Training Result | Test Result |
| :---: | :---: |
| <img width="1190" height="590" alt="image" src="https://github.com/user-attachments/assets/6806cbeb-748a-4773-80d0-8cd31e62f48a" /> | <img width="1189" height="590" alt="image" src="https://github.com/user-attachments/assets/c7da77b8-6d65-475c-9941-ec750ac709df" />|

### 2. LSTM Model
* **Epochs:** 500
* **Structure:** Long Short-Term Memory
* **Observation:** RNN에 비해 장기 의존성(Long-term dependency) 학습에 유리할 것으로 기대했으나, 여전히 그래프가 우측으로 밀리는(Lagging) 현상이 관측됨.

| Training Result | Test Result |
| :---: | :---: |
| <img width="1189" height="590" alt="image" src="https://github.com/user-attachments/assets/1ae1c004-a92b-4889-8da2-a65549945544" /> | <img width="1189" height="590" alt="image" src="https://github.com/user-attachments/assets/6a8b7dc6-ccae-44dc-a818-46c135857d14" />|


## 🧪 Analysis: The "Lagging" Problem (그래프 밀림 현상 분석)

실험 결과, 테스트 단계에서 예측 그래프가 실제 그래프보다 **한 박자 늦게 따라가는(Shifted to the right) 현상**이 뚜렷하게 관측되었습니다.

### Why does this happen? (원인 분석)
* **Persistence Model Behavior (지속성 모델의 함정):**
  딥러닝 모델이 데이터의 복잡한 패턴을 학습하는 대신, Loss(MSE)를 최소화하기 위한 가장 쉬운 방법인 **"내일의 주가는 오늘의 주가와 같다 ($\hat{y}_{t+1} \approx y_t$)"**라는 가중치로 수렴해버린 현상입니다.
* **Non-Stationarity (비정상성):**
  주가 데이터(Raw Price)는 시간에 따라 통계적 특성이 변하는 비정상성 데이터입니다. 이를 전처리 없이 그대로 사용할 경우, 모델은 추세(Trend)가 아닌 직전 값의 크기(Scale)에만 의존하게 됩니다.

> **Implication:** 수치적인 Loss(MSE)는 낮게 나올 수 있으나, 실제 주가의 **등락 방향(Direction)**을 예측하지 못하므로 실질적인 예측 모델로서의 성능은 미흡하다고 판단됩니다.

## 🚀 Future Works & Solutions

이러한 Lagging 문제를 해결하고 예측 성능을 고도화하기 위해 다음과 같은 전략을 도입할 계획입니다.

### 1. Target Transformation (Stationarity 확보)
* **Strategy:** 주가의 절대값(`Close Price`)이 아닌, **전일 대비 수익률(Log Returns)** 또는 **차분값(Difference, $\Delta P$)**을 예측하도록 변경합니다.
* **Goal:** 모델이 "값의 크기"가 아닌 **"변화의 패턴"**을 학습하도록 강제하여 밀림 현상을 방지합니다.

### 2. Custom Loss Function (Directional Loss)
* **Strategy:** 단순 값의 차이만 줄이는 MSE Loss 대신, **예측 방향(상승/하락)이 틀렸을 때 추가적인 페널티**를 부여하는 사용자 정의 Loss 함수를 도입합니다.
  $$Loss = MSE + \lambda \times \text{DirectionPenalty}$$

### 3. Advanced Architecture (모델 고도화)
* **Attention Mechanism:** 시퀀스 내에서 가장 최근 데이터(직전일)에만 집중하는 RNN의 특성을 완화하기 위해, 과거의 중요한 시점을 참조할 수 있는 Attention 기법을 적용합니다.
* **Transformer (Time-Series):** 순차적 처리가 아닌 전체 문맥을 파악하는 Transformer 기반 모델을 적용하여 장기 의존성(Long-term dependency) 학습 능력을 강화합니다.

### 4. Feature Engineering
* **Strategy:** 단순 종가(Close) 외에 **기술적 지표(RSI, MACD, MA)** 및 **거시 경제 지표(환율, 금리)**를 입력 변수로 추가하여 정보의 다양성을 확보합니다.
---
