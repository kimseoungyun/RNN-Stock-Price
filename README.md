# RNN-Stock-Price
RNN 계열 모델을 활용한 주가 예측 프로젝트


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


## 🧪 Conclusion & Issues
### The "Lagging" Problem (우측 편향 문제)
Window sliding (Sequence Length=10) 기법을 사용하여 최신 과거 정보를 학습시켰습니다. 실험 결과, 예측 그래프가 실제 그래프보다 오른쪽으로 밀려있는 현상이 발견되었습니다.

* **원인 분석:** 모델이 데이터의 실질적인 '패턴'이나 '추세'를 학습하기보다는, **"내일의 주가는 오늘의 주가와 가장 비슷하다"**는 가중치(Persistence Model)를 학습해버린 것으로 보입니다.
* **결과:** 급격한 변동이 일어날 때 예측이 한 템포 늦게 반응하여, 실질적인 예측 모델로서의 성능 한계가 확인되었습니다.

## 🚀 Future Works
* **Hyperparameter Tuning:** Batch size 및 Sequence length 조정
* **Model Upgrade:** Deep RNN/LSTM, GRU, Bi-LSTM 적용
* **Feature Engineering:** 단순 종가(Close) 외에 거래량(Volume), 이동평균선(MA), RSI 등 보조지표 추가
* **Data Stationarity:** 차분(Differencing) 또는 로그 변환을 통해 데이터의 정상성 확보 후 학습 시도

---
