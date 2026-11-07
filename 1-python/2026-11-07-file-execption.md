# 오늘의 Study
- 파일 입출력 (open, with, r/w/a 모드)
- 숫자 맞추기 게임에 최고 기록 저장 기능 추가
- 예외 처리 (try / except)
- Date: 2026-11-07
- 환경: macOS, zsh, conda(llm), Python 3.12

## Study 내용

### 1. 파일 입출력
- `"r"` 읽기 / `"w"` 덮어쓰기 / `"a"` 이어쓰기. 터미널의 cat, >, >> 랑 같음
- `with`로 열면 블록 끝날 때 알아서 닫아줌
- write는 문자열만 됨. read도 항상 문자열로 돌아옴

```python
with open("memo.txt", "w", encoding="utf-8") as f:
    f.write("첫째 줄\n")
with open("memo.txt", "w", encoding="utf-8") as f:
    f.write("둘째 줄\n")
with open("memo.txt", "a", encoding="utf-8") as f:
    f.write("셋째 줄\n")
# 결과: 둘째 줄, 셋째 줄
```

### 2. 최고 기록 저장

```python
def load_best():
    if not os.path.exists("best.txt"):
        return None
    with open("best.txt", "r", encoding="utf-8") as f:
        return int(f.read())

def save_best(count):
    with open("best.txt", "w", encoding="utf-8") as f:
        f.write(str(count))
```

- 저장: 숫자 → str() → 파일 / 불러오기: 파일 → int() → 숫자
- 파일이 없을 때 "r"로 열면 에러라서 os.path.exists로 먼저 확인
- `if best is None or best > count:` → None 확인을 앞에 둬야 count < None 에러가 안 남

### 3. 예외 처리

```python
while True:
    try:
        number = int(input("숫자를 입력하세요 :"))
    except ValueError:
        print("숫자만 입력하세요.")
        continue
```

- abc를 넣으면 ValueError가 나면서 게임이 죽었는데, try/except로 감싸서 다시 묻게 함
- 잘못된 입력은 count 올리기 전에 걸러서 횟수에 안 들어감

### 4. 고친 버그

**① save_best**

```python
# 처음
def save_best(count):
    with open("best.txt", "w", encoding="utf-8") as f:
    count = f.read()
```

- 들여쓰기 빠짐 + 저장하는 함수인데 read를 씀

```python
# 중간
    f.write(count)
# TypeError: write() argument must be str, not int
```

```python
# 고친 후
    f.write(str(count))
```

**② 최고 기록 출력**

```python
# 처음
print("f현재 최고 기록: {count}번")
# 고친 후
print(f"현재 최고 기록: {best}번")
```

- f는 따옴표 앞에. 그리고 시작할 때 count는 0이라 best를 써야 함

**③ except 안에서 다시 묻기**

```python
# 처음
except ValueError:
    number = int(input("문자가 아닌 숫자를 입력하세요 :"))
```

- except 안은 try 밖이라 두 번 연속 문자를 넣으면 죽음
- 다시 묻지 말고 안내 + continue → 맨 위의 try가 다시 물어봄

## 배운 점
- f-string은 f를 따옴표 앞에 붙인다. f"~{}~"
- Traceback은 맨 아래부터 읽는다. 맨 아래 줄이 에러 종류, 그 위가 에러가 난 줄, 더 위가 거기까지 온 경로