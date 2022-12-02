# my-stock
自选股票小工具


- pyinstaller打包

```
(venv) D:\Python\my-stock\src>..\venv\Scripts\pyinstaller.exe -w -F -i favicon.ico --add-binary ..\venv\Lib\site-packages\py_mini_racer\mini_racer.dll;. my-stock.py

```

- 打包完成，启动会报如下错误：

```
Traceback (most recent call last):
  File "my-stock.py", line 8, in <module>
  File "<frozen importlib._bootstrap>", line 991, in _find_and_load
  File "<frozen importlib._bootstrap>", line 975, in _find_and_load_unlocked
  File "<frozen importlib._bootstrap>", line 671, in _load_unlocked
  File "PyInstaller\\loader\\pyimod02_importers.py", line 499, in exec_module
  File "akshare\\__init__.py", line 4419, in <module>
  File "<frozen importlib._bootstrap>", line 991, in _find_and_load
  File "<frozen importlib._bootstrap>", line 975, in _find_and_load_unlocked
  File "<frozen importlib._bootstrap>", line 671, in _load_unlocked
  File "PyInstaller\\loader\\pyimod02_importers.py", line 499, in exec_module
  File "akshare\\futures\\futures_basis.py", line 27, in <module>
  File "akshare\\futures\\cons.py", line 498, in get_calendar
FileNotFoundError: [Errno 2] No such file or directory: 'C:\\\\Users\\\\ADMINI~1\\\\AppData\\\\Local\\\\Temp\\\\_MEI166802\\\\akshare\\\\file_fold\\\\calendar.json'

```

主要是因为没有把akshare的calendar.json打包进来，这时候需要写个hook，然后放进pyinstaller的hooks里面，再重新打包。

```
# 目录 my-stock\venv\Lib\site-packages\PyInstaller\hooks

# 文件名hook-akshare.py

(venv) D:\Python\my-stock\venv\Lib\site-packages\PyInstaller\hooks>dir
 驱动器 D 中的卷没有标签。
 卷的序列号是 934D-7A17

 D:\Python\learning\my-stock\venv\Lib\site-packages\PyInstaller\hooks 的目录

2022/12/01  16:56    <DIR>          .
2022/12/01  16:56    <DIR>          ..
2022/11/30  12:03                96 hook-akshare.py        <------
2022/11/30  10:21               649 hook-babel.py
2022/11/30  10:21               590 hook-difflib.py
2022/11/30  10:21             1,896 hook-distutils.py
2022/11/30  10:21               674 hook-distutils.util.py
2022/11/30  10:21               649 hook-django.contrib.sessions.py
2022/11/30  10:21               643 hook-django.core.cache.py
2022/11/30  10:21             1,094 hook-django.core.mail.py
2022/11/30  10:21               961 hook-django.core.management.py
2022/11/30  10:21               624 hook-django.db.backends.mysql.base.py
2022/11/30  10:21               575 hook-django.db.backends.oracle.base.py
2022/11/30  10:21             1,008 hook-django.db.backends.py
2022/11/30  10:21             4,014 hook-django.py
2022/11/30  10:21               640 hook-django.template.loaders.py

# 内如如下：

from PyInstaller.utils.hooks import collect_data_files
 
datas = collect_data_files("akshare")
```



```
# config.yaml

stocks:
  # 一行一个股票代码，需要带引号
  - "600519"
  - "002165"
  - "000858"
configuration:
  # 数据刷新时间（秒）
  flush_time: 3
  # 窗口透明度
  WindowOpacity: 0.2

```


- 截图：

<img src="https://raw.githubusercontent.com/dayerong/my-stock/main/截图.png" width="500" alt="help"/>