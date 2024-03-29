# FORTRAN学习


## FORTRAN环境安装
使用Visual Studio + Intel Fortran (Intel Parallel Studio XE)，安装过程参考[这个博客](https://blog.csdn.net/Mr_JjPolarBear/article/details/89449667)

## FORTRAN学习
* FORmula TRANslator = FORTRAN
* FORTRAN $66\to77\to 90\to 95$
* FORTRAN 有 fixed format 格式（.for）和 free format 格式(.f90)，fixed format 格式对于每行前面6个字符有限制，现在主要用free format，但是要能看懂fixed format
* 空格无代码意义，大小写无区别
* C或者*顶格注释（对于fixed format），或者！注释(free format)；对于fixed format 运行代码必须前面有个tab键（从第7个位置开始）
    ```fortran
    C THIS IS A COMMENT
    ! THIS IS also A COMMENT
    ```
* .LT.表示less than ，即<，两者都可使用
    ```fortran
        IF (I .LT. 8)
        IF (I < 8)
    ```
* 可以控制跳到哪一行运行，起那么标识一个入口数字，如10在顶格
    ```fortran
        DO 10, WHILE(I .LT. 8)
        PRINT *, 'HELLO WORLD!'
        I = I + 1
    10  CONTINUE
    ```
* 输出
  * PRINT输出到屏幕，后接一个 *,代表输出格式不限定；
  * 而WRITE是可以输出到其他地方，可以制定输出位置和输出格式，需要带括号，常用``(*,*)``，其中第一个``*``指的是输出位置默认为屏幕，第二个指输出格式不限制
    ```fortran
        PRINT *, 'HELLO WORLD!'
        WRITE (*,*), 'HELLO WORLD'
    ```
  * 格式化输出可以用FORMAT定义格式，前面加代码号，再用 WRITE使用，也可也直接输入格式
    ```fortran
    10  format(I4)  !四个字符宽度输出整数
        write(*,10),100
        write(*,"(1x,I4)"),100 !输出位置向右跳过1位，并且按照4个字符宽度输出整数
    ```
* IF必须加括号, 后面加THEN, 结束后要END IF，如
    ```fortran
        IF (I .LT. 89) THEN
            PRINT *,GRADE
        END IF
    ```
    也可以直接不换行写执行语句，不用END IF ，类似单句C语言不用大括号
    ```fortran
        IF (i .LT. 89) PRINT *,GRADE
    ```
* READ 读入，多个一起输入可用逗号，空格，enter分隔
    ```FORTRAN
    READ *,YEAR
    READ (*,*),YEAR
    ```

* 使用&连接跨行代码，或者在第6位置写一个不是0的字符（fixed）

* 程序结束用``END``,这在77中是唯一的，90是可以用 ``END``、``END PROGRAM`` 、``END PROGRAM NAME``三种方式
* 数据类型：INTEGER,REAL,COMPLEX,CHARACTOR,LOGICAL
* 整数
  * 定义长整数用KIND=4，用2就是短整数，还可以用1
        ```fortran
        INTEGER(KIND = 4) A
        INTEGER(4) B
        INTEGER*4 C
        ```
* 变量命名不能是和关键词以及主程序名称相同，也不能重复声明
* 乘幂用**，如``2**3 = 8``
* 浮点数
  * 用E表示单精度的$\times 10^n$，D表示双精度的
    ```fortran
    a = 1E10
    B = 1D10
    ```
  * 注意用浮点数运算的时候一定加上小数点，如``1.0/3.0=0.33333``，若用``1/3=0 ``,尤其注意0.0和0的区别，一些函数输入需要是浮点数而不是整数，会报错
* 字符和字符串
  * 在FORTRAN90中字符串使用单引号或者双引号，而77使用单引号（大部分编译器现在也可用双引号）
  * 使用``CHARACTER``，字符串使用``CHARATER(10)``或者``CHARACTER(LEN = 10)``或者``CHARACTER*10``
  * 字符串中用:取出某些段，可以进行修改赋值
    ```fortran
    CHARACTER(20) STR
    STR = "HELLO WORLD!"
    STR(6:) = "KITTY!" !将第6位后的改为KITTY
    STR(1:6) = "DUANG" !将1~5位改为DUANG
    ```
  * 字符串合并：使用//
    ```fortran
    add = str1//str2
    ```
  * 字符串声明的长度和实际的长度不同，会用空格补齐，若要得到缩减后的字符串，可用``trim(string)``
  * 用``len(string)``可以获得声明的长度，用``len_trim(string)``可以获得实际长度
* 逻辑变量
  * ``logical``定义，``.true``表示真,``.false.``表示假，输出会给出``T``，``f``
* 不经定义直接使用的数，会根据变量名首字母判断类型，若是I,J,K,L,M,N开头，当作整数，其余当作浮点数。这种不定义就使用不好。加入``implicit none``关闭那种不好的定义。这必须放在``program name``的下一行
* 定义常数，用``parameter``
  * ```fortran
    real pi
    parameter(pi=3.14159)
    ```
  * 还可以用双冒号``::``和声明合并
  ```fortran
  real, parameter:: pi = 3.14159
  ```
* 声明的时候双冒号``::``表示形容词已经形容完毕，可以开始给变量定名称，因此要声明的时候定义，则使用双冒号。**所有的声明应该放在可执行代码之前**
  ```fortran
  real :: pi = 3.14159
  ```
* 自定义数据类型
  * 可以用``TYPE``自定义组合数据类型
    ```fortran
    type :: person
        character(len = 30) :: name
        integer :: age
        integer :: length
        integer :: weight
        character(len = 80) :: address
    end type person

    type(type)::hanwenbin !声明了一个person类型的变量hanwenbin
    ```
    取用其内容使用百分号``%``或者``.``（在visual fortran中可用）
    ```fortran
    hanwenbin%name  !取hanwenbin变量的name
    ```
    赋值可用整体赋值
    ```fortran
    hanwenbin = person("hanwenbin",21,168,55,"Tsinghua")
    ```
* 当用固定格式的.for时候，注意行的长度有限，有白色提示，如果超出了则要换行，不然会报错,可在代码行的前面一个位置加随意一个字符，连接两行
* 动态数组用```ALLOCATABLE```和```ALLOCATE```配合进行
  ```fortran
  REAL*8,ALLOCATABLE::T(:,:)
  N=10
  ALLOCATE(T(N,N))
  ```
* 数组可以整体赋相同值如```T(:,:)=10```
* 数组取第一行，即为```T(1,:)```
* 数组可以进行加减乘除操作（用的是逐点）如```T(:,1)+C(:)```表示T的第1列和C每个元素相加，若一边是一个常数，则是每点都加
* 数组的矩阵乘法是```matmul(A,B)```
* 数组求最大值是```maxval(A)```,求和是SUM(A)
* 在格式化输出的时候，若是不确定要输出多少个，如```WRITE(*,"(3(F12.3))")```中的3要改变，用变量替代，可以用加一个尖括号```<N>```
* 循环迭代可以步进小数，如```DO R=1.0,1.9,0.1```
* 
