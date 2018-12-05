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




## 注意事项：
（1）map[][]的前一个格是Y坐标，后一个格式X坐标。X坐标的边界是10，Y坐标的边界是11。

（2）对于snake_eat，要注意food的坐标不能是蛇的身体的坐标，吃到长度为20时GAMEOVER。

（3）要使用清屏函数。要在函数中改变整数数值要用指针变量。




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
		if(snakeY[4]==snakeY[i]&&snakeX[4]==snakeX[i]||snakeX[4]==0||snakeX[4]==10||snakeY[4]==0||snakeY[4]==11){
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

## C code snake_eat
```
#include<stdio.h> 
#include <stdlib.h>
#include<time.h> 
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
int foodX=rand()%9+1;
int foodY=rand()%10+1;
int *px=&foodX;
int *py=&foodY;
int *len=&snakelength;
int eat=0;

void food(int snakeX[],int snakeY[],int *px,int *py,char map[][12]){
	int same=0;
	while(same==0){
		int i=0;
		for( i=0;i<snakelength;++i){
			if(*px==snakeX[i]&&*py==snakeY[i]){
				*px=rand()%9+1;
				*py=rand()%10+1;
				break;
			}
		}	
		if(i==snakelength){
			++same;
		}
}
map[*py][*px]='$';
}

int snake_eat(char map[][12],int snakeX[],int snakeY[],int foodX,int foodY,int *len){
		char a;
		scanf("%c",&a);
		switch(a){
			case 'a':
			case 'A':
				if(snakeX[*len-1] !=snakeX[*len-2]+1){
					if(snakeX[*len-1]-1==foodX&&snakeY[*len-1]==foodY){
						++*len;
						snakeX[*len-1]=snakeX[*len-2]-1;
						snakeY[*len-1]=snakeY[*len-2];
						map[snakeY[*len-1]][snakeX[*len-1]]='H';
						map[snakeY[*len-2]][snakeX[*len-2]]='X';
						return 1;
						
					}
					else{
									
					map[snakeY[0]][snakeX[0]]=' ';
					for(int i=0;i<*len-1;++i){
						snakeX[i]=snakeX[i+1];
						snakeY[i]=snakeY[i+1];
					}
					--snakeX[*len-1];					
					map[snakeY[*len-1]][snakeX[*len-1]]='H';
					map[snakeY[*len-2]][snakeX[*len-2]]='X';			
				}
			}
				return 0;
			case 'd':
			case 'D':
				if(snakeX[*len-1] !=snakeX[*len-2]-1){
					if(snakeX[*len-1]+1==foodX&&snakeY[*len-1]==foodY){
						++*len;
						snakeX[*len-1]=snakeX[*len-2]+1;
						snakeY[*len-1]=snakeY[*len-2];
						map[snakeY[*len-1]][snakeX[*len-1]]='H';
						map[snakeY[*len-2]][snakeX[*len-2]]='X';
						return 1;
					}
					else{
					map[snakeY[0]][snakeX[0]]=' ';
					for(int i=0;i<*len-1;++i){
						snakeX[i]=snakeX[i+1];
						snakeY[i]=snakeY[i+1];
					}
					++snakeX[*len-1];					
					map[snakeY[*len-1]][snakeX[*len-1]]='H';
					map[snakeY[*len-2]][snakeX[*len-2]]='X';
				}
			}
				return 0;
			case 'w':
			case 'W':
				if(snakeY[*len-1] !=snakeY[*len-2]+1){
					if(snakeX[*len-1]==foodX&&snakeY[*len-1]-1==foodY){
						++*len;
						snakeX[*len-1]=snakeX[*len-2];
						snakeY[*len-1]=snakeY[*len-2]-1;
						map[snakeY[*len-1]][snakeX[*len-1]]='H';
						map[snakeY[*len-2]][snakeX[*len-2]]='X';
						return 1;
					}
					else{				
					map[snakeY[0]][snakeX[0]]=' ';
					for(int i=0;i<*len-1;++i){
						snakeX[i]=snakeX[i+1];
						snakeY[i]=snakeY[i+1];
					}
					--snakeY[*len-1];					
					map[snakeY[*len-1]][snakeX[*len-1]]='H';
					map[snakeY[*len-2]][snakeX[*len-2]]='X';			
				}
			}
				return 0;
			case 's':
			case 'S':
				if(snakeY[*len-1] !=snakeY[*len-2]-1){
					if(snakeX[*len-1]==foodX&&snakeY[*len-1]+1==foodY){
						++*len;
						snakeX[*len-1]=snakeX[*len-2];
						snakeY[*len-1]=snakeY[*len-2]+1;
						map[snakeY[*len-1]][snakeX[*len-1]]='H';
						map[snakeY[*len-2]][snakeX[*len-2]]='X';
						return 1;
					}
					else{				
					map[snakeY[0]][snakeX[0]]=' ';
					for(int i=0;i<*len-1;++i){
						snakeX[i]=snakeX[i+1];
						snakeY[i]=snakeY[i+1];
					}
					++snakeY[*len-1];					
					map[snakeY[*len-1]][snakeX[*len-1]]='H';
					map[snakeY[*len-2]][snakeX[*len-2]]='X';			
				}
					
			}
			return 0;
		}
	}
		
int gameover(int snakeX[],int snakeY[],int snakelength){
	for(int i=0;i<snakelength-1;++i){
		if(snakeY[snakelength-1]==snakeY[i]&&snakeX[snakelength-1]==snakeX[i]||snakeX[snakelength-1]==0||snakeX[snakelength-1]==10||snakeY[snakelength-1]==0||snakeY[snakelength-1]==11){
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
	food(snakeX,snakeY,px,py,map);
	print(map);
	while(gameo==0&&snakelength<SANKE_MAX_LENGTH){
		if(eat==1){
			food(snakeX,snakeY,px,py,map);
			eat=0;
		}
		eat=snake_eat(map,snakeX,snakeY,foodX,foodY,len);
		system("cls");
		print(map);
		gameo=gameover(snakeX,snakeY,snakelength);
	}
	system("cls");
	for(int j=0;j<=11;++j){
    	printf("%s\n",map2[j]);
		}
}
```