# 需求
需要通过java程序执行nohup命令来启动外部jar（要以后台方式启动，所以用到了nohup）

# 方案
1. Runtime.getRuntime().exec(命令)
2. 用Apache Commons Exec

# 问题
在采用第一个方案的时候始终没有执行相应的jar。不知道要怎么启动。

# 最后
采用第二个方案解决。
```
CommandLine cmdLine = CommandLine.parse(命令);
DefaultExecutor executor = new DefaultExecutor();
executor.setExitValue(0); //貌似正常执行返回的是0， 尚不知道为什么
int exitValue = executor.execute(cmdLine);
```
