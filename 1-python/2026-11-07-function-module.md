# 오늘의 Study
- 함수 (def, return, 매개변수와 인자, 스코프)
- print와 return의 차이, None
- 모듈과 import, if __name__ == "__main__"
- 숫자 맞추기 게임을 함수와 모듈로 나누기
- Date: 2026-11-07
- 환경: macOS, zsh, conda(llm), Python 3.12

## Study 내용

### 1. print와 return

```python
def add_print(a, b):
    print(a + b)

def add_return(a, b):
    return a + b

x = add_print(2, 3)    # 화면에 5, x에는 None
y = add_return(2, 3)   # 출력 없음, y에는 5
```

- `return`이 없는 함수는 자동으로 `None`을 돌려준다
- 함수는 계산해서 돌려주고, 화면 출력은 바깥에서 한다

### 2. 스코프

```python
def make_total():
    total = 100
    return total

result = make_total()
print(total)   # NameError
```

- 함수 안에서 만든 이름표는 함수가 끝나면 사라진다
- `None`은 이름표는 있고 값이 빈 것, `NameError`는 이름표 자체가 없는 것

### 3. 모듈

```python
# game_utils.py
def is_valid(number):
    return 1 <= number <= 100

def check_guess(number, answer):
    if number < answer:
        return "더 큰 수입니다."
    elif number > answer:
        return "더 작은 수입니다."
    else:
        return "정답"

if __name__ == "__main__":
    print("테스트:", check_guess(30, 50))
```

```python
# game.py
import random
from game_utils import is_valid, check_guess
```

- `import`는 파일 전체를 한 번 실행한 뒤 이름표를 가져온다
- `if __name__ == "__main__":` 안의 코드는 직접 실행할 때만 동작한다

### 4. 함수를 만들며 고친 버그

**① is_valid 호출**

```python
# 처음
if is_valid(number == False):
```

- 인자가 먼저 계산돼서 `is_valid(False)`가 됨 → `-5`도 범위 검사를 통과

```python
# 고친 후
if not is_valid(number):
```

**② check_guess**

```python
# 처음
def check_guess(number, answer):
    if number < answer:
        return "더 작은 수입니다."
    else:
        return "더 큰 수입니다."
```

- 메시지 방향이 반대 (입력이 작으면 "더 큰 수"라고 알려줘야 함)
- "정답" 경우가 없어서 맞혀도 게임이 끝나지 않음 → `elif` + `else: return "정답"`

## 배운 점
- 함수 안의 값을 밖에서 쓰려면 return으로 내보내야 한다. return이 없으면 함수는 끝나지만 None을 돌려준다.
- 변수를 함수 안에 만드느냐 밖에 만드느냐에 따라 결과가 바뀐다. 함수 안의 변수는 함수가 끝나면 사라지기 때문에, 계속 기억해야 하는 값(count)은 함수 밖에 둔다.