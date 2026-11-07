# 오늘의 Study
- 반복문 (for, range, while, break, continue)
- 누적 패턴, 나머지 연산자 `%`, f-string
- 과제: 숫자 맞추기 게임
- Date: 2026-11-06
- 환경: macOS, zsh, conda(llm), Python 3.12

## Study 내용

### 1. for와 range
- `range(5)` → 0~4, `range(1, 10, 2)` → 1, 3, 5, 7, 9, `range(10, 0, -3)` → 10, 7, 4, 1
- 끝 숫자는 포함되지 않는다

```python
total = 0
for i in range(1, 101):
    total += i
print(total)   # 5050
```

- 짝수 합: `range(2, 101, 2)`는 50번, `if i % 2 == 0`은 100번 반복 + 검사

### 2. while, break, continue
- `while`은 조건이 True인 동안 반복. 처음부터 False면 블록은 실행되지 않는다
- `break`: 반복 종료 / `continue`: 이번 회차만 건너뜀
- 둘 다 **자기보다 아래 줄만** 건너뛴다

### 3. 숫자 맞추기 게임

```python
import random
answer = random.randint(1, 100)
count = 0
while True:
    number = int(input("숫자를 입력하세요 :"))
    if number > 100 or number <= 0:
        print("범위를 벗어났습니다.")
        continue
    count = count + 1
    if number < answer:
        print("더 큰 수 입니다")
    elif number > answer:
        print("더 작은 수입니다")
    else:
        print(f"정답! {count}번 만에 맞혔습니다")
        break
```

### 4. 게임을 만들며 고친 버그
### 4. 게임을 만들며 고친 버그

**① 첫 시도: 반복이 안 되고 횟수가 0**

```python
# 처음
while True:
    number = int(input("숫자를 입력하세요 :"))
    if number < answer:
        count += count
        print("더 큰 수 입니다")
    elif number > answer:
        count += count
        print("더 작은 수입니다")
    if number == answer:
            count += count
            print("정답", count , "번 만에 맞혔습니다")
    break
```

- `break`가 `while` 블록(4칸)에 있어서 정답이 아니어도 매 회차 실행 → 첫 입력 후 바로 종료
- `count += count`는 0 + 0 = 0이 계속됨
- `count`를 올리는 줄이 세 군데에 중복

```python
# 고친 후
while True:
    number = int(input("숫자를 입력하세요 :"))
    count = count + 1                  # 입력마다 한 번만
    if number < answer:
        print("더 큰 수 입니다")
    elif number > answer:
        print("더 작은 수입니다")
    else:                              # 남은 경우 = 정답
        print(f"정답! {count}번 만에 맞혔습니다")
        break                          # 정답 블록 안으로
```

**② 범위 검사 추가: 검사가 실행되지 않음**

```python
# 처음
    count = count + 1
    if number < answer:
        print("더 큰 수 입니다")
    elif number > answer:
        print("더 작은 수입니다")
    elif number > 100 or number <= 0:
        print("범위를 벗어났습니다.")
        continue
```

- `-5`는 `number < answer`에 먼저 걸려서 "더 큰 수입니다"가 나옴
- 범위 밖 숫자는 항상 위의 두 조건 중 하나에 먼저 걸리므로, 이 검사는 절대 실행되지 않음

**③ 범위 검사를 앞으로: 범위 밖 입력도 횟수에 포함됨**

```python
# 처음
    count = count + 1
    if number > 100 or number <= 0:
        print("범위를 벗어났습니다.")
        continue
    elif number < answer:
        ...
```

- `-5` → `200` → 정답 순서로 넣으면 "3번 만에"
- `continue`는 자기보다 아래 줄만 건너뛰는데, `count`는 그보다 위에 있어서 이미 늘어남

```python
# 최종
    if number > 100 or number <= 0:    # 예외를 맨 먼저
        print("범위를 벗어났습니다.")
        continue
    count = count + 1                  # 범위 안의 입력만 셈
    if number < answer:
        ...
```

## 배운 점
- 조건문과 반복문은 **줄의 순서와 들여쓰기**에 따라 결과가 바뀐다.
  게임 버그 세 개가 모두 코드가 틀려서가 아니라 줄의 위치 때문이었다.
  그래서 예외 상황은 맨 먼저 거르고, 어느 블록에 속하는지 생각하며 작성해야 한다.
- `break`와 `continue`는 실행되는 순간 **자기보다 아래 줄을 건너뛴다**.
  `break`는 반복 전체를 끝내고, `continue`는 다음 회차로 넘어간다.
  그래서 건너뛰면 안 되는 코드(안내 문구)는 그 위에, 건너뛰어야 하는 코드(횟수 세기)는 그 아래에 둔다.