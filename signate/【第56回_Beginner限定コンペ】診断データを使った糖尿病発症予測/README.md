# 🌿 SIGNATE 대회 정보

**대회명**: [第56回\_Beginner限定コンペ] 診断データを使った糖尿病発症予測\
**대회 기간**: 2025년 3월 1일 \~ 3월 31일\
**참가자 수**: 255명 (제출자 171명)\
**최종 순위**: 15위 (상위 8.7%)\
**최종 점수**: 0.8045857




---

## 📊 프로젝트 개요

이 프로젝트는 SIGNATE 대회에서 제공된 **진단 데이터를 기반으로 당뇨병 발병 여부를 예측하는 모델을 개발**한 것입니다.

### 📄 데이터 설명

- `train_df`: 대회 측에서 제공한 학습용 데이터
- `test_df`: 대회 측에서 제공한 테스트 데이터
- `sample_sub`: 제출 양식 예시
- `other_df`: 외부 데이터 (Pima Indians Diabetes Database)

> 최종 학습에는 `train_df` + `other_df`를 결합하여 사용하였습니다.

### 📋 컬럼 설명

| 칼럼                       | 설명                     |
| ------------------------ | ---------------------- |
| Pregnancies              | 임신 횟수                  |
| Glucose                  | 포도당 농도                 |
| BloodPressure            | 혈압                     |
| SkinThickness            | 피부 두께                  |
| Insulin                  | 인슐린 수치                 |
| BMI                      | 체질량지수                  |
| DiabetesPedigreeFunction | 당뇨병 가족력 함수             |
| Age                      | 나이                     |
| Outcome                  | 당뇨병 여부 (0: 미발병, 1: 발병) |

---

## ⚖️ 평가 방식

대회에서는 **정확도 (Accuracy)** 를 기준으로 성능을 평가하였습니다.\
공식 수식은 다음과 같습니다:

\(\text{Accuracy} = \frac{n(\{i | y_i = \hat{y}_i \})}{N}\)

---

## 🧰 모델링 전략

1. **결측치 확인**: 모든 데이터셋에서 결측치는 없었음.
2. **이상치 처리**: 모델에 따라 다르지만 기본적으로 중앙값으로 보간.
3. **교차검증**: `cv=10`, `StratifiedKFold` 방식으로 고정.
4. **모델 선택**:
   - 시도한 모델: XGBoost, LightGBM, Gradient Boosting, Logistic Regression, SVM, Random Forest, KNN
   - 최종 사용 모델: XGBoost, LightGBM, Gradient Boosting (Random Forest는 시간 문제로 제외)
5. **하이퍼파라미터 튜닝**: Optuna를 이용하여 튜닝 수행
6. **파생 변수 생성**: 관계 기반 파생변수 생성 → 성능 하락 → 사용하지 않음
7. **예측 결과 생성**
   - 3개 모델의 예측값이 같으면 유지
   - 다를 경우, **룰 기반 후처리** 적용

### 📈 Rule-based 후처리 기준 (일부 예시)

```python
rule_df['N1'] = ((rule_df['Age'] <= 30) & (rule_df['Glucose'] <= 120)).astype(int)
rule_df['N2'] = (rule_df['BMI'] <= 30).astype(int)
...
rule_score = rule_df.loc[i, ['N1','N2',...]].sum()
final_preds.append(0 if rule_score > 2 else 1)
```

8. **최종 예측 조합 방식**:
   - 7-1: XGB + LGB + GBDT + Rule 기반 예측
   - 7-2: XGB + LGB + Rule 기반 예측
   - 7-3: XGB + LGB + GBDT 하드보팅 앙상블 (후처리 X)
   - ✨ **7-1, 7-2 결과 동일 → 유지 / 다르면 7-3 결과 반영**
9. **후처리 기준은 시각화와 분포 분석을 바탕으로 설정**

---

## 🙏 회고 및 교훈

- 자격증 준비와 병행하느라 다양한 시도 실험을 충분히 못 한 점이 아쉬움
- 도메인 지식을 바탕으로 한 변수 설계와 보간법을 더 실험해볼 가치 있음
- 상위권 참가자의 노트북에서 아이디어를 참고하고 통찰 얻음
- Rule 기반 후처리 기법은 단순하지만, 의외로 유의미한 성능 향상을 보임

---

## 🌟 최종 결과

- **제출 수**: 31회
- **최종 점수**: **0.8045857**
- **순위**: **15위 / 171명 제출자 중** (상위 8.7%)
- **1등과의 점수 차이**: 0.0052111

![대회 결과 테이블](https://github.com/user-attachments/assets/0ee85f96-1ee1-4cea-92fa-b2e79e3652b1)  
![리더보드 화면](https://github.com/user-attachments/assets/b9de0a20-17b4-48df-9376-9e5fde509225)

---

감사합니다! 🙌

> 본 프로젝트는 Kaggle, SIGNATE, 그리고 오픈 소스 커뮤니티에 영감을 받았습니다.

