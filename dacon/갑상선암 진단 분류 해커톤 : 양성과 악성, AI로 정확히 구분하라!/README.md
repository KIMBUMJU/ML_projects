# 🧬 갑상선암 진단 분류 해커톤: 양성과 악성, AI로 정확히 구분하라!

**대회 기간**: 2025.05.07 ~ 2025.06.30  
**참가자 수**: 973명  
**최종 순위**: 🧪 174위 (상위 17.88%)  
**최종 점수**: `0.5093`

---

## 📊 프로젝트 개요

이 프로젝트는 Dacon에서 제공한 **갑상선과 관련된 건강 데이터를 바탕으로 양성/악성 여부를 분류하는 AI 이진 분류 모델 개발**을 목표로 진행되었습니다.

---

## 📁 데이터 구성

- `train_df`: 학습용 데이터  
- `test_df`: 테스트 데이터  
- `sample_submission`: 제출 양식 예시

> 외부 데이터는 사용하지 않았습니다.

---

## 📄 컬럼 설명

| 컬럼명              | 설명                            |
|-------------------|-------------------------------|
| ID                | 샘플 고유 ID                   |
| Age               | 나이                           |
| Gender            | 성별                           |
| Country           | 국적                           |
| Race              | 인종                           |
| Family_Background | 가족력 여부                     |
| Radiation_History | 방사선 노출 이력                  |
| Iodine_Deficiency | 요오드 결핍 여부                  |
| Smoke             | 흡연 여부                        |
| Weight_Risk       | 체중 관련 위험도                  |
| Diabetes          | 당뇨 여부                        |
| Nodule_Size       | 갑상선 결절 크기                  |
| TSH_Result        | TSH 호르몬 검사 결과              |
| T4_Result         | T4 호르몬 검사 결과               |
| T3_Result         | T3 호르몬 검사 결과               |
| Cancer            | **예측 대상 (Target)**: 갑상선암 여부 (0: 양성, 1: 악성) |

---

## ⚖️ 평가 방식

대회의 평가지표는 **Weighted MAE (가중 평균 절대 오차)** 입니다.

<p align="center">
  <img src="62c1463d-3ea3-4ff2-9f42-7b006786d5b0.png" alt="Weighted MAE" width="400"/>
</p>

\[
\text{Weighted MAE} = \frac{\sum_{i=1}^{n} w_i \cdot |y_i - \hat{y}_i|}{\sum_{i=1}^{n} w_i}
\]

- \( y_i \): 실제 정답값  
- \( \hat{y}_i \): 예측값  
- \( w_i \): 정답값의 빈도 기반 가중치  
- \( n \): 샘플 수

- **Public Score**: 테스트 데이터 중 50% 샘플링  
- **Private Score**: 전체 테스트 데이터 기준

---

## 🧰 모델링 전략

### 📌 전처리 및 피처 엔지니어링

1. **결측치 처리**  
   - 결측치 없음

2. **피처 엔지니어링**  
   - 범주형-범주형 2gram 파생 변수 생성

3. **인코딩**  
   - Label Encoding  
   - Count Encoding  
   - CatBoost Encoding

---

### 🧠 모델 학습 및 앙상블

- **사용한 모델**  
  - XGBoost, LightGBM, CatBoost, HistGradientBoosting, AdaBoost, TabNet, MLP, RandomForest, KNN, SVM 등 다양한 실험  
  - 그중 **XGBoost + LightGBM + CatBoost** 조합의 앙상블 성능이 가장 뛰어남

- **하이퍼파라미터**  
  - XGBoost, LightGBM: `n_estimators=100`, `learning_rate=0.05`  
  - CatBoost: `n_estimators=1000`, `learning_rate=0.05`

- **앙상블 전략**  
  - Hard Voting 기반  
  - 모델 가중치 비율: **XGB : LGBM : CatBoost = 3 : 3 : 4**

- **Threshold 설정**  
  - 앙상블 메타 모델의 예측 확률 기준 threshold = **0.49**

---

## 📝 회고 및 교훈

- ✅ Public Score 기준 **6위 (상위 1%)** 성적을 기록했으나, Private에서는 **174위 (상위 17.88%)**로 마감  
  → **Public에만 최적화된 모델의 위험성**을 직접 체험한 값진 경험

- ✅ 다양한 교차검증 기반의 **일반화 가능한 모델 설계의 중요성**을 깨달음  
  → Validation set 구성 시, test와 유사한 분포를 고려하는 것이 매우 중요

- ✅ 앙상블의 다양성 중요성  
  → 성능이 다소 약하더라도 **모델 간 예측 방향성이 다른 모델 조합이 성능을 끌어올릴 수 있음**

---

## 🌟 최종 결과 요약

- **총 제출 수**: 142회  
- **최종 점수**: `0.5093`  
- **최종 순위**: 🧪 174위 / 973명 중 (상위 17.88%)

---

> 이 프로젝트를 통해 교차검증, 모델 다양성, 앙상블 전략, threshold 설정의 중요성을 실무적으로 학습할 수 있었으며, 특히 public-private 점수 간 괴리 문제에 대한 교훈을 얻은 매우 유익한 대회였습니다. 📚
