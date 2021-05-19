# windows 10 chia一键P盘
# 为windows10优化且预编译的Swar-Chia-Plot-Manager

#### chia 一键P盘软件， plotter已更新至chia 1.1.5: https://www.chia.net/
[中文介绍](README.md) [English](README-EN.md)


#### 全新电脑，仅需一行命令即可开始P盘

启动P盘程序
`manager.exe start`

查看最新状态
`manager.exe view`

停止所有P盘
`manager.exe stop`

## 运行截图
![start and stop the manager](https://github.com/don-lin/Windows10-Chia-Plot-Manager/blob/main/screenshot/start_stop.png?raw=true)
![view the manager](https://github.com/don-lin/Windows10-Chia-Plot-Manager/blob/main/screenshot/view.png?raw=true)



## 其他特点

* 错开开始P盘的时间，降低资源消耗峰值
* 可以同时选择多个P盘目标路径
* 在临时空间闲置时增加新的P盘任务
* 同时运行最多的P盘程序，避免资源瓶颈
* 更加直观的实时状态显示

## 如何编译源码 (如果你想要自己生成exe文件)

1. 去python官网下载 Python 3.7 或更高版本（推荐python 3.8）: https://www.python.org/
2. `git clone https://github.com/don-lin/Windows10-Chia-Plot-Manager.git`
3. 打开命令行 `cd` 到这个git目录.
   * 比如: `cd C:\Users\Donlin\Documents\Swar-Chia-Plot-Manager`
5. 安装所需的库: `pip install -r requirements.txt`
6. 编译manager.exe `pyinstaller -F manager.py`
7. 编译stateless-manager.exe `pyinstaller -F stateless-manager.py`



## 修改P盘设置

阅读 `config.yaml` 里面的内容，然后按需要修改每一项