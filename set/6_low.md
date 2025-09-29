# 저수준 입출력 (Low-level I/O)

운영체제의 시스템 호출을 직접 사용 (open, read, write, close)
- unistd.h와 fcntl.h 필
- 버퍼링이 없고, 파일 디스크립터를 사용

```c
#include <stdio.h>
#include <fcntl.h>    // open
#include <unistd.h>   // read, write, close
int main() {
    int fd;
    char buffer[100];
    ssize_t bytes_read;

    // 파일 열기 (읽기 전용)
    fd = open("low_level.txt", O_RDONLY);
    if (fd == -1) {
        perror("파일 열기 실패");
        return 1;
    }
    // 읽기
    bytes_read = read(fd, buffer, sizeof(buffer)-1);
    if (bytes_read == -1) {
        perror("읽기 실패");
        close(fd);
        return 1;
    }
    buffer[bytes_read] = '\0';  // 문자열 종료

    // 출력
    write(STDOUT_FILENO, buffer, bytes_read);

    // 파일 닫기
    close(fd);

    return 0;
}
```
### 특징
- read와 write는 바이트 단위로 동작
- 파일 디스크립터(int fd) 사용
- 버퍼링 없음 → 작은 단위 입출력 시 성능 저하 가능

```c
int open(const char *path, int oflag [, mode_t mode]);

int close(int fildes);
```

### 파일 열기 옵션 (oflag) 정리

| 옵션 | 설명 |
|------|------|
| `O_RDONLY` | 파일을 읽기 전용으로 연다 |
| `O_WRONLY` | 파일을 쓰기 전용으로 연다 |
| `O_RDWR`   | 파일을 읽기와 쓰기가 가능하게 연다 |
| `O_CREAT`  | 파일이 없으면 새로 생성한다 |
| `O_EXCL`   | `O_CREAT`과 함께 사용할 경우, 파일이 이미 있으면 오류를 출력하고 생성하지 않는다 |
| `O_APPEND` | 파일의 맨 끝에 내용을 추가한다 |
| `O_TRUNC`  | 파일이 이미 존재하고 쓰기 옵션으로 열 경우, 내용을 모두 지우고 길이를 0으로 만든다 |
| `O_NONBLOCK` / `O_NDELAY` | 비블로킹(Non-blocking) 입출력 |
| `O_SYNC` / `O_DSYNC` | 저장장치에 쓰기가 끝나야 쓰기 동작을 완료 |

### 파일 권한 모드 (Mode) 옵션

| 플래그       | 설명                                | 해당 사용자/그룹/기타 |
|-------------|-----------------------------------|---------------------|
| `S_IRUSR`   | 소유자(owner)가 읽기 가능           | 사용자(User)        |
| `S_IWUSR`   | 소유자(owner)가 쓰기 가능           | 사용자(User)        |
| `S_IXUSR`   | 소유자(owner)가 실행 가능           | 사용자(User)        |
| `S_IRGRP`   | 그룹(Group)이 읽기 가능             | 그룹(Group)         |
| `S_IWGRP`   | 그룹(Group)이 쓰기 가능             | 그룹(Group)         |
| `S_IXGRP`   | 그룹(Group)이 실행 가능             | 그룹(Group)         |
| `S_IROTH`   | 기타(Others)가 읽기 가능            | 기타(Others)        |
| `S_IWOTH`   | 기타(Others)가 쓰기 가능            | 기타(Others)        |
| `S_IXOTH`   | 기타(Others)가 실행 가능            | 기타(Others)        |
```c
  fd = open("unix.txt", O_CREAT | O_EXCL, 0644);
      open() 함수 호출: 파일 열기 또는 생성
      "unix.txt": 파일 이름
      O_CREAT | O_EXCL:
            O_CREAT: 파일이 없으면 새로 생성
            O_EXCL: 파일이 이미 존재하면 오류 발생
      0644: 파일 권한 모드
        소유자: 읽기 + 쓰기 (rw-)
        그룹: 읽기 (r--)
        기타: 읽기 (r--)
```

### 추가 파일 열기 옵션 (oflag)

| 옵션 | 설명 |
|------|------|
| `O_CLOEXEC` | `exec()` 호출 시 파일 디스크립터가 자동으로 닫히도록 설정 |
| `O_DIRECTORY` | 디렉터리만 열 수 있도록 강제 |
| `O_NOCTTY` | 터미널 장치를 열어도 controlling terminal로 설정하지 않음 |
| `O_NOFOLLOW` | 심볼릭 링크를 따라가지 않고 링크 자체를 열 때 사용 |
| `O_TMPFILE` | 임시 파일 생성 (파일 이름 없음, 주로 `/tmp`에서 사용) |
| `O_RSYNC` | 읽기 시 동기화 모드 적용 (읽기 데이터의 안정성 보장) |
| `O_DSYNC` | 쓰기 완료 시 디스크 동기화 (데이터만) |
| `O_SYNC` | 쓰기 완료 시 디스크 동기화 (데이터 + 메타데이터) |
| `O_PATH` | 파일의 경로만 열고 읽기/쓰기 권한 없이 파일 디스크립터 반환 |



## 파일 오프셋 지정
```c
lseek(fd, 5, SEEK_SET); 시작 기준 5번 위치
lseek(fd, 0, SEEK_CUR); 현재 위치
lseek(fd, 0, SEEK_END); 끝기준
```

## unlink()
```c
#include <unistd.h>
int unlink(const char *pathname);
```
설명: 파일 시스템에서 파일 이름(entry)를 삭제하는 시스템 호출

## remove()
```c
#include <stdio.h>
int remove(const char *filename);
```
설명: 파일 또는 빈 디렉터리를 삭제하는 표준 C 라이브러리 함수

<br><br><br><br>
