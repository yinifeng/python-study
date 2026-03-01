# python-study
python学习

## 1、环境设置和VsCode设置
### 1.1、windows终端输出env
```shell

#  安装PowerShell 7以上版本
# Windows PowerShell Env
Get-ChildItem Env:

# Windows PowerShell Env 配置
C:\Users\XXX\Documents\WindowsPowerShell\Microsoft.PowerShell_profile.ps1

$OutputEncoding = [System.Text.Encoding]::UTF8
```
### 1.2、VsCode设置
- 方法一（通过菜单）
```text
按 Ctrl+Shift+P 打开命令面板
输入 Preferences: Open Settings (JSON)
```
- 方法二（直接文件路径）：
```shell
# 导航Windows路径文件
C:\Users\Administrator\AppData\Roaming\Code\User\settings.json
```
- 配置 terminal.integrated.env.windows
  
  在 settings.json 中添加或修改配置：
  ```json
  {
    // 其他配置...
    
    "terminal.integrated.env.windows": {
        // 1. 添加虚拟环境到 PATH（最简单的方式）
        "PATH": "${workspaceFolder}\\.venv\\Scripts;${env:PATH}",
        
        // 2. 或者设置 VIRTUAL_ENV 变量（某些工具会识别）
        "VIRTUAL_ENV": "${workspaceFolder}\\.venv",
        
        // 3. 自定义 Python 路径
        "PYTHONPATH": "${workspaceFolder};${workspaceFolder}/src",
        
        // 4. 其他环境变量
        "MY_CUSTOM_VAR": "some_value",
        "DJANGO_SETTINGS_MODULE": "myproject.settings"
    }
    
    // 其他配置...
  }
  ```

- 高级配置示例
  
  如果你想针对不同项目有不同的环境变量，可以结合使用`${workspaceFolder}`变量
  ```json
  {
    "terminal.integrated.env.windows": {
        // 自动检测并使用项目中的 .venv
        "PATH": "${workspaceFolder}\\.venv\\Scripts;${workspaceFolder}\\node_modules\\.bin;${env:PATH}",
        
        // Python 相关
        "PYTHONIOENCODING": "utf-8",
        "PYTHONUTF8": "1",
        
        // 如果使用 Conda
        // "CONDA_DEFAULT_ENV": "${workspaceFolderBasename}",
        
        // 开发工具
        "DEBUGPY": "1",
        "PYDEVD_DISABLE_FILE_VALIDATION": "1"
    }
  }
  ```

- 工作区级别的配置

  如果不想全局应用，可以为特定项目创建工作区配置：

  在项目根目录创建 `.vscode` 文件夹

  在里面创建 `settings.json` 文件

  添加相同的配置，但只对该项目生效：

  `.vscode\settings.json ` 
  ```json
  {
    "terminal.integrated.env.windows": {
        "PATH": "${workspaceFolder}\\.venv\\Scripts;${env:PATH}",
        "VIRTUAL_ENV": "${workspaceFolder}\\.venv"
    }
  }
  ```

- 验证配置是否生效

  配置保存后，打开 VS Code 的新终端，运行以下命令验证：
  ```shell
  # 查看 PATH 是否包含虚拟环境
  $env:PATH -split ';' | Select-String ".venv"

  # 查看 VIRTUAL_ENV 变量
  $env:VIRTUAL_ENV

  # 或者直接查看所有环境变量
  Get-ChildItem Env:
  ```

- 自动激活虚拟环境

  如果你想在打开终端时自动激活虚拟环境，可以结合使用 `terminal.integrated.profiles.windows`：  
  ```json
  {
    "terminal.integrated.profiles.windows": {
        "PowerShell (with venv)": {
            "source": "PowerShell",
            "args": [
                "-NoExit",
                "-Command",
                "& { if (Test-Path .venv) { .\\.venv\\Scripts\\Activate.ps1 } }"
            ]
        }
    },
    "terminal.integrated.defaultProfile.windows": "PowerShell (with venv)"
  }
  ```

- 调试环境变量

  如果配置不生效，可以：

  关闭所有 VS Code 窗口重新打开

  检查路径中的反斜杠是否正确转义（JSON中需要双反斜杠 \\）

  确保 ${workspaceFolder} 能正确解析到项目路径  

- 与其他扩展配合

  如果你使用 Python 扩展，可以在 `settings.json` 中添加
  ```json
  {
    "python.terminal.activateEnvironment": true,
    "python.terminal.activateEnvInCurrentTerminal": true,
    "python.defaultInterpreterPath": "${workspaceFolder}\\.venv\\Scripts\\python.exe"
  }
  ```  

## 2、python环境准备
### 2.1、python环境
`.python-version`确定python项目使用的版本

- 创建venv虚拟环境
```shell
python -m venv .venv
```

- 激活虚拟环境venv
```shell
source .venv/bin/activate
```

- 手动编辑`pyproject.toml`，格式样例
```toml
[project]
name = "python-study"
version = "1.0.0"
# 这里指定的版本需要去查询依赖包和版本，相对繁琐容易出错
dependencies = [
    "Flask==3.1.1"
]
```  

- pip安装依赖
```shell
pip install -e .
```

### 2.2、社区UV配置python环境（推荐）
- uv对应的项目
```text
main.py
pyproject.toml
```  

- 对于`pyproject.toml`配置
```toml
[project]
name = "python-study"
version = "1.0.0"
# 定义兼容的 Python 版本范围
requires-python = ">=3.9"
```  

- UV添加依赖
```shell
# uv会自动pyproject.toml文件添加依赖，同时创建.venv环境
# 并且打包当前项目安装到当前虚拟环境中
uv add flask
```  

- 当一个python项目需要多人协助，uv的项目通过`uv.lock`同步环境
```shell
# 这个命令会根据 pyproject.toml 和 requirements.txt 安装所有依赖，类似于 pip install -e . 但更高效
# 通过uv.lock文件，以下命令同步相关依赖和创建虚拟环境
uv sync
```  

- 激活虚拟环境的python运行环境
```shell

# Mac/Linux
source .venv/bin/activate

# Windows Cmd
.venv\Scripts\activate.bat

# Windows PowerShell
.venv\Scripts\Activate.ps1

# Windows PowerShell出现无法加载文件 .venv\Scripts\Activate.ps1，因为在此系统上禁止运行脚本。
# 执行以下命令临时解决
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser

# 验证查看Python解释器路径
where python
# 查看已安装的包（应该是虚拟环境中的包）
pip list

# 需要退出虚拟环境时
deactivate

```   

- 这里UV可以省略source激活环境
```shell
uv run main.py
```

说明：
```txt
uv sync 是一个依赖管理命令，它的作用类似于您可能更熟悉的 pip install -r requirements.txt，但更快、更强大、更可靠。

您可以把它理解为："一键安装这个项目正常运行所需的所有第三方软件包（依赖库）"。

uv sync 如果安装太慢，可以设置国内镜像源 https://pypi.tuna.tsinghua.edu.cn/simple：

在项目根目录的 pyproject.toml 文件 [tool.uv] 处设置 index-url：

[tool.uv]
index-url = "https://pypi.tuna.tsinghua.edu.cn/simple"
```


