用Python制作一个PDF转Word工具
工具：Python3.9.13，VSCode1.73.1，pdf2docx0.5.6，tkinter，Win10Home
PDF文件不易编辑，想要编辑需要转成Word，但网上的工具很多要充VIP，所以今天我们就来做个PDF转Word工具。
首先先安装第三方库：

```bash
pip install tkinter
```

导入库：

```python
#coding=utf-8
import os
import tkinter
from pdf2docx import parse
from tkinter import filedialog
from tkinter.filedialog import askdirectory
from tkinter import *
from tkinter import ttk
from tkinter.messagebox import *
```

调用第三方库进行转换和选择文件操作：

```python
pdf_path = ""
_path_ = ""
def selectPath():
    global _path_
    _path_ = askdirectory()
    _path_ = _path_.replace("/", "\\")
    temp2.set(_path_)
    entry2.configure(textvariable = temp2)
def transPath():
    _path_ = str(entry2.get())
    for filename in os.listdir(_path_):
        if (filename.endswith(".pdf")):
            global pdf, docx
            pdf = os.path.join(_path_, filename)
            docx = pdf[:-4] + '.docx'
            parse(pdf, docx)
    showinfo("提示", "转换完成，文件保存在原目录！")
def trans():
    pdf_file = str(entry1.get())
    docx_file = pdf_file[:-4] + '.docx'
    parse(pdf_file, docx_file)
    showinfo("提示", "转换完成，文件保存在原目录！")
def getPath():
    temp = tkinter.Tk()
    temp.withdraw()
    global pdf_path
    pdf_path = filedialog.askopenfilename(title='选择PDF', filetypes=[('PDF Files', '*.pdf'), ('All Files', '*.')])
    temp1.set(pdf_path)
    entry1.configure(textvariable = temp1)
```

最后再搭建GUI界面：

```python
window = tkinter.Tk()
window.title('PDF转Word工具2.1   Powered by 印皓显')
window.geometry('700x450')
# window.resizable (0, 0)
text1 = 'PDF转Word工具'
lb = tkinter.Label(window, text = text1, width=13, height = 1, justify = 'center', anchor = 'nw', font = ('宋体',18), fg = 'white', bg = 'grey', padx = 10, pady = 5)
lb.place(x = 350, y = 25, anchor = "center")
current_work_dir = os.path.dirname(__file__)
img_png = PhotoImage(file = current_work_dir + '\\Pdf转word_clear_compress - 副本.gif')
label_img = Label(window, image = img_png)
label_img.place(x=180, y=50, anchor='n')
temp1 = StringVar()
entry1 = ttk.Entry(window, textvariable = temp1, width = 36)
entry1.place (x = 375, y = 100, anchor = "nw")
Button1 = ttk.Button(window,text="选择文件",command = getPath)
Button1.place(x = 375, y = 145, anchor = "nw")
Button2 = ttk.Button(window,text="一键转换",command = trans)
Button2.place(x=550, y=145, anchor="nw")
temp2 = StringVar()
entry2 = ttk.Entry(window, textvariable = temp2, width = 36)
entry2.place (x = 375, y = 300, anchor = "nw")
Button3 = ttk.Button(window,text="选择目录",command = selectPath)
Button3.place(x = 375, y = 350, anchor = "nw")
Button4 = ttk.Button(window,text="批量转换",command = transPath)
Button4.place(x = 550, y = 350, anchor = "nw")
window.mainloop()
```



所有代码：

```python
#coding=utf-8
import os
import tkinter
from pdf2docx import parse
from tkinter import filedialog
from tkinter.filedialog import askdirectory
from tkinter import *
from tkinter import ttk
from tkinter.messagebox import *
pdf_path = ""
_path_ = ""
def selectPath():
    global _path_
    _path_ = askdirectory()
    _path_ = _path_.replace("/", "\\")
    temp2.set(_path_)
    entry2.configure(textvariable = temp2)
def transPath():
    _path_ = str(entry2.get())
    for filename in os.listdir(_path_):
        if (filename.endswith(".pdf")):
            global pdf, docx
            pdf = os.path.join(_path_, filename)
            docx = pdf[:-4] + '.docx'
            parse(pdf, docx)
    showinfo("提示", "转换完成，文件保存在原目录！")
def trans():
    pdf_file = str(entry1.get())
    docx_file = pdf_file[:-4] + '.docx'
    parse(pdf_file, docx_file)
    showinfo("提示", "转换完成，文件保存在原目录！")
def getPath():
    temp = tkinter.Tk()
    temp.withdraw()
    global pdf_path
    pdf_path = filedialog.askopenfilename(title='选择PDF', filetypes=[('PDF Files', '*.pdf'), ('All Files', '*.')])
    temp1.set(pdf_path)
    entry1.configure(textvariable = temp1)
window = tkinter.Tk()
window.title('PDF转Word工具2.1   Powered by 印皓显')
window.geometry('700x450')
# window.resizable (0, 0)
text1 = 'PDF转Word工具'
lb = tkinter.Label(window, text = text1, width=13, height = 1, justify = 'center', anchor = 'nw', font = ('宋体',18), fg = 'white', bg = 'grey', padx = 10, pady = 5)
lb.place(x = 350, y = 25, anchor = "center")
current_work_dir = os.path.dirname(__file__)
img_png = PhotoImage(file = current_work_dir + '\\Pdf转word_clear_compress - 副本.gif')
label_img = Label(window, image = img_png)
label_img.place(x=180, y=50, anchor='n')
temp1 = StringVar()
entry1 = ttk.Entry(window, textvariable = temp1, width = 36)
entry1.place (x = 375, y = 100, anchor = "nw")
Button1 = ttk.Button(window,text="选择文件",command = getPath)
Button1.place(x = 375, y = 145, anchor = "nw")
Button2 = ttk.Button(window,text="一键转换",command = trans)
Button2.place(x=550, y=145, anchor="nw")
temp2 = StringVar()
entry2 = ttk.Entry(window, textvariable = temp2, width = 36)
entry2.place (x = 375, y = 300, anchor = "nw")
Button3 = ttk.Button(window,text="选择目录",command = selectPath)
Button3.place(x = 375, y = 350, anchor = "nw")
Button4 = ttk.Button(window,text="批量转换",command = transPath)
Button4.place(x = 550, y = 350, anchor = "nw")
window.mainloop()
```
![image](https://s1.ax1x.com/2023/07/14/pC4UENV.png))
最后用auto-py-to-exe打包成exe，再用Inno Setup打包成安装包。（~~无聊~~）
安装包下载
安装包：
链接：https://pan.baidu.com/s/1CtOdB9zJKKr2uCVKZtXtgA
提取码：1234
绿色版：
链接：https://pan.baidu.com/s/1pti8YRlGIrJQHUDbNqBQPQ
提取码：1234
（图片获取也可以在绿色版目录下找）
