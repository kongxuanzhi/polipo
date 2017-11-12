1. git分支记录阅读记录和重构记录
2. 尽可能的将代码跑起来
3. 写readme
4. 目标：拥有两天看懂一份源代码的能力

c语言中的宏定义中#号和##号和&符号的作用   --> config.h : 58
http://blog.chinaunix.net/uid-27666459-id-3772549.html
1. 配置参数类型：
    type 1
    CONFIG_INT: 整型
    CONFIG_OCTAL: 八进制
    CONFIG_HEX: 16进制
    CONFIG_TIME: 时间戳
    CONFIG_BOOLEAN: bool类型
    CONFIG_TRISTATE:   三状态tristate 代表在内核中有三种状态   https://zhidao.baidu.com/question/526127689.html
    CONFIG_TETRASTATE: 四状态
    CONFIG_PENTASTATE: 五状态
    type 2
    CONFIG_FLOAT 浮点数

    type 3
    CONFIG_ATOM  元素
    CONFIG_ATOM_LOWER 小写的atom
    CONFIG_PASSWORD 密码
    
    type 4
    CONFIG_INT_LIST 整数list
    
    type 5
    CONFIG_ATOM_LIST 元素list
    CONFIG_ATOM_LIST_LOWER 小写元素list
  
2. config的变量存放在全局变量 ConfigVariablePtr configVariables = NULL; 中

3. 配置变量是个包含联合体的对象结构

```cpp
typedef struct _ConfigVariable {
    AtomPtr name;
    int type;
    union { //全部是双指针
        int *i;    存放整型
        float *f;  浮点型
        struct _Atom **a; 一个元素
        struct _AtomList **al; 一个元素数组
        struct _IntList **il; 一个整型数组
    } value;
    int (*setter)(struct _ConfigVariable*, void*); 设置callback， 对存放的数据进行处理
    char *help;   说明字符串
    struct _ConfigVariable *next;  是个单链表元素
} ConfigVariableRec, *ConfigVariablePtr;
```

4. 语法：
  1. value = strtol(buf + offset, &p, 0);  [将字符串转化为长整型](http://www.jb51.net/article/71463.htm)
