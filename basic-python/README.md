# Python学习

1、修改字体大小

2、开发工具vscode settings中函数括号自动补全：`"python.analysis.completeFunctionParens": true`

3、运行时使用cmd命令，建议不要使用PowerShell

4、代码自动补全，第三方插件：python-snippets

5、代码格式化：autopep8

6、公网ip获取：[httpbin.org/ip](https://httpbin.org/ip)

# 一、基础语法

参考学习：[简介 - Python教程 - 廖雪峰的官方网站](https://liaoxuefeng.com/books/python/introduction/index.html)



# 二、常用类库





## python调用C/C++

通过ctypes(cpython)类库调用C/C++编译的动态链接库

# 三、工程化

## 1、python内置虚拟环境 venv解决环境冲突

需要（python3.3+）版本支持，虚拟环境类似一个容器环境下隔离python项目中依赖的三方包，避免python项目之间三方包版本冲突

安装venv环境：[venv - Python教程 - 廖雪峰的官方网站](https://liaoxuefeng.com/books/python/built-in-modules/venv/index.html)

```shell
# 1、进入项目路径
# 2、运行venv模块，会在目录下创建一个.venv, 通常用.venv目录 虚拟一个虚拟环境
python -m venv .venv
# 3、进入创建的文件夹下 执行命令，也就是激活虚拟环境，激活后这个项目就是用虚拟环境，而不是用的全局python环境
# windows下 进入 创建的虚拟环境（.venv）\Scripts\activate.bat  或者 （.venv）\Scripts\Activate.ps1
```







# 四、框架学习

1、数据计算三方包

1.1、numpy

1.2、pandas

1.3、Matplotlib



2、Web三方包

2.1、Flask



3、Http请求

3.1、requests



4、进度条包

4.1、progress

4.2、tqdm

4.3、alive-progress
