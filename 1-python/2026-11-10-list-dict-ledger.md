# 오늘의 Study
- 리스트와 딕셔너리
- JSON 저장/불러오기 (json.dump, json.load)
- 가계부 프로그램 설계와 완성
- Date: 2026-11-10
- 환경: macOS, zsh, conda(llm), Python 3.12

## Study 내용

### 1. 리스트와 딕셔너리
- 기록 하나 = 딕셔너리, 기록 여러 개 = 딕셔너리들의 리스트
- GitHub API 응답이랑 같은 모양 `[ {...}, {...} ]`

```python
records = [
    {"date": "2026-11-08", "type": "지출", "item": "식비", "amount": 12000},
    {"date": "2026-11-08", "type": "수입", "item": "용돈", "amount": 50000},
]
records[1]["item"]   # 왼쪽부터: records[1] → 딕셔너리 → ["item"] → "용돈"
```

### 2. 가계부 설계
- add_record / load_records / calc_total 로 역할 나눔
- 불러오기와 계산은 따로. 한 함수는 한 가지 일만
- 작업은 RAM(records 리스트)에서, 바뀌면 바로 SSD(record.json)에 저장

### 3. JSON
- f.read()는 문자열, json.load(f)는 리스트/딕셔너리로 되살려줌
- 파일이 없으면 None 대신 [] → for, calc_total, append가 처음 실행에도 그냥 됨

```python
FILE_PATH = "record.json"

def load_records():
    if not os.path.exists(FILE_PATH):
        return []
    with open(FILE_PATH, "r", encoding="utf-8") as f:
        return json.load(f)

def save_records(records):
    with open(FILE_PATH, "w", encoding="utf-8") as f:
        json.dump(records, f, ensure_ascii=False, indent=2)
```

### 4. 고친 버그

**① records와 record**
- `records["type"]` → TypeError: list indices must be integers
- 리스트 전체가 아니라 for로 꺼낸 하나(record)에서 꺼내야 함

**② total 위치**

```python
# 처음
total = 0
def calc_total(records):
    for record in records:
        total = total - record["amount"]
# UnboundLocalError
```

- 함수 안의 total은 밖의 total이랑 다른 지역 변수 → total = 0을 함수 안으로

**③ load_records**

```python
# 처음
content = f.load()      # AttributeError: load는 파일 기능이 아님
content = json.load()   # 어느 파일인지 안 넘김
# 고친 후
content = json.load(f)
```

- 파일 이름도 load는 record.txt, save는 record.json으로 달랐음 → FILE_PATH 상수로 한 곳에

**④ append 빠짐**

```python
# 처음
record = {"date": date, "type": kind, "item": item, "amount": amount}
save_records(records)
print("기록되었습니다.")
```

- "기록되었습니다"는 나오는데 전체 보기는 비어 있고 합계 0원
- 딕셔너리만 만들고 리스트에 안 넣고 저장함 → `records.append(record)` 추가

## 배운 점
## 배운 점
- records(리스트 전체)와 record(꺼낸 하나)는 s 하나 차이지만 완전히 다르다. -> 이름을 복수형, 단수형으로 지어야겠다.
- 같은 파일 이름을 상수(FILE_PATH)로 미리 정의 해놓자.
