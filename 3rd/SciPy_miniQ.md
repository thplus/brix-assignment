## SciPy Mini-Quest
[Colab link](https://colab.research.google.com/drive/1JtQII7PMk0anb-Q3BGwXKAeHLyvk22yI?usp=sharing)</br>
1. [정규분포(Normal Distribution)](#1-정규분포normal-distribution)
2. [기술통계(Descriptive Statistics)](#2-기술통계descriptive-statistics)
3. [가설검정(Hypothesis Testing)](#3-가설검정hypothesis-testing)
4. [통계적 시각화(Statistical Visualization)](#4-통계적-시각화statistical-visualization)
### 1. 정규분포(Normal Distribution)
1. 평균 `60`, 표준편차 `15`를 갖는 정규분포에 500개의 데이터를 생성한 후 데이터의 기본 통계 정보(평균, 표준편차, 최소값, 최대값)를 출력하는 코드를 작성하세요.</br>
    샘플 데이터
    ```python
    data = np.random.normal(loc=60, scale=15, size=500)
    ```
    풀이
    ```python
    stats.describe(data)
    ```
    출력
    ```
    DescribeResult(nobs=500, minmax=(17.371887113855024, 102.01757033697038), mean=60.16439166661173, variance=202.4669475100948, skewness=-0.01988836442802157, kurtosis=0.03347222642531866)
    ```
2. 평균 `50`, 표준편차 `10`을 갖는 정규분포에서 특정 값 `x = 65`의 확률 밀도 함수(PDF)값을 계산하고 출력하는 코드를 작성하세요.</br>
    풀이
    ```python
    stats.norm.pdf(65, loc=50, scale=10)
    ```
    출력
    ```
    0.012951759566589175
    ```

3. 평균 `70`, 표준편차 `8`을 갖는 정규분포에서 (1) 특정 값 `x = 80`이하일 확률을 CDF로 계산하고 (2) 상위 5%에 해당하는 점수를 PPF로 계산하여 출력하는 코드를 작성하세요.</br>
    풀이
    ```python
    print(stats.norm.cdf(80, loc=70, scale=8))
    print(stats.norm.ppf(0.95, loc=70, scale=8))
    ```
    출력
    ```
    0.8943502263331446
    83.15882901561177
    ```
### 2. 기술통계(Descriptive Statistics)
1. 주어진 데이터에서 평균과 중앙값의 차이를 계산하는 코드를 작성하세요.</br>
    샘플 데이터
    ```python
    np.random.seed(42)
    data = np.random.normal(loc=50, scale=10, size=100)
    df = pd.DataFrame(data, columns=["value"])
    ```
    풀이
    ```python
    np.mean(df['value']) - np.median(df['value'])
    ```
    출력
    ```
    0.2310977438562034
    ```
2. 데이터에서 이상값(Outlier)을 찾아 제거한 후 원래 데이터와 이상값 제거 후 데이터의 평균을 비교하는 코드를 작성하세요. 이상값은 IQR(사분위 범위)를 사용하여 탐지하세요.</br>
    샘플 데이터
    ```python
    np.random.seed(42)
    data = np.random.normal(loc=50, scale=10, size=100)
    df = pd.DataFrame(data, columns=["value"])
    ```
    풀이
    ```python
    Q1 = np.percentile(df['value'], 25)
    Q3 = np.percentile(df['value'], 75)
    IQR = Q3 - Q1

    lower_bound = Q1 - 1.5 * IQR
    upper_bound = Q3 + 1.5 * IQR

    outliers = df[(df['value'] < lower_bound) | (df['value'] > upper_bound)]

    print(len(outliers))
    print(outliers.reset_index())
    ```
    출력
    ```
    1
    index      value
    0     74  23.802549
    ```
3. 데이터의 왜도(Skewness)와 첨도(Kurtosis)를 계산하여 데이터의 분포 특성을 분석하는 코드를 작성하세요.</br>
    샘플 데이터
    ```python
    np.random.seed(42)
    data = np.random.normal(loc=50, scale=10, size=100)
    df = pd.DataFrame(data, columns=["value"])
    ```
    풀이
    ```
    skew = df['value'].skew()
    kurt = df['value'].kurtosis()

    print(skew, kurt)
    ```
    출력
    ```
    -0.1779481426259557 -0.10097745347286446
    ```
### 3. 가설검정(Hypothesis Testing)
1. 단일 표본 t-검정(One-Sample t-test)을 수행하여 샘플 데이터의 평균이 특정 값과 유의미한 차이가 있는지 검정하는 코드를 작성하세요. (평균 `50`, 표준 편차 `5`를 따르는 정규 분포에서 30개의 데이터를 생성하고, 해당 데이터가 평균 `52`와 차이가 있는지 확인하세요.)</br>
    샘플 데이터
    ```python
    np.random.seed(42)
    sample_data = np.random.normal(loc=50, scale=5, size=30)
    ```
    풀이
    ```python
    t_stat, p_value = stats.ttest_1samp(sample_data, 52)
    print(f"t-통계량: {t_stat}")
    print(f"p-값: {p_value}")
    ```
    출력
    ```
    t-통계량: -3.5793224601074907
    p-값: 0.0012367258279248469
    ```
2. 카이제곱 검정(Chi-Square Test)을 수행하여 관측된 데이터와 기대값이 유의미한 차이가 있는지 확인하는 코드를 작성하세요.</br>
    샘플 데이터
    ```python
    observed = np.array([50, 60, 90])

    expected = np.array([66, 66, 66]) * (observed.sum() / np.sum([66, 66, 66]))
    ```
    풀이
    ```python
    chi2_stat, p_value = stats.chisquare(f_obs=observed, f_exp=expected)
    print(f"카이제곱 통계량: {chi2_stat}")
    print(f"p-값: {p_value}")
    ```
    출력
    ```
    카이제곱 통계량: 13.0
    p-값: 0.0015034391929775717
    ```
3. 분산 분석(ANOVA, Analysis of Variance)을 수행하여 여러 그룹의 평균이 서로 다른지 검정하는 코드를 작성하세요.</br>
    샘플 데이터
    ```python
    np.random.seed(42)
    group_1 = np.random.normal(loc=50, scale=10, size=30)
    group_2 = np.random.normal(loc=55, scale=10, size=30)
    group_3 = np.random.normal(loc=60, scale=10, size=30)
    ```
    풀이
    ```python
    anova_stat, p_value = stats.f_oneway(group_1, group_2, group_3)
    print(f"분산 분석 통계량: {anova_stat}")
    print(f"p-값: {p_value}")
    ```
    출력
    ```
    분산 분석 통계량: 12.20952551797281
    p-값: 2.1200748140507065e-05
    ```
### 4. 통계적 시각화(Statistical Visualization)
1. `NumPy`를 사용하여 평균 `70`, 표준편차 `20`을 따르는 정규분포 데이터 1,000개를 생성한 후, `Matplotlib`을 활용하여 박스플롯을 그리는 코드를 작성하세요.</br>
    샘플 데이터
    ```python
    np.random.seed(42)
    data = np.random.normal(loc=70, scale=20, size=1000)
    ```
2. 평균이 각각 `55`와 `60`이고, 표준편차가 `8`인 두 개의 그룹(A, B) 데이터를 생성한 후, 두 그룹의 데이터 분포를 `Seaborn`을 활용하여 KDE(커널 밀도 함수)와 함께 히스토그램으로 시각화하세요. 이후 두 그룹 간 평균 차이가 유의미한지 t-검정을 수행하는 코드를 작성하세요.</br>
    샘플 데이터
    ```python
    np.random.seed(42)
    group_A = np.random.normal(loc=55, scale=8, size=200)
    group_B = np.random.normal(loc=60, scale=8, size=200)
    ```
3. 광고 A를 본 500명 중 120명이 클릭하였고, 광고 B를 본 500명 중 150명이 클릭을 한 데이터가 있습니다. 이 데이터를 바탕으로 카이제곱 검정을 수행하여 광고 A와 B의 클릭률 차이가 유의미한지 분석하고, Seaborn의 barplot을 사용하여 클릭률을 비교하는 그래프를 그리는 코드를 작성하세요.</br>
    샘플 데이터
    ```python
    observed_data = pd.DataFrame({
        "Ad_A": [120, 380],
        "Ad_B": [150, 350]
    }, index=["Click", "No Click"])
    ```