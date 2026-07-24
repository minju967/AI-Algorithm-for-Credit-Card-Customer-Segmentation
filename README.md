DACON_신용카드 고객 세그먼트 분류 AI 경진대회

[대회 링크](https://dacon.io/competitions/official/236460/overview/description)

### 신용등급 분포 비율
|Segment|Count \(개수\)|Ratio \(%\)|
|---|---|---|
|E|1922052|80\.09|
|D|349242|14\.55|
|C|127590|5\.32|
|A|972|0\.04|
|B|144|0\.01|

## 2. Feature selection
- data features가 858개로 Curse of Dimensionality 또는 모델 Overfitting, 연산 속도 저하 문제 발생.
- 금융 데이터의 경우, 비즈니스 도메인 검증과 단계별 머신러닝 feature selection 필요.

### 2.1 5단계 파이프라인 프로세스
1. 비즈니스 및 도메인 기반 필터링
   - 결측치 및 단일값 제거: 결측치 비율이 80~90% 이상인 feature(column) 제거
   - is_missing 표기 고민
   - 단일값 또는 variance가 0에 가까운 feature 삭제
3. 통계적 필터 기법
4. 임베디드 및 래퍼 기법
5. 안정성 및 변수 건정성 검증
6. 권장 피처 셀렉션 워크플로우 예시

```
[전체 피처: 858개]
       ↓
(1단계) 결측치 > 80%, Zero Variance, Leakage 피처 제거  => [약 600~700개 남아있음]
       ↓
(2단계) 상관계수 > 0.85 제거 + IV(Information Value) < 0.02 제거 => [약 150~200개 남아있음]
       ↓
(3단계) LightGBM + SHAP 중요도 하위 피처 제거 => [약 40~60개 핵심 피처로 압축]
       ↓
(4단계) PSI > 0.25 안정성 결여 변수 제외 & 현업/도메인 전문가 검토 => [최종 20~40개 피처 선정]
```
