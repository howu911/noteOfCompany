1. 第五版：加减乘除重构版

   ```c
   
   #include <stdio.h>
   #include <string.h>
   #include <assert.h>
   
   
   typedef struct
   {
   	int value;
   	int num[10];
   	char op[10];
   	int opCount;
   	int numCount;
   	char* inputChar;
   }C_DATA;
   
   int StrToNum(char str[]) {
   	int num = 0;
   	for (int i = 0; str[i] >= '0' && str[i] <= '9'; i++) {
   		num = num * 10 + (str[i] - '0');
   	}
   	return num;
   }
   
   void splitString(C_DATA* pData){
   	int expr_len = strlen(pData->inputChar);
   	int result = StrToNum(pData->inputChar);
   	pData->num[pData->numCount++] = result;
   
   	for (int i = 0; i < expr_len; i++){
   		switch (pData->inputChar[i]){
   			case '+':
   			{
   				result = StrToNum(pData->inputChar + i + 1);
   				pData->num[pData->numCount++] = result;
   				pData->op[pData->opCount++] = '+';
   				break;
   			}
   			case '-':
   			{
   				result = StrToNum(pData->inputChar + i + 1);
   				pData->num[pData->numCount++] = result;
   				pData->op[pData->opCount++] = '-';
   				break;
   			}
   			case '*':
   			{
   				result = StrToNum(pData->inputChar + i + 1);
   				pData->num[pData->numCount-1] *= result;
   				break;
   			}
   			case '/':
   			{
   				result = StrToNum(pData->inputChar + i + 1);
   				pData->num[pData->numCount - 1] /= result;
   				break;
   			}
   			default:
   				break;
   		}
   	}
   }
   
   
   void addSubMul(C_DATA* pData) {
   	int result = 0;
   	int numPos = 1;
   	result += pData->num[0];
   	for (int i = 0; i < pData->opCount; i++) {
   		if (pData->op[i] == '+') {
   			result += pData->num[numPos++];
   		}
   		else if (pData->op[i] == '-') {
   			result -= pData->num[numPos++];
   		}
   	}
   
   	pData->value = result;
   }
   
   
   int calc(char* expr)
   {
   	C_DATA data;
   
   	data.inputChar = expr;
   	data.value = 0;
   	data.numCount = 0;
   	data.opCount = 0;
   
   	splitString(&data);
   	addSubMul(&data);
   
   	return data.value;
   }
   int main(void)
   {
   	assert(calc("101") == 101);
   
   	assert(calc("101+2-20+2") == 85);
   	assert(calc("10-3+2") == 9);
   
   	assert(calc("1*3+2") == 5);
   	assert(calc("10-3*2-1") == 3);
   	assert(calc("10*3*2-1") == 59);
   
   	assert(calc("3/1+2") == 5);
   	assert(calc("10-3/1+1") == 8);
   	assert(calc("1+80/4/2*2-1") == 20);
   	printf("sucess\n");
   	return 0;
   }
   
   
   ```

   

https://www.cnblogs.com/lanhaicode/p/10776166.html



```c
#if 0
#include <stdio.h>
#include <string.h>
#include <assert.h>


typedef struct
{
	int value;
	int num[10];
	char op[10];
	int opCount;
	int numCount;
	char* inputChar;
}C_DATA;

int StrToNum(char str[]) {
	int num = 0;
	for (int i = 0; str[i] >= '0' && str[i] <= '9'; i++) {
		num = num * 10 + (str[i] - '0');
	}
	return num;
}

void splitString(C_DATA* pData){
	int expr_len = strlen(pData->inputChar);
	int result = 0;

	for (int i = 0; i < expr_len; i++){
		switch (pData->inputChar[i]){
			case '(':
			{
				pData->op[pData->opCount++] = '(';
			}
			case ')':
			{
				while (pData->op[pData->opCount-1] != '(')
					pData->num[pData->numCount++] = pData->op[--pData->opCount];
				--pData->opCount;
			}
			case '+':
			{
				
				break;
			}
			case '-':
			{
				result = StrToNum(pData->inputChar + i + 1);
				pData->num[pData->numCount++] = result;
				pData->op[pData->opCount++] = '-';
				break;
			}
			case '*':
			{
				result = StrToNum(pData->inputChar + i + 1);
				pData->num[pData->numCount-1] *= result;
				break;
			}
			case '/':
			{
				result = StrToNum(pData->inputChar + i + 1);
				pData->num[pData->numCount - 1] /= result;
				break;
			}
			default:
				break;
		}
	}
}


int addSubMul(C_DATA* pData) {
	splitString(pData);
	int result = 0;
	int numPos = 1;
	result += pData->num[0];
	for (int i = 0; i < pData->opCount; i++) {
		if (pData->op[i] == '+') {
			result += pData->num[numPos++];
		}
		else if (pData->op[i] == '-') {
			result -= pData->num[numPos++];
		}
		else if (pData->op[i] == '('){
			char subStr[20];
			i++;
			for (int j = 0; pData->op[i] != ')';i++){
				subStr[j++] = pData->op[i];
			}
			C_DATA data;

			data.inputChar = subStr;
			data.value = 0;
			data.numCount = 0;
			data.opCount = 0;

			result += addSubMul(&data);
		}
	}

	pData->value = result;
	return 0;
}


static int calc(char* expr)
{
	C_DATA data;

	data.inputChar = expr;
	data.value = 0;
	data.numCount = 0;
	data.opCount = 0;

	addSubMul(&data);

	return data.value;
}
int main(void)
{
	assert(calc("101") == 101);

	assert(calc("101+2-20+2") == 85);
	assert(calc("10-3+2") == 9);

	assert(calc("1*3+2") == 5);
	assert(calc("10-3*2-1") == 3);
	assert(calc("10*3*2-1") == 59);

	assert(calc("3/1+2") == 5);
	assert(calc("10-3/1+1") == 8);
	assert(calc("1+80/4/2*2-1") == 20);

	assert(calc("3/(1+2)") == 1);
	printf("sucess\n");
	return 0;
}
#endif

#include <stdio.h>
#include <string.h>
#include <assert.h>
#include <stdlib.h>


typedef struct
{
	int value;
	int num[10];
	char op[10];
	int opCount;
	int numCount;
	char *inputChar;
}C_DATA;

int StrToNum(char str[]) {
	int num = 0;
	for (int i = 0; str[i] >= '0' && str[i] <= '9'; i++) {
		num = num * 10 + (str[i] - '0');
	}
	return num;
}

char *NumToStr(int num) {
	char conver_str[10];
	_itoa_s(num, conver_str, 10, 10);
	return conver_str;
}

void splitString(C_DATA* pData){
	int expr_len = strlen(pData->inputChar);
	int result = StrToNum(pData->inputChar);
	pData->num[pData->numCount++] = result;

	for (int i = 0; i < expr_len; i++){
		switch (pData->inputChar[i]){
		case '+':
		{
			result = StrToNum(pData->inputChar + i + 1);
			pData->num[pData->numCount++] = result;
			pData->op[pData->opCount++] = '+';
			break;
		}
		case '-':
		{
			result = StrToNum(pData->inputChar + i + 1);
			pData->num[pData->numCount++] = result;
			pData->op[pData->opCount++] = '-';
			break;
		}
		case '*':
		{
			result = StrToNum(pData->inputChar + i + 1);
			pData->num[pData->numCount - 1] *= result;
			break;
		}
		case '/':
		{
			result = StrToNum(pData->inputChar + i + 1);
			pData->num[pData->numCount - 1] /= result;
			break;
		}
		default:
			break;
		}
	}
}


void addSubMul(C_DATA* pData) {
	int result = 0;
	int numPos = 1;
	result += pData->num[0];
	for (int i = 0; i < pData->opCount; i++) {
		if (pData->op[i] == '+') {
			result += pData->num[numPos++];
		}
		else if (pData->op[i] == '-') {
			result -= pData->num[numPos++];
		}
	}

	pData->value = result;
}


int calc_true(char *expr)
{
	C_DATA data;

	data.inputChar = expr;
	data.value = 0;
	data.numCount = 0;
	data.opCount = 0;

	splitString(&data);
	addSubMul(&data);

	return data.value;
}

void digui(char* expr, char *str, int *pos){
	char str_temp[20];
	int expr_len = strlen(expr);
	int i = 0;
	int conver_pos = *pos;
	for (i = 0; i < expr_len; i++){
		if (expr[i] == '('){
			i++;
			digui(expr + i, str_temp, &i);
		}
		if (expr[i] == ')'){
			char *s = (char)malloc(sizeof(char)*20);
			s = NumToStr(calc_true(str_temp));
			for (int j = 0; j < strlen(s);){
				str[conver_pos-1] = s[j++];
				conver_pos++;
			}
			(*pos)++;
			return;
		}
		(*pos)++;
		if (*pos > expr_len)
			break;
		str_temp[i] = expr[i];
	}
	int j = 0;
	for (j = 0; j < i; j++){
		str[j] = str_temp[j];
	}
	str[j] = '\0';
}

int calc(char *expr){
	char s[20];
	int pos = 0;
	digui(expr, s, &pos);

	return calc_true(s);
}

int main(void)
{
	assert(calc("101") == 101);

	assert(calc("101+2-20+2") == 85);
	assert(calc("10-3+2") == 9);

	assert(calc("1*3+2") == 5);
	assert(calc("10-3*2-1") == 3);
	assert(calc("10*3*2-1") == 59);

	assert(calc("3/1+2") == 5);
	assert(calc("10-3/1+1") == 8);
	assert(calc("1+80/4/2*2-1") == 20);

	assert(calc("1*(2+3)+8") == 13);
	printf("sucess\n");
	return 0;
}



```



array.c

```c
#include "StackArray.h"
//是否为空栈
int IsEmpty(Stack S)
{
	return S->TopOfStack == EmptyTOS;
}
//是否为满栈
int IsFull(Stack S)
{
	return S->TopOfStack >= S->Capacity - 1;
}
//使栈为空
void MakeEmpty(Stack S)
{
	S->TopOfStack = EmptyTOS;
}
//创建一个栈
Stack CreatStack(int MaxElement)
{
	if (MaxElement < MinStackSize)
		perror("MaxElement littler to MinStackSize");

	Stack S = (Stack)malloc(sizeof(struct StackArray));
	if (S == NULL)
		perror("Stack S malloc error");

	S->Capacity = MaxElement;
	S->IntArray = (int *)malloc(sizeof(int) * MaxElement);
	if (S->IntArray == NULL)
		perror("S->IntArray malloc error");

	MakeEmpty(S);

	return S;
}

//销毁一个栈
void DisposeQueue(Stack S)
{
	if (S != NULL)
	{
		free(S->IntArray);
		free(S);
		S = NULL;
	}
}
//Push
void Push(int X, Stack S)
{
	if (IsFull(S))
		perror("Stack is full");
	else
		S->IntArray[++S->TopOfStack] = X;
	
}
//Pop
void Pop(Stack S)
{
	if (IsEmpty(S))
		perror("Stack is empty");
	else
		S->TopOfStack--;
}
//Top
int Top(Stack S)
{
	if (IsEmpty(S))
		perror("Stack is empty");
	else
		return S->IntArray[S->TopOfStack];
	return 0;
}
//Pop and Pop
int TopAndPop(Stack S)
{
	if (IsEmpty(S))
		perror("Stack is empty");
	else
		return S->IntArray[S->TopOfStack--];
	return 0;
}

```



array.h

```c
#include <stdio.h>
#include <stdlib.h>

#define MinStackSize (5)
#define EmptyTOS (-1)

struct StackArray;
typedef struct StackArray *Stack;

//是否为空栈
int IsEmpty(Stack S);
//是否为满栈
int IsFull(Stack S);
//使栈为空
void MakeEmpty(Stack S);
//创建一个栈
Stack CreatStack(int MaxElement);
//销毁一个栈
void DisposeQueue(Stack S);
//Push
void Push(int X, Stack S);
//Pop
void Pop(Stack S);
//Top
int Top(Stack S);
//Pop and Pop
int TopAndPop(Stack S);




struct StackArray
{
	int Capacity;
	int TopOfStack;
	int *IntArray;
};
```

main.c

```c
#include "StackArray.h"

int main(void)
{
	Stack S = CreatStack(4);

	//入栈
	Push(1, S);
	Push(2, S);
	Push(4, S);
	Push(3, S);
	//输出
	while (!(IsEmpty(S)))
	{
		printf("%d ", Top(S));
		Pop(S);
	}


	while (1);
	return 0;
}
```

