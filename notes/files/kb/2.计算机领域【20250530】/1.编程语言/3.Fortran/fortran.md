Fortran保姆级教学——考试所有知识点看这一篇就够了

RedGhost117

已于 2023-01-13 22:49:59 修改

阅读量4.5k
 收藏 183

点赞数 53
分类专栏： fortran 南信大 考试 文章标签： 经验分享
版权

南信大
同时被 3 个专栏收录
8 篇文章70 订阅
订阅专栏

考试
7 篇文章7 订阅
订阅专栏

fortran
1 篇文章4 订阅
订阅专栏
Fortran保姆级教学——考试所有知识点看这一篇就够了
临近期末本人复习的同时将整个fortran课堂知识整理了下来，希望学弟学妹们今后学这门课的时候不至于在csdn找不到系统的教程，也希望能帮到需要期末突击的同学。

都是考试重点，跳过了一些不常用的方法，用的最通俗易懂的语言和例子，如果有疏漏或者错误的希望得到大家指正

不要为了考试学编程，爱上它就会发现新天地

文章目录
Fortran保姆级教学——考试所有知识点看这一篇就够了
一、Fortran语言的历史和优点
历史
优点
二、Fortran语言基础
1.字符集
2.保留字
3.基本数据类型
1.整型常量
2.实型常量
3.复型常量
4.字符型常量
5.逻辑型常量
6.符号常量
4.变量的声明
1.整型变量声明
2.实型变量的声明
3.字符型
4.逻辑型变量声明
5. 全局变变量的声明
三. 表达式和语句
1.表达式
2.关系表达式
3.逻辑运算
四. 程序怎么写（三大结构和写法）
1.Fortran程序框架
2.顺序结构
3.选择结构
1.if系列
2.select case
4.循环结构
1.DO循环
2.DO-WHILE循环
3.退出循环语句
4.如何选择两种循环
5. forall循环
筛选操作
5.输入语句
6.输出语句
. 写程序中容易犯的错误
五. 数组
1.什么是数组
2.数组的定义
3. 数组引用
4.数组的储存结构
5.数组的输入输出
6.给数组赋初始值
7. 数组运算
8.数组的常用内置函数
9. where
10. 可变长数组
11. 改变当前数组的长度
六. 文件
1.读文件
2.要记住的两个读写方式
1.文本文件顺序读写
2.二进制文件顺序读取
3.二进制文件直接读取
3.文件的读写
7.子程序
1.什么时候需要子程序
2. 函数子程序function
3. 子例行程序subroutine
4. 传参的注意点
1. fortran是地址传参
2. 数组参数
八. 一个综合例子
一、Fortran语言的历史和优点
历史
Fortran全称：Formula Translator(公式翻译器)
1953年由巴库斯第一次提出
解决科学和公式计算问题的程序设计语言
1991年发布的Fortran90 是主要使用的Fortran版本
优点
适合计算的语言，语法简单
在数值计算，科学和工程技术领域，Fortran有强大的优势
Fortran编写的大型科学计算软件比C编写的通常快一个量级
总结一句话：快！大家现在学的程度处理的数据量还太小了，但一但是超大数据量计算时，就能体现出速度的差异了，一个量级可不是开玩笑的

二、Fortran语言基础
1.字符集
Fortran中规定可以使用的字符为：A-Z,z-a 数字0-9，以及一些特殊符号
Fortran中不区分大小写，实际编写中按自己习惯来，我习惯关键字要么都大写要么都小写，保证程序的可读性

2.保留字
语句保留字：用于描述语句语法成分的固定单词
常用的保留字有
PROGRAM INTEGER REAL READ PRINT WRITE DO END SUBROUTINE FUNCITION
1
2
稍后的使用中这些都是一定会使用的，不用特殊去记忆，但是需要注意避免使用保留字作为实体名称，这会降低程序的可读性

3.基本数据类型
这一部分有个大概了解就行，重点是各种数据类型变量的声明

这是一部分Fortran数据类型，如果看的有点懵逼不要紧，考试阶段我们只用掌握内部数据类型，简单又好学

1.整型常量
整型常量又称为整型常数或整数，包括正数，负数和0
如：1，2，3，4，0，-10都是整型
使用关键字integer来声明
2.实型常量
实型可以理解为实数——包括小数，指数
使用real关键字声明
小数应该大家都会的，重点说一下指数的表达
!声明一个实型变量
real a

!声明时候赋值(小数点后可以没有数字)
real ::a=9.
1
2
3
4
5
指数的表达:用E分隔，E左边是数字部分，右边是指数部分
real ::a= 3.14E5 ！表示3.14*乘十的五次方
注意：
E后面的指数只能是整型常量
E左右两边数字必须都有，E9是不合法的
E表示单精度实数，当单精度实数不足以表示一个数的大小或者精度时候，用双精度表示——将E换位D即可
3.复型常量
用一对括号括起来的两个实数表示，一个表示实部，一个表示虚部，**很简单，考试应该也不怎么考，看看就好**
	ex: (1.0,1.0)表示 1.0+1.0i
1
2
4.字符型常量
用单引号引起来的若干非空字符串，称为字符型，如’a’ ‘#dfhgsf’
使用关键字character来声明
如果在单引号内想使用引号，需要在需要的地方加两个‘
ex：'I '‘m a boy’ 表示的是——I’m a boy
字符串长度：从第一个字符算起，有多少个字符长度就是多少，注意：“空格” 空格算一个字符，有一个空格不表示空，同时’中间啥都没有’字符串长度为0
5.逻辑型常量
逻辑型（boolean）只有两个——true 和 false
逻辑型也可以表示整型——true赋值后可以表示任何整数，未赋值的时候true表示-1， false表示0 ，可以参与数学运算
6.符号常量
可以理解为定义宏变量
PARAMETER (pi=3.1415926) =>定义pi是3.1415926，之后使用pi就代表这个数字，方便，对于一些全局都要使用的如year，可以使用这样定义，增加程序可读性和解耦合
4.变量的声明
变量的声明是程序的重要重要部分，不会这部分就无法写程序！！！

变量是什么：指在程序运行期间其值可以变化的量,实质上是系统在内存中开辟了一个存储单元来存放变量的值
变量名的命名规范（考试必考）：必须！必须！以字母开头，只要看到不是字母开头的都是错的，记住这个原则就对了，非开头可以使用数字和下划线
ex: Sum, Average1, prcp, hello_world 都是合法的 | 1sum, ok% 这样的就是非法的
变量名的声明最好见名知意，方便程序阅读
1.整型变量声明
使用关键字integer
! 声明一个名叫a的变量
integer a
integer a,b 
! 同时声明了两个整型变量
1
2
3
4
可以选择整数在内存中储存的字节数
integer(1) a
integer(2) a ->短整型
integer(4) a ->长整型（默认就是4）
integer(8) a
!括号中数字越大表示储存的字节数越长，能储存的数字范围就越大
1
2
3
4
5
在声明期间想给a赋值要用” :: “
integer ::a=0
!切记不可不用::直接赋值——integer a=4是非法的
1
2
2.实型变量的声明
和整型类似，但是用real关键字声明
只有real(4) 和real(8) 分别是单精度实型和双精度实型
3.字符型
使用关键字character声明

需要指定字符型长度，否则默认是长度为1
character a ->长度为1的变量a
character (9) b,d ->同时声明了两个长度为9的字符串
character(3) ::b='hel' ->声明了b且赋值为'hel'
character*5 ::b='hello'  =>使用character*len 也可以设定相应长度的字符串
1
2
3
4
字符型常用函数
CHAR(num)-> 返回数字num对应的ASCII字符表的值
LEN(string)-> 返回字符串长度
LEN_TRIM(string)-> 返回去掉字符串尾端空格后的长度
INDEX(string,key)-> 第一个string是要处理的字符串对象，第二个是要查找的值，返回key在string中第一次出现的位置
TRIM(string)-> 返回string 去掉尾端空格的值
1
2
3
4
5
4.逻辑型变量声明
使用logical关键字声明

program main
	logical ::b=.TRUE. =>这种方式声明的true，默认参与运算时是-1
    logical ::a=5
    write(*,*)a+5,b+1
!    => 10 0
    pause
end

program main2
    logical a
    a=true
    a=3
    write(*,*)a
!    =>T 就算赋值之后a也是布尔类型，但是参与运算的时候当3
    end
!注意：赋初始值的时候a不能使用 logical ::a=true或者a=T,只能让a是一个数或者a=.TRUE.，只要非0就是true，是5也可以

1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
5. 全局变变量的声明
在一个函数或者一个子程序中声明的变量，作用域只有在该函数或者子程序中，如果想声明全局变量需要用common关键字声明
program main
    integer a,b,c
    common a,b,c
    external sub
    a=3.14
    b=4.76
    c=3.81
    
    call sub()
    pause
    end
    
subroutine sub()
    integer a,b,c
    common a,b,c
    write(*,*)a
    end

1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
由于全局变量是通过地址对应的方式实现共享数据的，因此如果声明了100个全局变量，想取到最后一个全局变量，则需要把之前99个变量都要写出来，才能拿到最后第100个变量
为了解决这个问题，可以将全局变量进行分组
使用common /groupname/ 就可以进行分组定义和分组取值
program main
    real pi, c1, c2
    common /group1/ pi, c1
    common /group2/ c2
    external sub
    const1 = 3.14 
    c2 = 4.47
    
    call sub()
    pause
    end
    
subroutine sub()
     real pi, c1, c2
     common /group2/ c2
     write(*,*) c2
end

1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
三. 表达式和语句
1.表达式
看这张图就ok


注意点：考试必考

算数运算顺序：先乘方再乘除最后加减；有括号括号优先

易考易错 不同类型之间的运算规则：

同类型之间运算结果还是同类型——最典型是整型之间除法，最后还是整型
ex：1/2 =0而不是0.5，0.5变成整型要把小数舍掉，还是0
不同类型之间的转换：会自动把低级类型转换为高级类型，同时转换从左到右进行，遇到不同类型才进行转换
ex: 1/2* 1.0=0
1./21=0.5
记住real类型比integer高级就行
2.关系表达式

关系表达式的格式： 表达式1 关系运算符 表达式2 ===>返回布尔值T/F

12>34 整数之间比较
(4+9)<10 带表达式的整数比较
(4.2,7.3)>(7.3,4.3) 复数之间比较
mod(4,2)==0 函数表达式参与
gpple‘ < 'baaaaaanana' 依次比较字符串的ASCII码值，只要有一个比较出来就返回，不比长度
这里g的ascii比b大，所以是F
1
2
3
4
5
6
3.逻辑运算

类似于其他与语言的
三个常用的掌握就行

&&=>.and.
|| =>.or.
! => .not.
四. 程序怎么写（三大结构和写法）
1.Fortran程序框架
程序需要用program+程序名——end括起来
示例代码：

program main
	!程序名字叫main，也可以叫任意的名字
	!程序体写在这之间
	
	integer a,b !定义变量
	a=2
	b=5
	
	!使用各种结构对数据进行处理
	do i=1,5
		a=a+1
		b=b+1
	enddo

	!最后输出
	write(*,*)a,b
end

1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
本质是将输入的数据处理后变成需要的数据输出

2.顺序结构
按照顺序一路走到黑


3.选择结构
1.if系列
重点

单分支
!单分支骨架

if(判断条件：是个布尔值T/F) then
	!代码块
endif
1
2
3
4
5
双分支
!双分支骨架
if(判断条件) then
	!代码块
else 
	!代码块
endif
1
2
3
4
5
6
多分支
if(判断条件) then
	!代码块
elseif then
	!代码块
else
	!代码块
endif	
1
2
3
4
5
6
7
使用示例

	program main
    real U,V,S
    read(*,*)U,V
    IF(U>0) THEN
        IF(V==0)THEN
            WRITE(*,*)"西风"
        else if(v<0) then
            write(*,*)'西北风'
        else
            write(*,*)'西南风' 
        ENDIF
    else if(U==0) then
        if(V==0) then
            write(*,*)'无风'
        else if(V>0) then
        write(*,*)'南风'
        else
            write(*,*)'北风'
        end if
    else 
        if(V==0)then
            write(*,*)'东风'
         else if(v<0) then
            write(*,*)'东北风'
         else
            write(*,*)'东南风' 
         ENDIf
    END IF
    
    S=sqrt(U**2+V**2)
    write(*,*)S
    if(S>=30.0)then
        write(*,*)'强湍流'
    else
        write(*,*)'弱湍流'
    end if
    pause
end

1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31
32
33
34
35
36
37
38
对于if中代码块只有一行的可以进行简写
program main
    integer ::a = 4
    if(2<a<8) write(*,*)a  =>不用写then，也不用写endif
      
    !等价于
    if(2<a<8) then
    	write(*,*)a
    endif
      
    pause
    end
1
2
3
4
5
6
7
8
9
10
11
注意事项：

if 后面一定要加 then，结束不是end，是endif
if可以嵌套多个使用
2.select case
类似于switch case

骨架
select case(运算表达式，得到一个数)
	case(控制表达式1)
		!代码块
	case(控制表达式2)
		!代码块
	.....可以有很多case
	endselect
1
2
3
4
5
6
7
8
示例代码

program main
	!a+b=7就走case(7),=8就走case(8)
    integer ::a=3,b=5
    select case(a+b)
    case(7)
        write(*,*)7
    case(8)
        write(*,*)8
    endselect
    pause
    end
1
2
3
4
5
6
7
8
9
10
11
4.循环结构
重中之重

1.DO循环
类似于其它语言的for循环

DO循环骨架
	DO 循环变量 = E1,E2
		代码块
	ENDDO
	
///
	do i=1,10
		write(*,*) i
	enddo
	!实际上就是i初始为1，每执行一次猜码快i+1，直到i+1之后>10，退出循环的过程
	
///
	!循环变量的增量是可以变的
	do i=1,10,4
	!就是i从1开始，每次加4，执行出来i分别等于1，5，9
	!注意循环变量退出循环后依然存在
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
示例代码
program main
    real ::plus=1
    real::r=1
    do i=1,100 
    	!循环变量叫i，i每次循环从1到100依次+1
        plus=((2*i)*(2.0*i))/((2*i-1)*(2*i+1))
        write(*,*)plus
        r=r*plus
        write(*,*)n
		!do中的代码块，每次都执行这些，然后结束后i+1，不断循环，最后i+1>100时候退出循环
    end do
    write(*,"(f11.9)")r
    pause
    
    end
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
2.DO-WHILE循环
满足逻辑表达才执行，否则就退出循环

DO-WHILE框架
	do while(逻辑表达式)
		!代码块
	enddo

!满足逻辑表达才执行，否则就退出循环
1
2
3
4
5
-示例代码

program main
    integer ::a=1
    do while(a<10)
        a=a+1
        write(*,*)a
    enddo
    !输出1,2,3,4,5,6,7,8,9,10,最后a=10,不满足a<10 就不走循环接下去了
    pause
 end
1
2
3
4
5
6
7
8
9
3.退出循环语句
EXIT 用来退出当前的循环体，相当于break
CYCLE 用来退出当前这轮循环，进入下一轮循环，相当于continue
4.如何选择两种循环
do循环适合已知循环次数的循环，可以自动增量，不用谢逻辑表达式
do while循环适合不知道循环次数的循环
5. forall循环
forall循环类似隐式循环，但是功能更强大，适合数组的遍历
在forall 和 endforall中包裹起来代码块
在forall中不能进行write操作，
program main
    integer ::a(2,2) = (/ 1,2,3,4 /)
    forall(i=1:2, j=1:2)
        a(i,j) = 0
    endforall
    write(*,*) a
    pause
end
1
2
3
4
5
6
7
8
筛选操作
在forall循环中可以对操作到的元素进行筛选，既可以是下标的筛选，也可以是元素的筛选
在forall(…, mask), …代表的是i,j,k等等用来遍历下标的变量，mask就是填的逻辑判断条件
这个例子就是对下标i==j的条件的时候让这个下标对应值改为0
program main
    integer ::a(2,3) = (/ 1,2,3,4,5,6 /)
    forall(i=1:2, j=1:3,i==j)
        a(i, j) = 0
    endforall
    write(*,*) a
    pause
end
1
2
3
4
5
6
7
8
这个例子就是对元素进行判断，满足的才执行
! forall用法
program main
    integer ::a(2,3) = (/ 1,2,3,4,5,6 /)
    forall(i=1:2, j=1:3,a(i,j)>3)
        a(i, j) = 0
    endforall
    write(*,*) a
    pause
end
1
2
3
4
5
6
7
8
9
5.输入语句
read(,)

表控输入语句
read(*,*) a,b,c
read *,a 两种都可以
!表示从键盘输入并且赋值给变量


program main
   integer a,b,c,d
   read (*,*)a,b,c,d
   write(*,*)a,b,c,d
   pause
   end
! 注意：1. 输入的时候用, 或者空格分割
   	   2. 输入的数量不小于变量数量，多余的不起作用
   	   3. 输入的类型匹配，输入实型给整型变量会报错，输入实型给整型变量会自动转换 
1
2
3
4
5
6
7
8
9
10
11
12
13
14
read(,)中第一个表示从键盘输入，第二个指输入格式，默认是*不用改
6.输出语句
print *,[输出表]

print只能最普通地输出，不能格式化输出，*代表从默认设备（显示器）输出
write()

write(* , * ) 第一个*表示输出设备（显示器or文件中），第二个星表示输出格式
格式化输出format
	integer ::a=100
	write(*,200)a  !200表示名叫200的format格式
	200 format(I4) !format里填的是各种类型的输出格式，一一对应
1
2
3
format中的格式(常用的)
整型：Iw w表示共显示多少个字符，超过了就左边用空格代替，不足就显示**，不显示数据
   integer ::a=345
   write(*,200)a
200 format(i2) =>显示**
200 format(i5) =>显示”  345“左边有俩空格
//
实型：Fw.d w表示占位多少个字符宽度，d表示小数部分占几个字符宽度
   real ::b=24.687
   write(*,200)b
200 format(f10.5)
=>显示'  24.68700'多的左边补空格，小数位数多的用0补
=> 小数位数比原来值小就四舍五入掉多的位数，总共位数小于整数部分，显示*
=> 总之只要使得整数部分不能完整表达，直接*

字符串:Aw w表示显示位数
   character(5) ::c='hello'
   write(*,200)c
200 format(A3)
=>多了左边补空格，少了直接截去

空格： nX n表示输出位置向右移动几格
   character(5) ::c='hello'
   write(*,200)c
200 format(6x,A3)
=>向右移动6个位置之后再输出

带上字符串的输出
   character(5) ::c='hello'
   write(*,200)c
200 format('a=',A3)
=> a=hel 

1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31
在write中直接写格式
在第二个星号的地方用双引号括起format中写的东西
   real ::b=24.687
   character(5) ::c='hello'
   write(*,"(f6.3,A3)")b,c
   write(*,"('b=',f6.3)")b
1
2
3
4
. 写程序中容易犯的错误
声明变量且赋值的时候不加 ::
忘记写endif enddo 写结构的时候先把选择/循环结构的结构写好，摆好缩进在写代码块
逻辑运算忘记加. 每个逻辑运算两端都有 ex: 4>3 .and. 5>4
循环变量的表达式不要写成i=1:4 习惯了别的语言有时候容易错
二进制的读写加了*，写成open(2,)，注意二进制读写只用写open(2)，没有
数组大小不能使用变量，但是可以用parameter后的变量
待~
五. 数组
重中之重

1.什么是数组
是一组具有同一类型的变量组成
占据一段连续的储存单元
数组中包含每个数据称为数组元素，通过下标访问每个元素

2.数组的定义
定义格式：
定义一维数组：
	datatype name(size)
	!datatype可以是任意类型
	integer array1(5) =>声明了一个长度为5，类型为实型的数组array1
	integer a(6),b(8) =>同时定义两个数组

定义多维数组：
	integer array2(3,5)
	=>size是3*5，就是一个三行五列的数组

特殊定义：
	integer array3(6:10)=>定义了一个一维数组，但是下标是从6到10
1
2
3
4
5
6
7
8
9
10
11
12
3. 数组引用
下标引用 ——用(下标)来引用
一维：
	integer a(5) =>定义了一个一维有五个元素的数组
	a(5)=6 =>6赋值给数组a的第五个元素
二维：
	integer b(6,8) =>定义了一个6*8的二维数组
	b(2,1)=6 => 6赋值给b数组的第二行第一列元素
1
2
3
4
5
6
切片引用——用:可以表示一段数组
	a(5:8)
	=>表示a中5到8的数组切片
	b(2:4,6:8)
	=>表示二维数组中的切片
	c(1:10:2)
	=>用三元表达式（起始下标，终止下标，步长）取出不连续的切片，取出c(1),c(3)...c(9)
1
2
3
4
5
6
4.数组的储存结构
二维数组定义时先行后列 real a(2,4)=>定义了两行四列的数组
逻辑结构：就是几行几列和平时一样
储存结构：按列顺序存储

5.数组的输入输出
使用do循环输入输出
!通过二重循环遍历二维数组进行读写
	integer a(10,4)
	do i=1,10
		do j=1,4
			read(*,*)a(i,j)
		enddo
	enddo

	do i=1,10
		do j=1,4
			write(*,*)a(i,j)
		enddo
	enddo
1
2
3
4
5
6
7
8
9
10
11
12
13
使用隐式循环读写
隐式循环：列循环，do循环是行循环，配合使用可以快速读取行列，在读取气象文件时很好用
一维数组，只有一行，用隐式循环就可以解决
	program main
	   integer a(5)
	   read(*,*)(a(i),i=1,5)
	   write(*,*)(a(i),i=1,5) 
	   pause
	end

二维数组，每一行用do循环包裹，内部用隐式循环
program main
   integer a(5,3)
   do i=1,5
       read(*,*)(a(i,j),j=1,3)
       write(*,*)(a(i,j),j=1,3)
   enddo 
   pause
    end

1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
6.给数组赋初始值
使用data赋值
real a(5),b(4,4)
data a /1,2,3,4,5/
!可以用*表示重复的内容
data a /5*2/ =>把五个2赋给了数组中元素
data b /1,5,9,13,2,6,10,14,3,7,11,15,4,8,12,16/ =>注意多维数组的顺序为列顺序
1
2
3
4
5
上述b的实际矩阵如下


用隐式循环进行循环,循环的下标从后面的/数据/中取
program main 
    integer a(5)
    data (a(i),i=2,4)/2,5,6/
    write(*,*) a
    pause
end
1
2
3
4
5
6
声明时直接赋值
	integer::a(5)=(/1,2,3,4,5/)
	!如果给每一个都赋一样的值，那可以简化
	integer ::a(5)=5
1
2
3
使用data赋值的时候，可以存在空值，会有默认值进行填充，但是在声明时直接赋值是不能有空值的
7. 数组运算
相同维数数组之间的加减乘除是各自相同位置的元素的加减乘除，返回一个一样大的数组，这里的除法和乘法不是线代里的矩阵乘法除法
数组倒序操作 a(1:10)=a(10:1:-1)
数组切片的修改 a(3:5)=(/2,3,4/)
8.数组的常用内置函数
all(a>5) =>判断a中元素是否全部大于5，返回值为布尔值
all(a>b) =>判断a中元素是不是每一个都比b中元素大，返回值为布尔值
/
maxloc(a) =>返回最大值的坐标
maxcal(a) =>返回最大值
minloc(a)
minval(a)
!如果是一维数组就返回只有一个元素的数组，用只有一个元素的数组接收，多维数组就返回对应坐标的一个数组
ex:program main
   integer a(3,2)
   data a /1,2,3,4,5,6/
   write(*,*) maxloc(a)
   pause
    end
    !返回的是(3，2)
ex: program main
	    integer :: a(5)=(/1,2,3,4,5/)
	    integer idex(1)
	    idex=maxloc(a)
	    write(*,*)idex(1)
	    pause
    end
对于一维数组，返回值也是一个数组，只有一个元素，但不能用整型去接收
//
sum(a) =>对整个a求和
sum(a(1:)) =>对第一行
sum(a(:3)) =>对第三列

1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
9. where
使用where和endwhere包裹的部分可以对数组进行筛选，并将筛选结果可以赋值给另一个相同维度的数组，会将对应满足条件保存到对应下标下，其他地方由默认值填充
和if的多分支语句一样，where也可以有多分支
在where(判断条件)就可以进行筛选
! 多分支where
program main
    integer ::a(5) = (/ 1,2,3,4,5 /)
    integer ::b(5) 
    where(a>3)
        !write(*,*)a
        b = a
    elsewhere(a==2)
        b = 10
    elsewhere
        b = -1
    endwhere
    write(*,*)a
    write(*,*)b
    pause
    end

1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
10. 可变长数组
有时候在一开始写代码的时候并不知道实际需要的数组有多长，定义少了不够用，定义多了又浪费
可以用allocatable关键字定义一个不知道长的数组
知道长度应该为多少后，用**allocate()**来配置内存空间，才真正将数组造出来
!可变长数组
program main
    integer stuNum
!   声明可变大小一维数组 
    integer, allocatable :: a(:)
    
    write(*,*)"有多少个学生"
    read(*,*)stuNum
    !   stuNUm已经有值之后，来配置内存空间，达到后定义数组长度的效果 
    allocate(a(stuNum))
    forall(i=1:stuNUm)
        a(i)=i
    endforall
    
    write(*,*)a
    pause
end

1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
11. 改变当前数组的长度
通过**deallocate(数组)**可以释放当前数组所占用的内存
释放之后再allocate就可以分配新的长度了
deallocate(a)
allocate(a(20))
1
2
六. 文件
重点，考试必考
主要是两种格式文件的操作：txt文本文件（有格式文件），二进制文件（无格式文件）

1.读文件
open关键字

看着很麻烦，但其实很多都有默认值，可以缺省

Unit：文件编号，在程序内部用来指定操作的是哪个文件,unit可以不写”unit“，直接写序号
open(20,file='E:\Grads\h4JJ-ave-pre.txt') 
!这个文件就是编号20文件，之后使用的时候都需要加上这个
write(20,*)'hello,world'
!往20号文件中写
1
2
3
4
file：文件的绝对路径
绝对路径就是路径加完整的文件名，这里就是’E:\Grads\h4JJ-ave-pre.txt‘

form：表示文件是二进制的还是文本文件，有两个选项，binary和 formatted，但是默认是formatted，只有在读二进制文件时才需要加上form=‘binary’
recl: 直接读取文件的时候需要填写recl项，指定文件中记录的长度
open(20,file='E:\Grads\h3\hgt-jj-79-10.grd',form='binary')
!注意：form=后面的binary或者formatte要用的单引号括起来
1
2
2.要记住的两个读写方式
读写操作分为三种：

文本文件顺序读写
二进制文件顺序读写
二进制文件直接读写
三个中前两个用的最多，必须记住读写操作的形式
1.文本文件顺序读写
open(unit=  ,file=' ')
read(unit= ,*)
write(unit= ,*)
close(unit= )
1
2
3
4
2.二进制文件顺序读取
open(unit= ,file='',form='binary') 
=>二进制文件的form就不是默认的formatted，需要手动写成binary
read(unit= )
write(unit= )
=> 注意！注意！注意！ 二进制文件不用加* 二进制文件不用加* 二进制文件不用加*
close(unit= )
1
2
3
4
5
6
3.二进制文件直接读取
open(unit=, file='',form='binary',access='direct',recl=4) =>recl指定了每一条记录的长度
read(unit= ,rec=第几条记录，一个整数)
write(unit= ,rec=第几条记录，一个整数)
close(unit= )
1
2
3
4
3.文件的读写
读写操作每次读一行，使用隐式循环配合可以将文件内容读出来放到数组中
写操作也是每次对应一行，write(unit,*)a,b,c,d 这一行就有a,b,c,d四个数
在写入时可以加上格式化输出让输出结果统一write(unit= ,fmt=“(F2.5)”)
program main
    real,parameter::year=20
    real data1(year,2)
    open(1,file='E:\1989-2008.txt')
    open(1,file='E:\1989-2008-2.txt')
    do i=1,year   
        read(1,*)(data1(i,j),j=1,2)
    end do

	do i=1,year
		write(2,*)2,5,6
	enddo
    close(1)
end
1
2
3
4
5
6
7
8
9
10
11
12
13
14


7.子程序
子程序可以理解成别的编程语言的方法或者叫函数，fortran中有两种子程序

subroutine：就叫子程序
function：函数

1.什么时候需要子程序
当代码中出现某个重复功能或重复使用某一段代码的时候，可以使用子程序——把一些需要重复使用的代码封装起来，通过传参的方式得到结果
好处在于：解耦合，提高代码的复用，减少多余的代码，是程序设计的重要思想
需要把功能抽象出来，提供一个接口供调用的地方访问即可
2. 函数子程序function
函数的思想在各个编程语言中更加通用，我本人也更喜欢使用函数

函数骨架
!主程序
program main
 external f1 =>声明f1是个函数 
 real f1 =>也要声明函数的返回值类型
 f1() =>调用函数

end 

!函数
real function f1(a,b) =>函数名为f1，返回值类型是个实型的函数，返回值的名字就叫f1,需要a,b作为参数
	!函数体
	real a,b =>如果主函数没有声明传来的变量类型，比如直接传了一个fun(3),需要声明传来参数的类型,如果传来的类型明确则不用在此声明
	f1=a+b =>最后要给返回值赋值
	return =>标志函数结束了
end

///
实例代码
program main
    external add
    real add
    write(*,*)add(2,3) =>返回5.0
    pause
    end
    
real function add(a,b)
    integer a,b
    add=a+b
    return
    end

1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
3. 子例行程序subroutine
骨架
program main
	external s1 =>声明s1是个子程序
	call s1(4,6) =>要用call 关键字调用
end

subroutine s1(a,b)
	integer a,b =>如果主函数没有声明传来的变量类型，比如直接传了一个fun(3),需要声明传来参数的类型,如果传来的类型明确则不用在此声明
	!子函数体
end

!示例代码
program main
    integer ::a=2,b=3
    external add2
    call add2(a,b)
    pause
end

subroutine add2(a,b)
    integer a,b
    write(*,*)a+b
    a=a+3
    b=b+4
    
end

1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
4. 传参的注意点
1. fortran是地址传参
4.fortran中传参是地址传参
向传参传过去的是地址而不是值，传过去的参数在函数/子程序中修改了，主函数中的变量值也会随之改变，必须注意这一点！

program main
    external fun
    integer ::a,x=17,b,fun
    a=fun(x)/fun(x)
    b=fun(x)-fun(x)
    write(*,*)a,b
    pause
    end
    
integer function fun(x)
    integer x  
    x=x/2	
    =>传来的x在此处的改动会改变主函数中的x的值，第一次进来时候x=17,第一次出去时x=8，当主程序a在计算分母的时候
    再次调用的时候x=8，之后以此类推
    fun=x*x
end

输出：4 3

1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
当传过去的时候是一个表达式的时候，子程序中修改的是表达式整体的量，而不是表达式中各个变量的值
program main
    real::a=3.0,b=5.0,c=4.0
    call subt(b-a,c) =>1. 传过去的第一个参数是b-a这个整体
    write(*,*)b-a,c => 3. 这里的b-a是用原函数中的b-a重新计算，5-2=3.0 
    pause
    end
    
subroutine subt(x,y)
    real x,y
    x=x+2 => 2. 修改的是b-a这个整体，原函数的b，a并没有被修改
    y=y+1
end
1
2
3
4
5
6
7
8
9
10
11
12
2. 数组参数
如果传递的参数是一个数组，传过去的也是数组的起始下标
因此接收的时候就会有如下情况：
从数组的某个位置将数组传过去，接收的也从这个位置出发
整个数组传过去，传过去的就是第一个的下标，在接收的时候要指定接收多少长度
! 数组传参
program main 
    integer :: a(5) = (/ 1,2,3,4,5 /)
    external sub1, sub2
    call sub1(a(2))
    call sub2(a)
    pause
end
    
subroutine sub1(a)
    integer :: a(3)
    write(*,*)a
    end
    
subroutine sub2(a)
    integer a(5)
    write(*,*)a
end

1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
八. 一个综合例子
 program main
    external rel =>声明rel函数
	parameter(nx=144,ny=73,nz=17,nt=32) =>声明了宏变量经纬度，层数，时间
	real data0(year,2),data1_temp(year),data1(year)
	real data2(nx,ny,nz,nt),res(nx,ny,nz,1)

	!两种文件的打开
	open(1,file='E:\Grads\h3\ISM-onset-1979-2010.txt')
	open(2,file='E:\Grads\h3\hgt-jj-79-10.grd',form='binary')
	read(1,*) =>第一行是表头，需要跳过
	do i=1,nt
		read(1,*) (data0(i,j),j=1,2)	=> do和隐式循环配合读取数据
		read(2)(((data2(i,j,k,n),i=1,nx),j=1,ny),k=1,nz) 
    enddo
    data1=data0(:,2) =>进行切片操作
    
	do i=1,nx
		do j=1,ny
			do k=1,nz
                res(i,j,k,1)=rel(data1,data2(i,j,k,:)) =>循环调用相关系数函数
			enddo
		enddo	
    enddo
    open(3,file='E:\Grads\h3\cor.ISM.JJ-hgt.79-10.grd',form='binary')
    write(3)res
	pause
end

 real function rel(x,y)
!计算相关系数函数，传入两个相同维数的数组
    integer,parameter::year=32
    real x(year),y(year),x_aver,y_aver,m1(year),m2(year),m3(year)
    !x向量，y向量，x平均数，y平均数，公式的分子，公式的两个分母
    x_aver=sum(x)/year
    y_aver=sum(y)/year
    do i=1,year
        m1(i)=(x(i)-x_aver)*(y(i)-y_aver)
        m2(i)=(x(i)-x_aver)**2
        m3(i)=(y(i)-y_aver)**2
    end do
    
    rel=sum(m1)/sqrt(sum(m2)*sum(m3))
    return
end

————————————————

                            版权声明：本文为博主原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接和本声明。
                        
原文链接：https://blog.csdn.net/m0_60394632/article/details/125230854