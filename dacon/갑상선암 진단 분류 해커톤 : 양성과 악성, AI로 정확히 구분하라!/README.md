# 🧬 갑상선암 진단 분류 해커톤: 양성과 악성, AI로 정확히 구분하라!

**대회 기간**: 2025.05.07 ~ 2025.06.30  
**참가자 수**: 973명  
**최종 순위**: 🧪 174위 (상위 17.88%)  
**최종 점수**: `0.5093`

---

## 📊 프로젝트 개요

본 프로젝트는 Dacon에서 주최한 **갑상선 건강 데이터를 기반으로 갑상선암이 양성인지 악성인지 분류하는 이진 분류 모델 개발**을 목표로 한 해커톤 참여 결과입니다.

---

## 📁 데이터 구성

- `train_df`: 학습용 데이터  
- `test_df`: 테스트 데이터  
- `sample_submission`: 제출용 양식 예시

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

이 대회의 평가지표는 **Binary F1 Score**입니다.

<p align="center">
  <img width="511" height="90" alt="Image" src="https://github.com/user-attachments/assets/454e868d-88be-4d6b-9774-b7c2a29dc54f" />
</p>

- **Public Score**: 전체 테스트 데이터 중 **사전 샘플링된 30%**로 산정  
- **Private Score**: 전체 테스트 데이터의 **나머지 70%**로 산정 (최종 순위 결정 기준)

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

- **모델 실험 리스트**  
  - XGBoost, CatBoost, LightGBM, HistGB, AdaBoost, TabNet, MLP, RandomForest, KNN, SVM 등 다수의 모델 실험  
  - 이 중 **XGBoost + LightGBM + CatBoost** 조합의 앙상블이 가장 우수한 성능을 보임

- **하이퍼파라미터 세팅**  
  - XGBoost, LightGBM: `n_estimators=100`, `learning_rate=0.05`  
  - CatBoost: `n_estimators=1000`, `learning_rate=0.05`

- **앙상블 전략**  
  - 세 모델의 예측 결과를 가중 평균하여 최종 예측  
  - 가중치: `XGB : LGBM : CatBoost = 3 : 3 : 4`

- **Threshold 설정**  
  - 앙상블 메타 모델에서 예측 확률이 `0.49` 이상이면 1(악성), 아니면 0(양성)으로 분류

---

## 📝 회고 및 교훈

- ✅ **Public Score 기준 6위 (상위 1%)**라는 높은 성적에도 불구하고, **Private Score 기준 174위 (상위 17.88%)**로 마무리  
  → **Public 점수에 과적합된 모델**의 위험성과 **검증 데이터 구성의 중요성**을 절실히 체감함

- ✅ 다양한 검증 데이터에 일반화된 모델을 설계하는 것이 무엇보다 중요  
  → Cross Validation 시 test set과 유사한 분포를 갖도록 설계해야 함

- ✅ 앙상블의 다양성이 중요  
  → 비슷한 성향의 모델을 결합하는 것보다, 예측 방향성이 다른 다양한 모델 조합이 성능 향상에 기여함

---

## 🌟 최종 결과 요약

- **제출 횟수**: 142회  
- **최종 점수**: `0.5093`  
- **최종 순위**: 🧪 174위 / 973명 중 (상위 17.88%)

  <img width="1491" height="587" alt="Image" src="https://github.com/user-attachments/assets/2cb4afc5-0ccd-4bc4-bd10-fb27d63d2bf2" />
  <img width="1485" height="578" alt="Image" src="https://github.com/user-attachments/assets/c17cf978-d95a-4677-994d-06a853e66399" />

---

> 이번 대회는 단순히 성능을 끌어올리는 것에 집중하기보다, **모델의 일반화 능력**, **검증 설계의 중요성**, **다양성 기반 앙상블의 장점**을 실무적으로 학습한 소중한 기회였습니다.  
> 특히 public-private 점수 간 괴리 문제는 이후 모든 대회에 큰 영향을 줄 중요한 교훈이 되었습니다. 🚀
