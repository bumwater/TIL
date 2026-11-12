# 오늘의 Study
- 타이타닉 데이터로 생존률 분석하기
- EDA 리포트 정리
- Streamlit 앱 만들기, 배포
- Date: 2026-11-13
- 환경: macOS, zsh, conda(llm), Python 3.12
- 결과물: https://bumwater-titanic.streamlit.app

## Study 내용

### 1. 호칭으로 나이 채우기
- 어제는 전체 중앙값(28세)으로 채워서 28세에 177명이 몰림
- 이름에서 Mr, Miss, Master 같은 호칭을 단서로 뽑아 호칭별 중앙값으로 다시 채웠다.

```python
df["Title"] = df["Name"].apply(get_title)
title_median = df.groupby("Title")["Age"].median()
df["Age"] = df["Age"].fillna(df["Title"].map(title_median))
```

- Master(남자아이)가 28세 → 3.5세로 바뀜. 봉우리는 30세(Mr 119명)로 옮겨가서 완전히 없어지진 않음
- "나이가 어릴수록 생존율이 높다"고 생각했는데 pd.cut으로 확인해보니 30대가 20대보다 높았음 → 10세 이하가 다른 나이대에 비해 생존율이 높다.

### 2. Git 브랜치

```bash
git switch -c practice   # 가지 만들고 이동
git switch main          # 돌아오면 practice에서 만든 파일이 안 보임
git merge practice       # 합치기
git branch -d practice
```

- main에 새 커밋이 없었으면 앞으로 당기기만 함(fast-forward)

### 3. Streamlit
- streamlit run app.py → localhost:8501, 끄려면 Ctrl + C
- 뭘 바꿀 때마다 코드를 처음부터 다시 실행해서 CSV도 계속 다시 읽음 → @st.cache_data로 한 번만 읽게 함
- 그래프는 fig, ax = plt.subplots() → st.pyplot(fig) - 도화지 개념

### 4. 배포
- GitHub에 titanic-app 저장소 만들고 push → Streamlit Cloud에서 배포
- 맥의 AppleGothic은 리눅스 서버에 없어서 한글이 네모로 깨짐(에러는 안 남) → 나눔고딕 파일을 프로젝트에 같이 넣음
- requirements.txt는 pip freeze 말고 import한 것만 직접 씀

### 5. 따옴표 실수
- `"FILE_PATH"` → FILE_PATH라는 파일을 찾음
- `df[Survived]` → NameError
- `"font_name"` → font_name이라는 글꼴을 찾음
