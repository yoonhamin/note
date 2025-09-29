# cp (copy)

### 기능
- 로컬 시스템에서 **파일 또는 디렉토리를 복사**

### 옵션
- `-r` : 디렉토리 복사 (재귀적으로)

### 예제
```bash
cp a.txt b.txt        # 파일 복사
cp -r dir1 dir2       # 디렉토리 복사

파일 위치 기준 복사

복사 대상이 있는 디렉토리에서 → 다른 디렉토리로 복사
# 현재 위치: /home/user/
cp a.txt /tmp/               # a.txt를 /tmp/ 디렉토리로 복사


복사 대상과 복사 위치에 있지 않은 상태에서 복사
# 현재 위치: /etc/
cp /home/user/a.txt /tmp/   # 절대경로를 사용하여 복사


복사 위치에 있는 상태에서 → 다른 디렉토리의 파일을 복사
# 현재 위치: /tmp/
cp /home/user/a.txt .       # /home/user/에 있는 a.txt를 현재 디렉토리(/tmp/)로 복사
```



# scp 

### 기능
- 로컬 ↔ 원격 서버 간 파일 또는 디렉토리 복사 (SSH 프로토콜 사용)
### 기본 형식
- scp [옵션] 파일명 사용자@서버주소:경로
### 옵션

- `-r` : 디렉토리 복사
- `-P` 포트번호 : SSH 포트 지정 (기본 22)

### 예제
```bash
scp filename server_name:path             # 파일을 원격 서버 경로로 복사
scp ./aaa.txt swist2.cbnu.ac.kr:~/        # aaa.txt → 서버 홈 디렉토리
scp ./aaa.txt swist2.cbnu.ac.kr:~/bbb.txt # aaa.txt → 서버에 bbb.txt 이름으로 저장
scp -r ./dir1 swist2.cbnu.ac.kr:~/        # dir1 디렉토리 전체를 서버 홈으로 복사
scp -r ./dir1 swist2.cbnu.ac.kr:~/dir2    # dir1 → 서버의 dir2로 복사

```


# 제출


- 로컬 시스템에서 **파일 또는 디렉토리를 복사**
- 제출 디렉토리(`/submit/`)로 파일을 이동시키는 데 사용

```bash
cp sp.ex1.2021076002.yoonhamin.zip /submit/

ls -aFl /submit2    #확인
