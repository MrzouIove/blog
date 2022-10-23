# Qt与python

## 1.搭建环境

```
pip install PyQt5

pip install PyQt5-tools

pip install PyQt5-tools -i http://pypi.douban.com/simple --trusted-host=pypi.douban.com

若出现错误使用如下纠正：

第一步，先删除原有的：

pip uninstall PyQt5

pip uninstall PyQt5-sip

第二步，安装指定版本的:

pip install PyQt5==5.11.3

pip install PyQt5-sip==4.19.19
```

## 2.pyqt第一个入门程序

```python
import sys
from PyQt5.QtWidgets import QApplication, QWidget

if __name__ == '__main__':
    # 创建QApplication的应用实例
    app = QApplication(sys.argv)
    # 创造一个窗口
    w = QWidget()
    # 指定窗口大小
    w.resize(300, 150)
    # 窗口的移动
    w.move(600, 400)
    w.setWindowTitle('第一个Qt应用')
    # 显示窗口
    w.show()
    # 进入程序主循环，并确保exit函数循环安全结束
    sys.exit(app.exec_())

```

### 2.1pyqt将ui文件转换为.py

> 第一种方式：
>
>  python -m PyQt.uic.pyuic demo.ui
>
> python -m PyQt5.uic.pyuic FirstUi.ui -o First.py



> 第二种方法
>
> 在idel中配置环境
>
> -m PyQt5.uic.pyuic $FileName$ -o $FileNameWithoutExtension$.py  

### 2.2 Qt中的布局

#### 2.2.1水平布局

#### 2.2.1 在python中使用qt

```python
import sys
from PyQt5.QtWidgets import QApplication, QWidget, QMainWindow
# 导入类
from text import First

if __name__ == '__main__':
    # 创建QApplication的应用实例
    app = QApplication(sys.argv)
    # 创建一个主窗口
    mainWin = QMainWindow()
    # 将ui导入主窗口
    ui = First.Ui_MainWindow()
    ui.setupUi(mainWin)
    mainWin.show()
    # 退出系统
    sys.exit(app.exec_())
```

#### 2.2.2薪资统计表

```python
import sys
from PyQt5.QtWidgets import QApplication, QMainWindow, QPushButton, QPlainTextEdit

if __name__ == '__main__':
    # 创建QApplication的应用实例
    app = QApplication(sys.argv)
    # 创建一个主窗口
    mainWin = QMainWindow()
    mainWin.resize(500, 400)
    mainWin.move(300, 200)
    textEdit = QPlainTextEdit(mainWin)
    textEdit.setPlaceholderText("输入薪资统计表")
    textEdit.resize(300, 350)
    textEdit.move(10, 25)
    mainWin.setWindowTitle('薪资统计')

    btn = QPushButton('统计', mainWin)
    btn.move(380, 80)
    mainWin.show()
    sys.exit(app.exec_())
```



