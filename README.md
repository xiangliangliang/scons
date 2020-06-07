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
    
