---
title:  "[C#] 마라탕"
excerpt: "md 파일에 마크다운 문법으로 작성하여 Github 원격 저장소에 업로드 해보자. 에디터는 Visual Studio code 사용! 로컬 서버에서 확인도 해보자. "


# C# for( ;; ) while( true )의 차이

C++에서는 최적화 컴파일러를 끈 상황에서 for( ;; )와 while( true )를 Visual Studio의 디스어셈블리를 통해 보면 차이가 존재합니다.

for( ;; )의 경우 루프 끝에서 jmp로 for 루프 시작 지점으로 무조건 분기합니다.
while( true )의 경우 cmp로 true에 대해 조건을 굳이 한 번 더 확인한 후 루프 시작 지점으로 조건 분기합니다.

while( true ) 조건 분기를 사용함으로써 아주 약간의 성능상 손해가 발생하므로 C++에서는 for( ;; )를 매크로로 처리하여 사용하고 있습니다.

```cpp
#define LOOP for( ;; )

/* 
example

int i = 0;
LOOP
{
	i += 1;
}

*/
```

C#에서…

```csharp
static void DoFor()
{
    for ( ;; )
    {
        Thread.Sleep( 1000 );
    }
}

static void DoWhile()
{
    while ( true )
    {
        Thread.Sleep( 1000 );
    }
}
```

위 예제 코드를 IL코드로 확인해보겠다.( VisualStudio 2022, ILSpy 사용 ) 

![Untitled](images/01/Untitled.png)

```csharp
// Summary: 정수 값 1을 int32로 계산 스택에 푸시합니다.
IL_0010: ldc.i4.1
// Summary: 계산 스택 맨 위에서 현재 값을 팝하여 인덱스 0에 있는 지역 변수 목록에 저장합니다.
IL_0011: stloc.0  
```

나머지는 전부 같은 코드지만 위의 두 줄이 추가됩니다.
1. stack 변수에 true값 1을 저장한 후에 무조건 분기합니다. (마지막 줄 br.s IL_0003)

2. 위에서 첫 번째 stack 변수를 사용해서 그런지 .locals init IL 명령어를 통해 bool 지역 변수 하나를 초기화합니다.

![Untitled](images/01/Untitled%201.png)

# 결론

while ( true )를 쓰더라도, bool 지역 변수 두 번의 값 대입 연산이 발생할 뿐 큰 차이는 발생하지 않는다.


categories:
  - csharp
tags:
  - [C#]

toc: true
toc_sticky: true
 
date: 2024-06-30
last_modified_at: 2024-06-30
---