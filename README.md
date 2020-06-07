# scons
learn scons

## 从入门到精通 

https://blog.csdn.net/yu132563/article/details/40823385?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-3.nonecase&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-3.nonecase

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
    
