# 오늘의 Study
- 리다이렉션 (`>`, `>>`), requirements.txt, 의존성
- VS Code 인터프리터 선택
- Git 용어와 .gitignore
- 파이썬 자료형, 변수(이름표), 불변과 가변
- 조건문 (if / elif / else)
- Date: 2026-11-05
- 환경: macOS, zsh, conda(llm), Python 3.12

## Study 내용

### 1. 리다이렉션과 requirements.txt
- `echo`는 출력만 한다. 파일에 쓰는 건 `>`(덮어쓰기), `>>`(이어 붙이기)
- pandas만 설치해도 numpy, python-dateutil, tzdata가 의존성으로 같이 설치된다
- 의존성 확인 : python -m pip show 확인하고 싶은 패키지?

```bash
$ python -m pip freeze > requirements.txt      # 상자 기록
$ python -m pip install -r requirements.txt    # 기록대로 재현
```

### 2. Git
- 작업 폴더 → `add`(상자에 담기) → `commit`(확정) → `push`(GitHub에 올리기)
- `.gitignore`에 `.env`를 적어두면 API 키 파일이 올라가지 않는다

### 3. 자료형과 변수
- `3 + 3` → `6`, `"3" + "3"` → `'33'`, `3 + "3"` → `TypeError`
- 변수는 값에 붙이는 이름표. `b = a` 후 리스트를 고치면 a도 바뀐다 → 따로 만들려면 `a.copy()`

### 4. 조건문

```python
age = int(input("나이를 입력하세요 : "))

if age < 0 or age > 120:
    print("잘못된 나이입니다.")
elif age < 8 or age >= 65:
    print("무료")
elif age <= 19:
    print("5000원")
else:
    print("10000원")
```

- 위에서부터 처음 True인 블록 하나만 실행 → 예외 상황을 맨 먼저 둔다
- `input()`은 항상 문자열이라 `int()`로 바꿔야 한다

## 배운 점
- 가상환경의 중요성을 다시 깨달았고, 프로젝트를 시작할 때 챙겨야 할 파일 (requirements.txt: 상자 기록, .gitignore: Git 무시 목록, .env: 비밀값)을 정리했다.
- python 의 기본적인 문법이지만 b = a 는 복사가 아닌 같은 값에 붙여진 이름표이다. > b.append(3) 을 하면 a 도 append 가 적용 된다. 복사본은 .copy() 로 생성
- 조건문은 위에서부터 처음 True인 것 하나만 실행된다(PATH처럼). 그래서 순서에 따라 결과가 달라지므로, 예외 상황을 먼저 두고 이미 걸러진 조건은 다음 조건에서 반복하지 않는다.