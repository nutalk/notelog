---
tags:
    - PYTHON
date: 2024-04-09
---

# python处理windows路径
在windows中用python处理文件的时候，需要输入文件路径。
## 在jupyter lab中使用路径
以下的写法都是正确能用的
``` py
from pathlib import Path

fpath = r'D:\next_cloud\newfile.txt'
fpath = 'D:\\next_cloud\\newfile.txt'
fpath = 'D:/next_cloud/newfile.txt'

fpath = Path(fpath)
```
但这三个方式都对原始的路径进行了一些修改。每次从windows文件管理器复制粘贴路径之后，需要手动修改总觉得有点烦人。
于是我写了个pypi包，来直接处理windows路径。
地址：[github](https://github.com/nutalk/slash_path),[pypi](https://pypi.org/project/slash-path/)

这样就能直接使用从windows的文件管理器复制过来的路径了。
```py
from slash_path import SlashPath
path_str = 'D:\next_cloud\newfile.txt'
sp = SlashPath(path_str)
```
SlashPath实例化之后获得的是Path对象，不改变使用习惯。

## 在命令行中使用路径
### powershell
在powershell，包括windows powershell和powershell v7中，直接用从windows的文件管理器复制过来的路径给Path对象就行。
main.py内容如下：
```py title="main.py"
import argparse
from pathlib import Path

if __name__ == "__main__":
    parser = argparse.ArgumentParser()
    parser.add_argument("fpath")
    args = parser.parse_args()
    run_config_dict = vars(args)
    fpath = Path(run_config_dict['fpath'])
    print(fpath)

```
在powershell中运行结果如下：
```sh
> python main.py D:\next_cloud\newfile.txt

D:\next_cloud\newfile.txt

```

### git bash
git bash会把反斜杠处理成转义符，所以在git bash中需要按下面的方式输入：
```sh
$ python main.py 'D:\next_cloud\newfile.txt'
D:\next_cloud\newfile.txt

$ python main.py D:\\next_cloud\\newfile.txt
D:\next_cloud\newfile.txt
```