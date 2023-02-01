# Makefile使用教程

记录makefile使用过程遇到的问题

- makefile嵌套使用

## makefile嵌套使用

在文件系统中，定义如何文件结构， 在test 文件下存在两个字文件夹，subdir1， subdir2, obj 是用来存放目标文件。

```shell
test
├── a.c
├── b.c
├── b.h
├── Makefile
├── obj
├── subdir1
│   ├── c.c
│   └── c.h
└── subdir2
    ├── d.c
    └── d.h
```

顶层Makefile内容如下 。

```makefile
.PHONY : clean

TARG 	:= test
# 获取当前路径, $( shell 命令)，是makefile调用shell命令的一种方法
CUR_DIR := $(shell pwd) 

# 定义头文件目录
INDIRS  :=  subdir1 \
		    subdir2 天气
# 定义源文件目录
SRCDIRS :=  $(CUR_DIR) \
			subdir1 \
		    subdir2 
# 使用patsubst函数生成 “-I subdir1 -I subdir2” ,在编译命令是时，
# 可以使用其引用头文件路径；
INCLUDS	:= $(patsubst %, -I %,$(INDIRS))
# $(wildcard $(dir)/*.c) 类似 %.c, 则在例程中表示如下：
# $(CUR_DIR)/a.c $(CUR_DIR)/b.c subdir1/c.c subdir2/d.c
# 最后赋值给CFILES变量，以空格符隔开。


CFILES  := $(foreach dir, $(SRCDIRS), $(wildcard $(dir)/*.c))
# $(notdir $(CFILES)) 去除文件路径， 只留下源文件名
COBJS   := $(notdir $(CFILES))
#  $(COBJS:.c=.o)) 将源文件名.c后缀改成.o后缀
OBJS	:= $(patsubst %, obj/%, $(COBJS:.c=.o))
# VPATH 源文件变量，当系统中提不到源文件，只会从该变量中搜索；
VPATH	:= $(SRCDIRS)

$(TARG) : $(OBJS)
	gcc $^ -o $@

$(OBJS):obj/%.o:%.c
	gcc -Wall $(INCLUDS) -c $< -o $@

clean:
	@echo INDIRS  : $(INDIRS)
	@echo INCLUDS :	$(INCLUDS)
	@echo CFILES  : $(CFILES) 
	@echo COBJS   : $(COBJS) 
	@echo OBJS    : $(OBJS) 
	rm obj/*.o $(TARG) -f  
```

