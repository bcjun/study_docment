# 第一个wxpython程序

> **面向过程**
>
> ~~~python
> import wx
> # 将创建一个应用程序对象
> app = wx.App()
> # 初始化一个对象设置
> frame = wx.Frame(None, title='Simple application')
> # 显示界面
> frame.Show()
> # 进入主循环
> app.MainLoop()
> ~~~
>
> **面向对象**
>
> ~~~python
> import wx
> 
> class Example(wx.Frame):
>     # 
>     def __init__(self, parent, title):
>         super(Example, self).__init__(parent, title=title,
>             size=(350, 250))
>         # wx.Frame.__init__(self,title=title) 参数可以传递进来,也可以给默认值
> 
> def main():
>     app = wx.App()
>     ex = Example(None, title='Sizing')
>     ex.Show()
>     app.MainLoop()
> 
> 
> if __name__ == '__main__':
>     main()
> ~~~

### 基础配置参数

> | 方法                                            | 描述                     |
> | ----------------------------------------------- | ------------------------ |
> | `Move(wx.Point point)`                          | 移动一个窗口到指定的位置 |
> | `MoveXY(int x, int y)`                          | 移动一个窗口到指定的位置 |
> | `SetPosition(wx.Point point)`                   | 设置窗口的位置           |
> | `SetDimensions(x, y, width, height, sizeFlags)` | 设置窗口的位置和大小     |
> | Centre()                                        | 窗口居中                 |
>
> **例子**
>
> ~~~python
> import wx
> 
> class Example(wx.Frame):
> 
>     def __init__(self, parent, title):
>         super(Example, self).__init__(parent, title=title,
>             size=(300, 200))
> 		self.Move((800, 250))
>         self.Centre()
> 
> def main():
>     app = wx.App()
>     ex = Example(None, title='Centering')
>     ex.Show()
>     app.MainLoop()
> 
> if __name__ == '__main__':
>     main()
> ~~~
>
> **初始配置**
>
> ~~~python
> # wx.Frame.__init__() 
> def __init__(self, parent=None, id=None, title=None, pos=None, size=None, style=None, name=None):
> 
> ~~~
>
> 一共七个参数
>
> + parent:父组件(默认是没有父组件的)
> + id:给定I
> + title : 窗口标题
> + pos : 窗口位置
> + size : 窗口位置
> + style:窗口样式
> + name:

~~~python
import wx
class MyFram(wx.Frame):
    def __init__(self):
        wx.Frame.__init__(self,None,title="这是第一个GUI编程",size=(200,300))
        self.InGui()
    def InGui(self):
        # 1.一般只创建一个menubar对象
        menubar = wx.MenuBar()
        # 2.一个MenuBar对象下面一般需要又多个Menu对象，Menu对象会返回一个menu item对象，用于绑定事件
        filemenu = wx.Menu()
        filemenu2 = wx.Menu()
        # 3.接收返回对象
        quit_window =filemenu.Append(wx.ID_NEW,"1")
        filemenu.Append(wx.ID_EXIT, "2")
        filemenu.Append(wx.ID_FILE1, "3")
        menubar.Append(filemenu,"feile")
        menubar.Append(filemenu2,"FILE")
        self.SetMenuBar(menubar)
        # 4.绑定事件，当点击子菜单1的时候自动退出
        self.Bind(wx.EVT_MENU, self.OnQuit, quit_window)
	# 5.定义事件
    def OnQuit(self, e):
        self.Close()

if __name__ == '__main__':
    app = wx.App()
    aa = MyFram()
    aa.Show(True)
    app.MainLoop()
~~~

# 常用应用

### 复选框

~~~python
import wx


class Example(wx.Frame):

    def __init__(self, *args, **kwargs):
        super(Example, self).__init__(*args, **kwargs)

        self.InitUI()

    def InitUI(self):

        menubar = wx.MenuBar()
        viewMenu = wx.Menu()
		
        self.shst = viewMenu.Append(wx.ID_ANY, 'Show statusbar',
            'Show Statusbar', kind=wx.ITEM_CHECK)
        self.shtl = viewMenu.Append(wx.ID_ANY, 'Show toolbar',
            'Show Toolbar', kind=wx.ITEM_CHECK)
		# 最重要的就是下面两行代码以及上面两行代码，实现了复选框
        viewMenu.Check(self.shst.GetId(), True)
        viewMenu.Check(self.shtl.GetId(), True)

        self.Bind(wx.EVT_MENU, self.ToggleStatusBar, self.shst)
        self.Bind(wx.EVT_MENU, self.ToggleToolBar, self.shtl)

        menubar.Append(viewMenu, '&View')
        self.SetMenuBar(menubar)

        self.toolbar = self.CreateToolBar()
        self.toolbar.AddTool(1, '', wx.Bitmap('exit.png'))
        self.toolbar.Realize()

        self.statusbar = self.CreateStatusBar()
        self.statusbar.SetStatusText('Ready')

        self.SetSize((450, 350))
        self.SetTitle('Check menu item')
        self.Centre()


    def ToggleStatusBar(self, e):

        if self.shst.IsChecked():
            self.statusbar.Show()
        else:
            self.statusbar.Hide()

    def ToggleToolBar(self, e):

        if self.shtl.IsChecked():
            self.toolbar.Show()
        else:
            self.toolbar.Hide()


def main():

    app = wx.App()
    ex = Example(None)
    ex.Show()
    app.MainLoop()


if __name__ == '__main__':
    main()
~~~

### 工具栏图标事件

~~~python

import wx


class Example(wx.Frame):

    def __init__(self, *args, **kwargs):
        super(Example, self).__init__(*args, **kwargs)

        self.InitUI()

    def InitUI(self):
		# 1.创建工具条对象
        vbox = wx.BoxSizer(wx.VERTICAL)
		# 2.创建工具条子对象
        toolbar1 = wx.ToolBar(self)
        # 3.将图标对象添加到子对象中
        toolbar1.AddTool(wx.ID_ANY, '', wx.Bitmap('tnew.png'))
        toolbar1.AddTool(wx.ID_ANY, '', wx.Bitmap('topen.png'))
        toolbar1.AddTool(wx.ID_ANY, '', wx.Bitmap('tsave.png'))
        # 4.实现
        toolbar1.Realize()
		# 2.
        toolbar2 = wx.ToolBar(self)
        # 3.
        qtool = toolbar2.AddTool(wx.ID_EXIT, '', wx.Bitmap('texit.png'))
        # 4。
        toolbar2.Realize()
		# 5.绑定
        vbox.Add(toolbar1, 0, wx.EXPAND)
        vbox.Add(toolbar2, 0, wx.EXPAND)

        self.Bind(wx.EVT_TOOL, self.OnQuit, qtool)

        self.SetSizer(vbox)

        self.SetSize((350, 250))
        self.SetTitle('Toolbars')
        self.Centre()

    def OnQuit(self, e):
        self.Close()


def main():

    app = wx.App()
    ex = Example(None)
    ex.Show()
    app.MainLoop()


if __name__ == '__main__':
    main()
~~~

### 启用和禁用 

~~~python
import wx

class Example(wx.Frame):

    def __init__(self, *args, **kwargs):
        super(Example, self).__init__(*args, **kwargs)

        self.InitUI()

    def InitUI(self):

        self.count = 5

        self.toolbar = self.CreateToolBar()
        tundo = self.toolbar.AddTool(wx.ID_UNDO, '', wx.Bitmap('tundo.png'))
        tredo = self.toolbar.AddTool(wx.ID_REDO, '', wx.Bitmap('tredo.png'))
        self.toolbar.EnableTool(wx.ID_REDO, False)
        self.toolbar.AddSeparator()
        texit = self.toolbar.AddTool(wx.ID_EXIT, '', wx.Bitmap('texit.png'))
        self.toolbar.Realize()

        self.Bind(wx.EVT_TOOL, self.OnQuit, texit)
        self.Bind(wx.EVT_TOOL, self.OnUndo, tundo)
        self.Bind(wx.EVT_TOOL, self.OnRedo, tredo)

        self.SetSize((350, 250))
        self.SetTitle('Undo redo')
        self.Centre()

    def OnUndo(self, e):
        if self.count > 1 and self.count <= 5:
            self.count = self.count - 1

        if self.count == 1:
            self.toolbar.EnableTool(wx.ID_UNDO, False)

        if self.count == 4:
            self.toolbar.EnableTool(wx.ID_REDO, True)

    def OnRedo(self, e):
        if self.count < 5 and self.count >= 1:
            self.count = self.count + 1

        if self.count == 5:
            self.toolbar.EnableTool(wx.ID_REDO, False)

        if self.count == 2:
            self.toolbar.EnableTool(wx.ID_UNDO, True)


    def OnQuit(self, e):
        self.Close()


def main():

    app = wx.App()
    ex = Example(None)
    ex.Show()
    app.MainLoop()


if __name__ == '__main__':
    main()
~~~

### 布局

~~~python

import wx

class Example(wx.Frame):

    def __init__(self, parent, title):
        super(Example, self).__init__(parent, title=title)

        self.InitUI()
        self.Centre()

    def InitUI(self):
		# 1.创建一个Panel对象，即画本对象
        panel = wx.Panel(self)
		# （2）.创建字体配置对象， 可以不配置
        font = wx.SystemSettings.GetFont(wx.SYS_SYSTEM_FONT)
		
        font.SetPointSize(9)
		# 创建一个盒子对象，
        vbox = wx.BoxSizer(wx.VERTICAL)
        # 创建一个盒子对象,也可以理解成 一行对象,这一行你需要放那些东西,你就加到这个对象中去
        hbox1 = wx.BoxSizer(wx.HORIZONTAL)
        # 创建一个静态文字对象
        st1 = wx.StaticText(panel, label='Class Name')
        st1.SetFont(font)
        hbox1.Add(st1, flag=wx.RIGHT, border=8)
        # 创建一个文本框对象,可以输入文字的
        tc = wx.TextCtrl(panel)
        hbox1.Add(tc, proportion=1)
        vbox.Add(hbox1, flag=wx.EXPAND|wx.LEFT|wx.RIGHT|wx.TOP, border=10)

        vbox.Add((-1, 10))

        hbox2 = wx.BoxSizer(wx.HORIZONTAL)
        st2 = wx.StaticText(panel, label='Matching Classes')
        st2.SetFont(font)
        hbox2.Add(st2)
        vbox.Add(hbox2, flag=wx.LEFT | wx.TOP, border=10)
        # 多行文本
        vbox.Add((-1, 10))
        hbox3 = wx.BoxSizer(wx.HORIZONTAL)
        tc2 = wx.TextCtrl(panel, style=wx.TE_MULTILINE)
        hbox3.Add(tc2, proportion=1, flag=wx.EXPAND)
        vbox.Add(hbox3, proportion=1, flag=wx.LEFT|wx.RIGHT|wx.EXPAND,
            border=10)
        #
        vbox.Add((-1, 25))
        # 复选框
        hbox4 = wx.BoxSizer(wx.HORIZONTAL)
        cb1 = wx.CheckBox(panel, label='Case Sensitive')
        cb1.SetFont(font)
        hbox4.Add(cb1)
        cb2 = wx.CheckBox(panel, label='Nested Classes')
        cb2.SetFont(font)
        hbox4.Add(cb2, flag=wx.LEFT, border=10)
        cb3 = wx.CheckBox(panel, label='Non-Project classes')
        cb3.SetFont(font)
        hbox4.Add(cb3, flag=wx.LEFT, border=10)
        vbox.Add(hbox4, flag=wx.LEFT, border=10)
        vbox.Add((-1, 25))
        # 长方形按钮
        hbox5 = wx.BoxSizer(wx.HORIZONTAL)
        btn1 = wx.Button(panel, label='Ok', size=(70, 30))
        hbox5.Add(btn1)
        btn2 = wx.Button(panel, label='Close', size=(70, 30))
        hbox5.Add(btn2, flag=wx.LEFT|wx.BOTTOM, border=5)
        vbox.Add(hbox5, flag=wx.ALIGN_RIGHT|wx.RIGHT, border=10)

        panel.SetSizer(vbox)


def main():

    app = wx.App()
    ex = Example(None, title='Go To Class')
    ex.Show()
    app.MainLoop()


if __name__ == '__main__':
    main()
~~~

