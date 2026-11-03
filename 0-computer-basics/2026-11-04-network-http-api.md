# 오늘의 Study
- 네트워크 기초 (IP, DNS, 포트)
- HTTP 요청과 응답, 상태 코드
- API와 JSON, POST로 데이터 보내기 (curl)
- Date: 2026-11-04
- 환경: macOS, zsh

## Study 내용

### 1. IP와 DNS
- 도메인 → DNS → IP 순서로 상대를 찾는다
- DNS가 도메인을 보고 IP 로 변환해준다.
- `192.168.x.x`는 공유기 안에서만 쓰는 사설 IP, `localhost`는 나 자신

```bash
$ nslookup github.com     # 20.200.245.247
$ ping -c 3 github.com    # 평균 약 7ms
```

### 2. 포트
- IP가 건물 주소라면 포트는 창구 번호. 한 포트에는 프로세스 하나만

```bash
$ python3 -m http.server 8000   # 같은 포트에 두 번 띄우면 Address already in use
$ lsof -i :8000                 # 8000 포트를 쓰는 PID 찾기
$ kill PID
```

### 3. HTTP
- 클라이언트와 서버가 웹에서 요청과 응답을 주고받는 규칙(프로토콜)
- 요청: 방식(GET, POST 등) + 경로 / 응답: 상태 코드 + 헤더 + 본문
- 4xx는 요청한 쪽 문제, 5xx는 서버 문제
- 파이썬 파일 서버에 POST → `501` (POST 기능 자체가 없음)

### 4. API와 JSON
- API = 프로그램이 다른 프로그램에게 일을 시키는 약속된 방법
- 파일 서버의 경로는 파일 위치, API의 경로는 실행할 함수 + 재료

```bash
$ curl https://api.github.com/users/bumwater
$ curl -i -X POST https://jsonplaceholder.typicode.com/posts \
  -H "Content-Type: application/json" \
  -d '{"title": "TIL", "body": "오늘 API를 배웠다", "userId": 1}'
```

## 배운 점
- API 주소는 도메인(서버) / 경로(함수) / 값(재료)으로 나눠 읽는다.
  예: api.github.com / users / bumwater
- 같은 함수에 재료만 바꾸면 형식은 같고 내용만 달라진다.