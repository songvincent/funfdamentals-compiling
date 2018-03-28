#include<stdio.h>
#include<string>
#include<cstring>
#include<iostream>
#include<cstdlib>

using namespace std;

void error(string err);  //报错信息

void M(string word[]);  //主体函数

void S(string word[]);  //开始部分
void MultiLine(string word[]); //函数多行控制
void line(string word[]); //函数体的主体部分
void cal(string word[]);              //运算式
int Tell(string word[]);

int i=0;
int flag;
//string word[1000];
//出错时候的输出
void error(string err){
	flag=0;
	cout<<err<<endl;
	exit(0);
}

void S(string word[])
{
	if(word[i].compare("main"))
		error("lack keyword main");

	if(word[++i].compare("{")) error("lack left bracket");
	M(word);
	if(word[++i].compare("}")) error("lack left bracket");
}

//main函数主体部分
void M(string word[]){
    MultiLine(word);
    if(word[++i].compare("return")) error("error about keyword return");
    if(word[++i].compare(";")) error("lack symbol ;");
}
//函数主体语句
void MultiLine(string word[])
{
    while(word[++i].compare("return"))
    {
        i--;
        line(word);
    }
    i--;
    return ;
}


//每一行语句
void line(string word[])
{
    i++;
    //cout<<"most of them is here"<<endl;
    if(isIn(word[i],State)) //如果该词是int,double等关键字，声明语句
    {
        i++;
        if(!isIn(word[i],Var)) error("error about variable name");
        i++;
        if(word[i].compare("="))
        {
            i++;
            if(!isIn(word[i],Num) && !isIn(word[i],Char))
            error("error about Constant number/character");
        }
        else if(word[i].compare(";")) error("lack of symbol ;");
    }
    else if(isIn(word[i],Var))//赋值语句
    {
        i++;
        if(word[i].compare("=")) error("error of evaluation language");
        else{
            cal(word);
        }
        if(word[++i].compare(";")) error("lack of symbol ;");
    }
    else if(!word[i].compare("while"))  //while循环，但循环体只能有一行
    {
        Tell(word);
        line(word);

    }
    else if(!word[i].compare("if"))  //if判断，语句也只能有一句
    {
        Tell(word);//判断语句的分析
        line(word);//控制语句的分析
    }
    else error("unknown error!");
    return ;
}

//运算式
void cal(string word[]){
    if(!isIn(word[++i],Num) error("error of evaluation language");
    if(!isIn(word[++i],Calchar)) error("lack operation symbol");
    if(!isIn(word[++i],Num)) error("error of evaluation language");
}

//判断式
int Tell(string word[])
{
    if(word[++i].campare("(")) error("error of judgement");
    if(!isIn(word[++i],Var)) error("error of judgement ");
    if(!isIn(word[++i],Judgechar))  error("error of judgement");
    if(!isIn(word[++i],Var)) error("error of judgement ");
    if(word[++i].campare(")")) error("error of judgement");
}

int main()
{
    flag=1;
	string word[]={"main","{","M","return",";","}"};
	S(word);
	if(flag)
        cout<<"no error."<<endl;
	return 0;
}
