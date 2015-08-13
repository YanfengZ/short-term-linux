#<center>实验4  系统程序设计</center>
>
		misc.o 依赖于 misc.c misc.h
	e. -> power
				->main.c
			->stack.o
				->stack.h 
				->misc.h
				->misc.c
	f. 首先执行和d一样的指令来生成main.o stack.o misc.o这三个文件,然而由于make会自动扫描依赖关系树并记录更新情况,会执行 gcc -O3 -o power main.o stack.o misc.o -lm 来生成power文件。
>
> 
> 
>
Answer:
>
	编写文件过程省略下为执行结果.
![](/Users/Chiba/short-term-linux/hw4/4-1.png)
![](/Users/Chiba/short-term-linux/hw4/4-2.png)
>###3.	本实验目的观察使用带-f选项的tail命令及学习如何使用gcc编译器，并观察进程运行。自己去查阅资料获取下面源程序中的函数（或系统调用）的作用。首先复制smallFile文件（实验1中创建的），文件名为dataFile；然后创建一个文件名为lab4-3.c的c语言文件，内容如下：
    {
        int i;
        i = 0;
        sleep(10);
        while (i < 5) {
            system("date");
            sleep(5);
            i++;
        }
        while (1) {
            system("date");
            sleep(10);
        }
    }
>在shell提示符下，依次运行下列三个命令：
>
>
>Answer:
>![](/Users/Chiba/short-term-linux/hw4/4-3.png)
>###4.	编程开发一个shell 程序
    2)	clr  ——清屏。
    3)	dir <directory>  ——列出目录<directory>的内容。
    4)	environ  ——列出所有的环境变量。
    5)	echo <comment>  ——在屏幕上显示<comment>并换行（多个空格和制表符可能被缩减为一个空格）。
    6)	help ——显示用户手册，并且使用more 命令过滤。
    7)	quit  ——退出shell。
    8)	shell 的环境变量应该包含shell=<pathname>/myshell，其中<pathname>/myshell 是可执行程序shell 的完整路径（不是你的目录下的硬连线路径，而是它执行的路径）。
    (二)	 其他的命令行输入被解释为程序调用，shell 创建并执行这个程序，并作为自己的子进程。程序的执行的环境变量包含一下条目：
    parent=<pathname>/myshell。
    (三)	 shell 必须能够从文件中提取命令行输入，例如shell 使用以下命令行被调用：
    myshell batchfile
    这个批处理文件应该包含一组命令集，当到达文件结尾时shell 退出。很明显，如果shell 被调用时没有使用参数，它会在屏幕上显示提示符请求用户输入。
    (四)	 shell 必须支持I/O 重定向，stdin 和stdout，或者其中之一，例如命令行为：
    programname arg1 arg2 < inputfile > outputfile
    使用arg1 和arg2 执行程序programname，输入文件流被替换为inputfile，输出文件流被替换为outputfile。
    stdout 重定向应该支持以下内部命令：dir、environ、echo、help。
    使用输出重定向时，如果重定向字符是>，则创建输出文件，如果存在则覆盖之；如果重定向字符为>>，也会创建输出文件，如果存在则添加到文件尾。
    (五)	 shell 必须支持后台程序执行。如果在命令行后添加&字符，在加载完程序后需要立刻返回命令行提示符。
    (六)	 命令行提示符必须包含当前路径。
    提示：
    1)	你可以假定所有命令行参数（包括重定向字符<、>、>>和后台执行字符&）和其他命令行参数用空白空间分开，空白空间可以为一个或多个空格或制表符（见上面（四） 中的命令行）。
    2)	程序的框架：
    #include <stdio.h>
    #include <unistd.h>
    #define MAX LINE 80 /* The maximum length command */
    int main(void){
    char *args[MAX LINE/2 + 1]; /* command line arguments */
    int should run = 1; /* flag to determine when to exit program */
    while (should run) {
    printf("myshell>");
    fflush(stdout);
    /**
    * After reading user input, the steps are:
    *内部命令：
    *…..
    *外部命令：
    * (1) fork a child process using fork()
    * (2) the child process will invoke execvp()
    * (3) if command included &, parent will invoke wait()
    *…..
    */
    }
    return 0;
    }
    项目要求：
    1)	设计一个简单的全新命令行shell，满足上面的要求并且在指定的Linux 平台上执行。不能使用system函数调用原shell程序运行外部命令。拒绝使用已有的shell程序的任何环境及功能。
    2)	写一个关于如何使用shell 的简单的用户手册，用户手册应该包含足够的细节以方便Linux初学者使用。例如：你应该解释I/O 重定向、程序环境和后台程序执行。用户手册必须命名为readme，必须是一个标准文本编辑器可以打开的简单文本文档。
    3)	源码必须有很详细的注释，并且有很好的组织结构以方便别人阅读和维护。结构和注释好的程序更加易于理解，并且可以保证批改你作业的人不用很费劲地去读你的代码。
    4)	在截止日期之前，要提供很详细的提交文档。
    5)	提交内容为源码文件，包括文件、makefile和readme文件。批改作业者会重新编译源码，如果编译不通过将没办法打分。
    6)	makefile文件必须能用make工具产生二进制文件myshell，即命令提示符下键入make 就会产生myshell 程序。
    7)	根据上面提供的实例，提交的目录至少应该包含以下文件：
    makefile  
    myshell.c  
    utility.c  
    myshell.h  
    readme
    提交：
    需要makefile 文件，所有提交的文件将会被复制到一个目录下，所以不要在makefile 中包含路径。makefile 中应该包含编译程序所需的依赖关系，如果包含了库文件，makefile 也会编译这个库文件的。
    为了清楚起见，再重复一次：不要提交二进制或者目标代码文件。所需的只是源码、makefile 和readme 文件。提交之前测试一下，把源码复制到一个空目录下，键入make 命令编译。
    所需的文档要求：
    首先，源码和用户手册都将被评估和打分，源码需要注释，用户手册可以是你自己选择的形式（但要能被简单文本编辑器打开）。其次，手册应该包含足够的细节以方便Linux 初学者使用，例如，你应该解释I/O 重定向、程序环境和后台程序执行。用户手册必须命名为readme。
>
Answer:
>![](/Users/Chiba/short-term-linux/hw4/4-4-1.png)
>![](/Users/Chiba/short-term-linux/hw4/4-4-2.png)
>		
		详情可见 myshell/readme.pdf 或者readme.md