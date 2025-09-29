
# 고수준 입출력 (High-level I/O)

C 표준 라이브러리를 사용 (fopen, fread, fwrite, fprintf, fclose)

- stdio.h 필요
- 버퍼링 제공 → 효율적인 파일 입출력 가능

``` c
#include <stdio.h>
int main() {
    FILE *fp;
    char buffer[100];

    // 파일 열기 (읽기 전용)
    fp = fopen("high_level.txt", "r");
    if (fp == NULL) {
        perror("파일 열기 실패");
        return 1;
    }
    // 읽기
    if (fgets(buffer, sizeof(buffer), fp) != NULL) {
        printf("%s", buffer);
    }
    // 파일 닫기
    fclose(fp);

    // 파일 쓰기 예시
    fp = fopen("output.txt", "w");
    if (fp == NULL) {
        perror("파일 열기 실패");
        return 1;
    }
    fprintf(fp, "Hello, High-level I/O!\n");
    fclose(fp);

    return 0;
}
```
### 특징
- FILE * 포인터 사용
- 버퍼링 처리 → 효율적
- 문자열 단위 입출력 편리 (fgets, fprintf)
- 이식성이 좋고, 코드가 직관적


```C
FILE *fp;
fp = fopen("unix.txt", "r");
r+ : 파일이 없는 경우 에러
w+ : 파일이 없는 경우 생성
```