# 智能蛇

## 小说明：我的电脑是64位系统也打开了虚拟化设置，但是VBOX无法新建64位的Linux系统（不知道咋回事），而我们只有64位的ISO文件，所以只能在Windows环境下写一个了

## C code代码如下
```
#include<stdio.h> 
#include <stdlib.h>
#include<time.h> 
#include<windows.h> 
#define SANKE_MAX_LENGTH 20
#define SNAKE_HEAD 'H'
#define SNAKE_BODY 'X'
#define BLANK_CELL ' '
#define SNAKE_FOOD '$'
#define WALL_CELL '*'
 
char map[12][12]={
    {"***********"},
    {"*XXXXH    *"},
    {"*    **   *"},
    {"*         *"},
    {"*   *     *"},
    {"*         *"},
    {"*         *"},
    {"*         *"},
    {"*      *  *"},
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

int juedui(int a,int b){
	int c=a-b;
	if(c<0){
		return -c;
	}
	else{
		return c;
	}
}

char moveable(int snakeX[],int snakeY[],int foodX,int foodY,char map[][12],int x) {
	int distance[4]={0};
	char move[5]="adws";
	distance[0]=juedui(snakeX[x-1]-1,foodX)+juedui(snakeY[x-1],foodY);
	distance[1]=juedui(snakeX[x-1]+1,foodX)+juedui(snakeY[x-1],foodY);
	distance[2]=juedui(snakeX[x-1],foodX)+juedui(snakeY[x-1]-1,foodY);
	distance[3]=juedui(snakeX[x-1],foodX)+juedui(snakeY[x-1]+1,foodY);

	if(map[snakeY[x-1]+1][snakeX[x-1]]=='X'||map[snakeY[x-1]+1][snakeX[x-1]]=='*'||snakeY[x-1] ==snakeY[x-2]-1){
		distance[3]=999;		
	}
	if(map[snakeY[x-1]][snakeX[x-1]-1]=='X'||map[snakeY[x-1]][snakeX[x-1]-1]=='*'||snakeX[x-1] ==snakeX[x-2]+1){
		distance[0]=999;		
	}
	if(map[snakeY[x-1]][snakeX[x-1]+1]=='X'||map[snakeY[x-1]][snakeX[x-1]+1]=='*'||snakeX[x-1] ==snakeX[x-2]-1){
		distance[1]=999;		
	}
	if(map[snakeY[x-1]-1][snakeX[x-1]]=='X'||map[snakeY[x-1]-1][snakeX[x-1]]=='*'||snakeY[x-1] ==snakeY[x-2]+1){
		distance[2]=999;		
	}
	int y=9999;
	for(int i=0;i<=3;++i){
		y=y<distance[i]?y:distance[i];
	}
	for(int i=0;i<=3;++i){
		if(y==distance[i]){
			return move[i];
		}
	}
}


void food(int snakeX[],int snakeY[],int *px,int *py,char map[][12]){
	int same=0;
	while(same==0){
		int i=0;
		for( i=0;i<snakelength;++i){
			if(*px==snakeX[i]&&*py==snakeY[i]||map[*py][*px]=='*'){
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
		a=moveable(snakeX,snakeY,foodX,foodY,map,snakelength);
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
	srand ((unsigned) time (NULL));
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
		Sleep(1000);
	}
	system("cls");
	for(int j=0;j<=11;++j){
    	printf("%s\n",map2[j]);
		}
	system("pause");
}
```
