# scons
learn scons

http://www.linuxidc.com/Linux/2013-02/79467p3.htm


1.创建环境变量

    env = Environment()
    
   env.Program('foo.c')       根据不同系统创建不同的编译环境变量
   
    也可以改变：
    
    env = Environment(CC = 'gcc',CCFLAGS ='-O2')
    
    
2.复制环境变量

    env = Environment(CC = 'gcc')
    
    opt = env.Copy(CCFLAGS = '-O2')
    
    dbg = env.Copy(CCFLAGS = '-g')
    

3.获取环境变量

    env = Environment()
    
    print "CC is:",env['CC']
    
    
    
4.扩展环境变量

    env = Environment()
    
    print "CC is:",env.subst('$CC')
    
 
5.替换环境变量

    env = Environment(CCFLAGS ='-DDEFINE1')
    
    env.Replace(CCFLAGS = '-DDEFINE2')
    
    env.Program('foo.c')
    
    
6.尾部增加环境变量值

    env = Environment(CCFLAGS ='-DMY_VALUE')
    
    env.Append(CCFLAGS = ' -DLAST')
    
    env.Program('foo.c')
    
 
7.头部添加环境变量值

    env = Environment(CCFLAGS ='-DMY_VALUE')
    
    env.Prepend(CCFLAGS = '-DFIRST ')
    
    env.Program('foo.c')
    
    
7.2.6、多个构造环境 构造环境的真正优势是你可以创建你所需要的许多不同的构造环境，每一个构造环境对应了一种不同的方式去编译软件的一部分或其他文件。比如，如果我们需要用-o2编译一个程序，编译另一个用-g，我们可以如下做： 

opt=environment(ccflags='-o2') 

dbg=environment(ccflags='-g') 

opt.program('foo','foo.c') 

dbg.program('bar','bar.c')


我们甚至可以使用多个构造环境去编译一个程序的多个版本： 

opt=environment(ccflags='-o2') 

dbg=environment(ccflags='-g') 

o=opt.object('foo-opt','foo.c') 

opt.program(o) 

d=dbg.object('foo-dbg','foo.c') 

dbg.program(d)
    
    
https://www.sogou.com/link?url=LeoKdSZoUyCMNL1GYAdqYWDfV7I9B4wtZpR6MQDH4jfRLTf4YunY2bVFu5KmFeKi8dII-xH8uO9bXnmC1s1VPO4aBOKh09rc07uNVDQa5Va9b9AOmkVQjz3sXhKs5zAQreaVHN88tHY.


env = Environment()


# Set the include paths

env.Append(CPPPATH = ['src1', 'src2'])


# Notice the source files are referred to in the build dir

# If you dont do this, the compiled objects will be in the src dirs

src1Sources = ['build/lib1/class1.cc']

src2Sources = ['build/lib2/class2.cc']

mainSources = ['build/mainApp/main.cc']


env.VariantDir(variant_dir = 'build/lib1', src_dir = 'src1', duplicate = 0)

env.VariantDir(variant_dir = 'build/lib2', src_dir = 'src2', duplicate = 0)

env.VariantDir(variant_dir = 'build/mainApp', src_dir = 'srcMain', duplicate = 0)


lib1 = env.Library(target = 'build/lib1/src1', source = src1Sources)

lib2 = env.Library(target = 'build/lib1/src2', source = src2Sources)

env.Program(target = 'build/mainApp/main', source = [mainSources, lib1, lib2])


# 实用说明

## 1、使用scons编译程序： scons

## 2、清空编译程序：scons -c

## 3、将cpp或c文件编译为执行文件: 

3.1 Program('程序名', Split('程序文件序列 空格分隔'))  

3.2 Program('程序名', Glob('*.cpp'))

## 4、编译的时候想添加上-g -O2 -Wall参数

使用环境变量 env = Environment(CCFLAGS = ['-g', '-O3', '-Wall'])

生成程序 env.Program('程序名'， Glob('*.cpp'))

## 5、编译生成静态库

方法1：使用library关键字 

library('库名', split('程序文件序列,空格分隔') 或 library('库名', glob('*.cpp')) 

方法2：使用显示关键字staticlibrary 

staticlibrary('库名', split('程序文件序列,空格分隔')) 或 staticlibrary('库名', glob('*.cpp')) 

说明：库名不要前面的lib和后缀，如库libtestdota.a生成时的库名为testdota

## 6、编译生成动态库 

使用关键字sharedlibrary 

sharedlibrary('库名', split('程序文件序列,空格分隔')) 或 sharedlibrary('库名', glob('*.cpp')) 

说明：库名不要前面的lib和后缀，如库libtestso.so生成时的库名为testso

## 7、程序或库中引用已有的库 

使用关键字libs和libpath 

libs后跟程序需要引用的库名 

libpath后跟程序引用库的时候所需的库文件路径 

如： program('testpro','prog.c', libs = ['m','m2'], libpath = ['/usr/lib', '/usr/local/lib']) 

生成的testpro程序要引用库libm.so和libm2.so，库文件路径在/usr/lib和/usr/local/lib下

## 8、指定程序编译过程中需要查找的头文件路径 

使用关键字cpppath 

例如： program('hello','hello.c', cpppath = ['include', '/home/project/inc']) 

生成hello程序的时候hello.c可能引用了其他的头文件，在编译hello.c的时候会查找./include和/home/project/inc下是否有需要的头文件

## 9、打印依赖树

scons --tree=all

## 10、指明编译目标

scons -c -Q main2

说明： 如果有多个目标在sconstruct 文件内时不指定目标时会生成全部目标,指明目标时只生成目标

## 11、编译多个文件和文件的自动搜索

11.1 参数2位置可以是一个列表： Program('程序名',["main.cpp", "res/help.cpp"])

11.2 使用glob函数搜索源文件: 

file = Glob("*.cpp") + Glob("*/*.cpp")
Program("program_name", file)

## 12、拆分出汇编阶段和各个编译阶段的选项控制

env = environment()

env2 = env.clone()

env3 = env.clone()

12.1 设置汇编阶段

env["cc"] = "g++"

env["ccflags"] = ["-dccflag", "-s"]

env["objsuffix"] = ".s"

12.2 设置编译阶段

env2["as"] = "g++"

env2["asflags"] = ["-dasflags", "-c"]

env2["objsuffix"] = [".o"]

12.3 设置连接阶段

env3["link"] = "g++"

env3["linkflags"] = ["-dlinkflags"]

outfile1 = env.object("main.s", "main,cpp")

outfile2 = env2.object("main.o","main.s")

env3.program("abc","main.o")


原文链接：https://blog.csdn.net/fly542/article/details/38342003


## 13、分离编译文件

说明： Sconstruct 文件是一个工程的编译入口，一般在大型工程中不能将所有编译过程全部放到入口文件中，这样会导致工程可读性变差。

file1 = "build/file1" 

sconscript(file1)

## 14、节点对象

编译方法返回目标节点列表

hello_list = Object('hello.cpp', CCFLAGS='-dhello')

goodbye_list = Object('goodbye.cpp', CCFLAGS='-dgoodbye')

Program(hello_list + goodbye_list)

## 15、显示创建文件和目录节点

xyzzy=Entry('xyzzy')


## 16 环境

16.1 外部环境

通过Python的os.environ字典可以获得。你想使用外部环境的SConscript文件需要增加一个import os语句。a

16.2 构造环境

说明： 个构造环境是在一个sconscript文件中创建的一个唯一的对象，这个对象包含了一些值可以影响scons编译一个目标的时候做什么动作，以及决定从那一个源中编译出目标文件。scons一个强大的功能就是可以创建多个构造环境，包括从一个存在的构造环境中克隆一个新的自定义的构造环境。

16.3 创建一个构造环境： Environment函数

说明： 默认情况下，scons基于你系统中工具的一个变量集合来初始化每一个新的构造环境。当你初始化一个构造环境时，你可以设置环境的构造变量来控制一个是如何编译的。

import os

env = Environment(CC = 'gcc', CCFLAGS = '-O2')

env.Program('foo.c')

或者

env = Environment(CXX='/usr/local/bin/g++', CXXFLAGS = '-O2')

16.4 从一个构造环境中获取值

env = Environment(FOO = 'foo', BAR = 'bar')

dict = env.Dictionary()

for key in ['OBJSUFFIX', 'LIBSUFFIX', 'PROGSUFFIX']:
    print "key=%s, values=%s" % (key, dict[key])


16.5 多个构造环境

opt = Environment(CCFLAGS = '-O2')

dbg = Environment(CCFLAGS = '-g')

opt.Program('foo', 'foo.cpp')

dbg.Program('bar', 'bar.cpp')




