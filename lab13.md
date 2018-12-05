# 贪吃蛇游戏。
伪代码与头部：
```
输出字符矩阵
	WHILE not 游戏结束 DO
		ch＝等待输入
		CASE ch DO
		‘A’:左前进一步，break 
		‘D’:右前进一步，break    
		‘W’:上前进一步，break    
		‘S’:下前进一步，break    
		END CASE
		输出字符矩阵
	END WHILE
	输出 Game Over!!! 
```
头部：
![](https://sysu-swi.github.io/images/snake-head.png)



## C code：snake_move.c(用dev c++，在windows上运行)

```
#include<stdio.h> 
#include <stdlib.h>
#define SANKE_MAX_LENGTH 20
#define SNAKE_HEAD 'H'
#define SNAKE_BODY 'X'
#define BLANK_CELL ' '
#define SNAKE_FOOD '$'
#define WALL_CELL '*'
char map[12][12]={
    {"***********"},
    {"*XXXXH    *"},
    {"*         *"},
    {"*         *"},
    {"*         *"},
    {"*         *"},
    {"*         *"},
    {"*         *"},
    {"*         *"},
    {"*         *"},
    {"*         *"},
    {"***********"}
};

char map2[12][12]={
        {"***********"},
        {"*         *"},
        {"*         *"},
        {"*         *"},
        {"* G a m e *"},
        {"*         *"},
        {"* O v e r *"},
        {"*         *"},
        {"*         *"},
        {"*         *"},
        {"*         *"},
        {"***********"}
    	};
    	
    	
int snakeX[SANKE_MAX_LENGTH] ={1,2,3,4,5};
int snakeY[SANKE_MAX_LENGTH] ={1,1,1,1,1};
int snakelength=5;

void snake_move(char map[][12],int snakeX[],int snakeY[]){
		char a;
		scanf("%c",&a);
		switch(a){
			case 'a':
			case 'A':
				if(snakeX[4] !=snakeX[3]+1){				
					map[snakeY[0]][snakeX[0]]=' ';
					for(int i=0;i<4;++i){
						snakeX[i]=snakeX[i+1];
						snakeY[i]=snakeY[i+1];
					}
					--snakeX[4];					
					map[snakeY[4]][snakeX[4]]='H';
					map[snakeY[3]][snakeX[3]]='X';			
				}
				break;
			case 'd':
			case 'D':
				if(snakeX[4] !=snakeX[3]-1){
					map[snakeY[0]][snakeX[0]]=' ';
					for(int i=0;i<4;++i){
						snakeX[i]=snakeX[i+1];
						snakeY[i]=snakeY[i+1];
					}
					++snakeX[4];					
					map[snakeY[4]][snakeX[4]]='H';
					map[snakeY[3]][snakeX[3]]='X';
				}
				break;
			case 'w':
			case 'W':
				if(snakeY[4] !=snakeY[3]+1){				
					map[snakeY[0]][snakeX[0]]=' ';
					for(int i=0;i<4;++i){
						snakeX[i]=snakeX[i+1];
						snakeY[i]=snakeY[i+1];
					}
					--snakeY[4];					
					map[snakeY[4]][snakeX[4]]='H';
					map[snakeY[3]][snakeX[3]]='X';			
				}
				break;
			case 's':
			case 'S':
				if(snakeY[4] !=snakeY[3]-1){				
					map[snakeY[0]][snakeX[0]]=' ';
					for(int i=0;i<4;++i){
						snakeX[i]=snakeX[i+1];
						snakeY[i]=snakeY[i+1];
					}
					++snakeY[4];					
					map[snakeY[4]][snakeX[4]]='H';
					map[snakeY[3]][snakeX[3]]='X';			
				}
				break;	
			}
		}
		
int gameover(int snakeX[],int snakeY[]){
	for(int i=0;i<4;++i){
		if(snakeY[4]==snakeY[i]&&snakeX[4]==snakeX[i]||snakeX[4]==0||snakeX[4]==10||snakeY[4]==0||snakeY[4]==10){
		return 1;
		}
	}
return 0;	
}					
				
void print(char map[][12]){
	for(int i=0;i<=11;++i){
		printf("%s\n",map[i]);
	}
}		
			
		
int main(){
	int gameo=0;
	print(map);
	while(gameo==0){
		snake_move(map,snakeX,snakeY);
		system("cls");
		print(map);
		gameo=gameover(snakeX,snakeY);
	}
	system("cls");
	for(int j=0;j<=11;++j){
    	printf("%s\n",map2[j]);
		}
}
```
