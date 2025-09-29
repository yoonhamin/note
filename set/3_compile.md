# GCC 옵션 정리

## -o
- 출력(output) 화일명을 지정하는 옵션
- 사용 예시:
```bash
gcc -o hello hello.c
```
- -l 필요 라이브러리를 지정하는 옵션
- -g 컴파일된 오브젝트 파일에 디버깅 코드를 추가
추후 gdb를 사용할 때 필요


사용 예시:
```bash 
gcc -g -o hello hello.c
```

## -O
- 코드를 최적화(optimization)
- -O2는 가장 많이 최적화
- -O0는 최적화를 하지 않음
- 기본값: -O1 (시스템마다 상이함)

## -I
- #include 문장에서 지정한 헤더 파일이 들어있는 디렉토리를 지정
- gcc -c source.c -Iinclude
- 주의: -I는 붙여서 사용

# make
Target, dependency, command



```Makefile
CC = gcc
CFLAGS = -Wall -g
TARGET = program

OBJS = main.o func.o

$(TARGET): $(OBJS)
    $(CC) $(CFLAGS) -o $(TARGET) $(OBJS)

main.o: main.c func.h
    $(CC) $(CFLAGS) -c main.c

func.o: func.c func.h
    $(CC) $(CFLAGS) -c func.c

clean:
    rm -f $(OBJS) $(TARGET)
```



```Makefile
CC = gcc                # 컴파일러
CFLAGS = -Wall -g -Iinclude # 컴파일 옵션
TARGET = program        # 타겟 실행파일 이름
OBJS = main.o func.o    # 오브젝트 파일

$(TARGET): $(OBJS)      # 최종 실행파일 생성
	$(CC) $(CFLAGS) -o $(TARGET) $(OBJS)

main.o: main.c include/func.h   # main.o 생성
	$(CC) $(CFLAGS) -c main.c

func.o: func.c include/func.h   # func.o 생성
	$(CC) $(CFLAGS) -c func.c

clean:
	rm -f $(OBJS) $(TARGET)
```


```
project/
│
├─ Makefile
├─ main.c
├─ func.c
├─ include/
│   └─ func.h
└─ (컴파일 후 같은 위치에 추가)
    ├─ main.o
    ├─ func.o
    └─ program



stu
├── include/
│ └── student.h
├── src/
│ └── main.c
├── lib/
│ └── student.c
└── Makefile


```