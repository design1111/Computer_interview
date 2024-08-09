## 1、pyqt创建

配置Qt Designer和PyUIC

<img src="qt.assets/1116722-20180601150258845-574171932.png" alt="img" style="zoom:50%;" />

- Name：输入最后工具在菜单中的想呈现名称，比如我这里命名为 QtDesigner
- Program：designer.exe 程序的位置，就在上面安装的`C:\Users\feng\PycharmProjects\pythonProject\venv\Lib\site-packages\qt5_applications\Qt\bin\designer.exe`
- Working directory：designer.exe 工作路径，设置为  *$FileDir$*

![img](qt.assets/1116722-20180601151605421-493411367.png)

类似地添加 PyUIC，

- name：PyUIC

- Program：PyUIC 位于当前解析器的Scripts\pyuic5.exe，我这里是C:\Users\lei\AppData\Local\Programs\Python\Python39\python.exe

- Arguments：-m PyQt6.uic.pyuic $FileName$ -o $FileNameWithoutExtension$.py

- Working dirctory：*$FileDir$*

  

![img](qt.assets/1116722-20180601151811593-1976053363.png)



## 2、pyqt加载ui界面

### 使用Qt Designer开发

点击 Tools -> External Tools -> 选择 QtDesigner，则会打开 QtDesinger 的图像界面，在其上面进行编辑。

![img](qt.assets/v2-87474360d8153e2a750f931670dc9892_720w.jpg)

进入 Qt Designer 界面，选择 Main Window，点击 Create。

![img](qt.assets/v2-81fb0c4861598b95b982b6f2c2032e04_720w.jpg)

按 Ctrl+R 预览窗口，看是否是想要的界面。测试 OK，保存文件到 GUI 项目下。

![img](qt.assets/v2-f5754594b0744ec9f76fd2218322a2a5_720w.jpg)



回到 PyCharm，在 GUI 项目下面有一个 MainWindow.ui，就是刚在使用 Qt Designer 保存的文件，现在需要将这个文件转换成 .py 文件，在该文件上点击右键，选择 PyUIC。

![img](qt.assets/v2-91cd39fa1b6a96aa77a7988fe2db1b99_720w.jpg)



等程序运行完毕后，会多一个 MainWindow.py 的文件。

![img](qt.assets/v2-ebe281c8ba6e182dee4ac07e5a82d592_720w.jpg)



```

import sys

from PyQt6.QtWidgets import QApplication
from PyQt6 import uic

if __name__ == '__main__':
    app = QApplication(sys.argv)
    ui = uic.loadUi("./ui文件.ui")
    ui.show()

    sys.exit(app.exec())
```

