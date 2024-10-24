+++
author = "aobara"
title = "lrc2srt"
date = "2024-10-23"

categories = [
    "笔记",
    "编程",
]

image = "1.png"
+++
## 前置知识
sscanf:  
> int sscanf(const char *str, const char *format, ...)


返回值
如果成功，该函数返回成功匹配和赋值的个数。如果到达文件末尾或发生读错误，则返回 EOF。  
格式字符串 [%9[^]]]%[^\n]  
[%9[^]]：  
[ 和 ] 是格式说明符的一部分，表示一个字符集。  
9 表示最多读取 9 个字符。  
^ 表示否定字符集，即读取所有不是 ] 的字符。  
因此，[%9[^]] 表示读取最多 9 个字符，直到遇到 ]，并将结果存储到 timestamp 中。  
%[^\n]：  
^ 表示否定字符集，即读取所有不是换行符 \n 的字符。  
因此，%[^\n] 表示读取直到遇到换行符的所有字符，并将结果存储到 text 中。  
## 分析格式
lrc:  
```actionscript
[mm:ss.xx]test 
``` 
srt:  
```actionscript
1
hh:mm:ss,xxx --> hh:mm:ss,xxx
test

```
## 最终代码
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <wchar.h>
#include <locale.h>
#include <stdbool.h>
#include <string.h>

#define INPUT_FILE "input.lrc"
#define OUTPUT_FILE "output.srt"

char* formatTimeStamp(int min,int sec,int millisec){
     static char srt_time[20];
    int hours = min / 60;
    min %= 60;

    sprintf(srt_time, "%02d:%02d:%02d,%03d", hours, min, sec, millisec*10);
    return srt_time;
}

int extractTimeStamp(const char * buffer,char *timeStamp,char *OriText){
    if(sscanf(buffer,"[%9[^]]]%[^/n]",timeStamp,OriText)==2){
        return 1;
    }else
        if(sscanf(buffer,"[%9[^]]]%[^/n]",timeStamp,OriText)==1){
            return 1;
        }else{
            return 0;
        }
}

bool BreakUpTimeStamp(const char * timeStamp,int * mins,int* sec,int* millisec){
    if(sscanf(timeStamp,"%2d:%2d.%2d",mins,sec,millisec)==3){
        return 1;
    }else return 0;
}

int main(){

    FILE* inputFile=fopen(INPUT_FILE,"r");
    FILE* outputFile=fopen(OUTPUT_FILE,"w");

    char buffer[256];
    int index =1;
    int prev_mins=0,prev_sec=0,prev_millisec=0;
    int mins=0,sec=0,millisec=0;
    char timeStamp[10],OriText[200],prev_text[200];
    char startTime[20];
    char endTime[20];
    bool firstFlag=1;

    if(inputFile==NULL) {
        printf("can not open input file\n");
        return 1;
    }
    if(outputFile==NULL){
        printf("can not open output file\n");
        return 1;
    }

    setlocale(LC_ALL,"en_US.UTF-8");

    while(fgets(buffer,sizeof(buffer),inputFile)){
        if(extractTimeStamp(buffer,timeStamp,OriText)){
            if(BreakUpTimeStamp(timeStamp,&mins,&sec,&millisec)){
                if(firstFlag){
                    prev_mins=mins;
                    prev_sec=sec;
                    prev_millisec=millisec;
                    strcpy(prev_text,OriText);
                    firstFlag=0;
                    printf("%d,%d,%d\n",prev_mins,prev_sec,prev_millisec );
                }else{
                    fprintf(outputFile, "%d\n",index);

                    strcpy(startTime, formatTimeStamp(prev_mins, prev_sec, prev_millisec));
                    strcpy(endTime, formatTimeStamp(mins, sec, millisec)); 

                    printf("prev:%d,%d,%d\n",prev_mins,prev_sec,prev_millisec );
                    printf("curr:%d,%d,%d\n",mins,sec,millisec);
                    printf("\n");

                    fprintf(outputFile, "%s --> %s\n",startTime,endTime);
                    fprintf(outputFile, "%s\n",prev_text);
                    

                    prev_mins = mins;
                    prev_sec = sec;
                    prev_millisec = millisec;
                    strcpy(prev_text,OriText);
                    index++;

                }

            }else{
                printf("cannot BreakDown TimeStamp\n");
            }
        }else{
            printf("cannot extract TimeStamp\n");
        }
    }
    fclose(inputFile);
    fclose(outputFile);

    return 0;
}
```