# 오늘의 Study
- 가계부 상대 경로 문제 해결 (__file__, print 디버깅)
- 클래스와 OOP (캡슐화, 상속, 다형성)
- NumPy 기초
- Pandas 기초, 결측치 처리
- 시각화와 EDA (타이타닉)
- Date: 2026-11-11
- 환경: macOS, zsh, conda(llm), Python 3.12, Jupyter

## Study 내용

### 1. 상대 경로 문제
- 어디서 실행했냐에 따라 record.json이 두 군데 생겼음
- `__file__`로 코드 파일 위치 기준으로 경로 고정

```python
BASE_DIR = os.path.dirname(os.path.abspath(__file__))
FILE_PATH = os.path.join(BASE_DIR, "record.json")
```
- FILE_PATH 위에 지정해놓고 코드 짜면서 실수 없애기

### 2. 클래스
- 클래스 = 틀, 객체 = 붕어빵. 데이터(속성)와 동작(메서드)을 묶음

```python
class Account:
    def __init__(self, owner, balance):
        self.owner = owner
        self._balance = balance

    def withdraw(self, amount):
        if self._balance >= amount:
            self._balance -= amount
            return True
        return False

class SavingsAccount(Account):
    def withdraw(self, amount):   # 오버라이딩
        return False
```

- `a.balance = -50000`처럼 직접 바꾸면 메서드 검사가 의미 없음 → `_balance`로 약속(캡슐화)
- 같은 withdraw인데 Account는 True, SavingsAccount는 False (다형성)

### 3. NumPy
- 리스트 `* 2`는 이어 붙이기, 배열 `* 2`는 곱하기
- 길이 다른 배열 더하면 ValueError
- `axis=0`은 행을 접어서 열별 결과

```python
scores[scores[:, 0] >= 80]   # 국어 80 이상인 행
```

### 4. Pandas와 결측치
- 행 = 한 건의 정보 전체(가로줄), 열 = 한 항목의 모든 값(세로줄)
- 걸러도 index는 원래 번호 유지

```python
df["Age"] = df["Age"].fillna(df["Age"].median())   # 열에 대고 → 열로 받음
df = df.drop(columns=["Cabin"])                     # 표에 대고 → 표로 받음
df = df.dropna(subset=["Embarked"])
```

- 처음에 `df["Cabin"] = df.drop(...)`으로 써서 틀림

### 5. EDA (타이타닉)
- 여성 생존율 74%, 남성 19%
- 3등석 여성이 1등석 남성보다 생존율 높음 → 성별 영향이 더 큼
- 10세 이하는 생존이 더 많음
- 나이 히스토그램에서 28세만 솟아 있음 → 결측치를 중앙값으로 채워서 생긴 것

## 배운 점