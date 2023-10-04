# Python制作网易云音乐下载器 2.0 版本
## 使用环境
Python 3.9.13 with urllib, tkinter, getpass
Windows 10 Home
## 正文
### 前言
众所周知，网易云音乐下载是要付费的，这让白嫖党十分头疼。于是我花了一年零零碎碎的世界，做了个网易云音乐下载器
### 运行效果
![运行效果](https://s1.ax1x.com/2023/07/16/pCIVjE9.png)
<video width="810" height="540" controls>
    <source src="https://75q8yl-my.sharepoint.com/personal/zno_75q8yl_onmicrosoft_com/_layouts/52/download.aspx?share=EcrnLS5XoBtJmqCVzf5ZNeUBMIH34OPuCegQFr7eiQV9wg" type="video/mp4">
</video>

### 整体代码
#### `search.py`:
```python
import urllib.request, urllib.parse, sys

def GetHtml(url):
    req = urllib.request.Request(url)
    with urllib.request.urlopen(req, timeout=5) as response:
        html = response.read()
    return html

def SaveHtml(file_name,file_content):
    with open(file_name.replace('/','_')+".html","wb") as f:  
        f.write(file_content)
        f.flush()
        f.close()
    with open(file_name.replace('/','_')+".html", "r", encoding="utf-8") as f:
        return f.read()

def Download(song):
    params = {'csrf_token': '', 'type': 1, 's': song}
    query_string = urllib.parse.urlencode(params)
    url = "http://music.163.com/api/search/get/web?" + query_string
    html = GetHtml(url)
    SaveHtml("Tmp", html)
```
#### `main.py`:
```python
# coding = UTF-8
import search, sys
from tkinter import *
from getpass import getuser
from tkinter import ttk
from os import path
from os import system
from tkinter.messagebox import *

UserName = getuser()
WorkDir = path.dirname(__file__)
Name = Artist = ""
Id = Id1 = Id2 = Id3 = Id4 = Id5 = 0
Nm1 = Nm2 = Nm3 = Nm4 = Nm5 = ""
Art1 = Art2 = Art3 = Art4 = Art5 = ""

def Initialize():
    global Id1, Id2, Id3, Id4, Id5
    Id = Id1 = Id2 = Id3 = Id4 = Id5 = 0
    Nm1.set(" 空 "); Nm2.set(" 空 "); Nm3.set(" 空 "); Nm4.set(" 空 "); Nm5.set(" 空 ")
    Art1.set(" 空 "); Art2.set(" 空 "); Art3.set(" 空 "); Art4.set(" 空 "); Art5.set(" 空 ")

def Download(ID, SongName):
    temp = "http://music.163.com/song/media/outer/url?id=" + str(ID) + ".mp3"
    system("aria2c -s 5 -x 5 -j 10 -d C:\\Users\\" + UserName + "\\Downloads\\MusicDownload "  + "-o " + SongName + ".mp3 " + temp)
    system("start C:\\Users\\" + UserName + "\\Downloads")
    showinfo("提示", "下载完成，歌曲保存在下载目录！")

def Down1():
    Download(Id1, Nm1.get())

def Down2():
    Download(Id2, Nm2.get())

def Down3():
    Download(Id3, Nm3.get())

def Down4():
    Download(Id4, Nm4.get())

def Down5():
    Download(Id5, Nm5.get())

def Fun(idpos, artpos, List):
    global Id, Name, Artist
    i = idpos + 5
    Id = 0
    while (str.isdigit(List[i])):
        Id = Id * 10 + int(List[i])
        i += 1
    i += 9
    Name = ""
    while (List[i] != "\"" or List[i - 1] == "\\"):
        Name += List[i]
        i += 1
    i = artpos
    while (List[i] != ","):
        i = i + 1
    i += 9
    Artist = ""
    while (List[i] != "\"" or List[i - 1] == "\\"):
        Artist += List[i]
        i += 1

def Parse(List):
    cnt = 0
    pos1 = pos2 = pos3 = pos4 = pos5 = -1
    art1 = art2 = art3 = art4 = art5 = -1
    Size = len(List)
    for i in range(Size - 4):
        print(List[i], end = "")
        if List[i] == "\"" and List[i + 1] == "i" and List[i + 2] == "d" and List[i + 3] == "\"" :
            cnt = cnt + 1
            if cnt % 4 == 1 :
                if pos1 == -1 : pos1 = i; continue
                if pos2 == -1 : pos2 = i; continue
                if pos3 == -1 : pos3 = i; continue
                if pos4 == -1 : pos4 = i; continue
                if pos5 == -1 : pos5 = i; continue
            elif cnt % 4 == 2:
                if art1 == -1 : art1 = i; continue
                if art2 == -1 : art2 = i; continue
                if art3 == -1 : art3 = i; continue
                if art4 == -1 : art4 = i; continue
                if art5 == -1 : art5 = i; continue
    print("\n", pos1, pos2, pos3, pos4, pos5)
    print("\n", art1, art2, art3, art4, art5)
    global Id1, Id2, Id3, Id4, Id5
    global Nm1, Nm2, Nm3, Nm4, Nm5
    global Art1, Art2, Art3, Art4, Art5
    Initialize()
    Fun(pos1, art1, List)
    Id1 = Id
    Nm1.set(Name)
    Art1.set(Artist)
    Fun(pos2, art2, List)
    Id2 = Id
    Nm2.set(Name)
    Art2.set(Artist)
    Fun(pos3, art3, List)
    Id3 = Id
    Nm3.set(Name)
    Art3.set(Artist)
    Fun(pos4, art4, List)
    Id4 = Id
    Nm4.set(Name)
    Art4.set(Artist)
    Fun(pos5, art5, List)
    Id5 = Id
    Nm5.set(Name)
    Art5.set(Artist)
def Search():
    Name = entry.get()
    search.Download(Name)
    with open("Tmp.html", encoding = "utf-8") as f:
        Tmp = f.read()
        f.close()
    Parse(Tmp)

def No_x():
    Label(window, text = "序号", font = ("微软雅黑", 12)).place(x = 75, y = 125, anchor = "center")
    Label(window, text = " 1 ", font = ("微软雅黑", 11)).place(x = 75, y = 160, anchor = "center")
    Label(window, text = " 2 ", font = ("微软雅黑", 11)).place(x = 75, y = 195, anchor = "center")
    Label(window, text = " 3 ", font = ("微软雅黑", 11)).place(x = 75, y = 230, anchor = "center")
    Label(window, text = " 4 ", font = ("微软雅黑", 11)).place(x = 75, y = 265, anchor = "center")
    Label(window, text = " 5 ", font = ("微软雅黑", 11)).place(x = 75, y = 300, anchor = "center")

def Nm_x():
    Label(window, font = ("微软雅黑", 12), text = " 歌曲名 ").place(x = 215, y = 125, anchor = "center")
    Label(window, font = ("微软雅黑", 10), textvariable = Nm1).place(x = 215, y = 160, anchor = "center")
    Label(window, font = ("微软雅黑", 10), textvariable = Nm2).place(x = 215, y = 195, anchor = "center")
    Label(window, font = ("微软雅黑", 10), textvariable = Nm3).place(x = 215, y = 230, anchor = "center")
    Label(window, font = ("微软雅黑", 10), textvariable = Nm4).place(x = 215, y = 265, anchor = "center")
    Label(window, font = ("微软雅黑", 10), textvariable = Nm5).place(x = 215, y = 300, anchor = "center")

def Art_x():
    Label(window, font = ("微软雅黑", 12), text = " 歌手名 ").place(x = 380, y = 125, anchor = "center")
    Label(window, font = ("微软雅黑", 10), textvariable = Art1).place(x = 380, y = 160, anchor = "center")
    Label(window, font = ("微软雅黑", 10), textvariable = Art2).place(x = 380, y = 195, anchor = "center")
    Label(window, font = ("微软雅黑", 10), textvariable = Art3).place(x = 380, y = 230, anchor = "center")
    Label(window, font = ("微软雅黑", 10), textvariable = Art4).place(x = 380, y = 265, anchor = "center")
    Label(window, font = ("微软雅黑", 10), textvariable = Art5).place(x = 380, y = 300, anchor = "center")

def Down_x():
    Label(window, font = ("微软雅黑", 12), text = " 下载 ").place(x = 500, y = 125, anchor = "center")
    ttk.Button(window, text = '下载', command = Down1).place(x = 500, y = 160, anchor = "center")
    ttk.Button(window, text = '下载', command = Down2).place(x = 500, y = 195, anchor = "center")
    ttk.Button(window, text = '下载', command = Down3).place(x = 500, y = 230, anchor = "center")
    ttk.Button(window, text = '下载', command = Down4).place(x = 500, y = 265, anchor = "center")
    ttk.Button(window, text = '下载', command = Down5).place(x = 500, y = 300, anchor = "center")

def main():
    global Nm1, Nm2, Nm3, Nm4, Nm5
    global Art1, Art2, Art3, Art4, Art5
    global window, entry
    window = Tk()
    window.title("网易云音乐下载器 2.0  Powered by 印皓显")
    window.geometry("600x350")
    window.iconphoto(False, PhotoImage(file = WorkDir + "\\ICON.png"))
    lb = Label(window, text = "网易云音乐下载器", width = 16, height = 1, justify = "center", anchor = "nw", font = ("宋体", 18), fg = "white", bg = "grey", padx = 10, pady = 5)
    lb.place(x = 300, y = 35, anchor = "center")
    Nm1  = StringVar(); Nm2 = StringVar(); Nm3 = StringVar(); Nm4 = StringVar(); Nm5 = StringVar()
    Art1  = StringVar(); Art2 = StringVar(); Art3 = StringVar(); Art4 = StringVar(); Art5 = StringVar()
    Initialize()
    sv1 = StringVar()
    sv1.set("在这里输入要搜索的歌名")
    entry = ttk.Entry(window, width = 40, justify = CENTER, font = ("微软雅黑", 10),  exportselection = 0, textvariable = sv1)
    entry.place(x = 55, y = 72.5)
    png = PhotoImage(file =  WorkDir + "\\Search.png")
    button_search = ttk.Button(window, image = png, command = Search)
    button_search.place(x = 450, y = 68)
    No_x(); Nm_x(); Art_x(); Down_x()
    window.mainloop()

if __name__ == "__main__":
    main()
```
### 附带文件
#### ICON.png:
![ICON.png](https://s1.ax1x.com/2023/07/14/pC4UpcQ.png)
#### Search.png:
![Search.png](https://s1.ax1x.com/2023/07/14/pC4US1g.png)
### 说明
#### 我的仓库与博客
##### 仓库
[我的代码仓库](https://github.com/YHX5129/NetEase-Cloud-Music-Downloader-V2.0)
里面有代码文件，dist目录里有编译过的可执行文件
![pC5YLOs.png](https://s1.ax1x.com/2023/07/15/pC5YLOs.png)
[我的Github大号](https://github.com/YHX5129/)
如果打不开，下载[FastGithub](https://wwt.lanzouw.com/ieQsu0feawda)，请不要使用FireFox访问，FireFox会拦截FastGithub的证书
##### 博客
[我的博客地址](https://zno2010.github.io/)
![pC5HsNn.png](https://s1.ax1x.com/2023/07/15/pC5HsNn.png)
[我的Github小号](https://github.com/ZnO2010/)
### 代码解析
#### `search.py`:
这个是我从CSDN上找的代码，[原博客在这里](https://blog.csdn.net/john_bian/article/details/71025372)
![pC5bVbQ.png](https://s1.ax1x.com/2023/07/15/pC5bVbQ.png)
但是，在实际调用的过程中，因为Python的byte和str屡屡报错，于是我喊来了ChatGPT，但是ChatGPT一次只解决一个问题，于是在与ChatGPT斗智斗勇了20多回后，代码变成了现在的样子，另外，网易云音乐的搜索API是http://music.163.com/api/search/get/web?csrf_token=&type=1&s= + 歌名。
其实本来第二行还有一行代码
```python
sys.stdout.reconfigure(encoding = 'utf-8')
```
但是用Pyinstaller（用的Auto-Py-To-Exe界面）时，这一行屡屡报错，删了也没影响，于是就删了。
由于这里代码我不熟悉，不做过多讲述，有兴趣的自行查阅。
#### `main.py`:
##### 导入需要的库
```python
# coding = UTF-8
import search, sys #search就是search.py，sys是上面search.py所说的删去的代码，可以不加
from tkinter import * #界面
from getpass import getuser #获取用户名，找到“下载”目录
from tkinter import ttk #tkinter的ttk，界面要好看，我习惯上只用里面的Text, Entry, Button。其他都是用tkinter的
from os import path #用于获取工作目录
from os import system #用于执行aria2命令行
from tkinter.messagebox import * #提示框
```
这里没有什么要讲的，要讲的在注释里。
##### 变量初始值
```python
UserName = getuser() #用户名
WorkDir = path.dirname(__file__) #工作目录
Name = Artist = "" #用于解析表单时的临时存储信息的变量
Id = Id1 = Id2 = Id3 = Id4 = Id5 = 0 #歌曲ID
Nm1 = Nm2 = Nm3 = Nm4 = Nm5 = "" #全称Name，歌曲名字
Art1 = Art2 = Art3 = Art4 = Art5 = "" #全称Artist，歌手
```
##### 初始化
```python
def Initialize():
    global Id1, Id2, Id3, Id4, Id5
    Id = Id1 = Id2 = Id3 = Id4 = Id5 = 0
    Nm1.set(" 空 "); Nm2.set(" 空 "); Nm3.set(" 空 "); Nm4.set(" 空 "); Nm5.set(" 空 ")
    Art1.set(" 空 "); Art2.set(" 空 "); Art3.set(" 空 "); Art4.set(" 空 "); Art5.set(" 空 ")
```
因为要多次调用，所以把这个初始化写成函数。
注：Nm1~5在后面声明，是StringVar。
##### 下载
这里是通过歌曲ID调用aria2进行下载，编译的可执行文件里已经包含。
###### 主要函数：
```python
def Download(ID, SongName):
    temp = "http://music.163.com/song/media/outer/url?id=" + str(ID) + ".mp3"
    system("aria2c -s 5 -x 5 -j 10 -d C:\\Users\\" + UserName + "\\Downloads\\MusicDownload "  + "-o " + SongName + ".mp3 " + temp)
    system("start C:\\Users\\" + UserName + "\\Downloads")
    showinfo("提示", "下载完成，歌曲保存在下载目录！")
```
###### 供各个按钮使用的函数：
```python
def Down{x}():
    Download(Id{x}, Nm{x}.get())
```
实际中{x}要换成数字，我的范围是一到五，为节省篇幅写成这种形式。
例：
```python
def Down2():
    Download(Id2, Nm2.get())
```
由于Nm{x}是StringVar，使用前要get一下。
##### 解析表单
###### 代码：
```python
def Fun(idpos, artpos, List):
    global Id, Name, Artist
    i = idpos + 5
    Id = 0
    while (str.isdigit(List[i])):
        Id = Id * 10 + int(List[i])
        i += 1
    i += 9
    Name = ""
    while (List[i] != "\"" or List[i - 1] == "\\"):
        Name += List[i]
        i += 1
    i = artpos
    while (List[i] != ","):
        i = i + 1
    i += 9
    Artist = ""
    while (List[i] != "\"" or List[i - 1] == "\\"):
        Artist += List[i]
        i += 1

def Parse(List):
    cnt = 0
    pos1 = pos2 = pos3 = pos4 = pos5 = -1
    art1 = art2 = art3 = art4 = art5 = -1
    Size = len(List)
    for i in range(Size - 4):
        print(List[i], end = "")
        if List[i] == "\"" and List[i + 1] == "i" and List[i + 2] == "d" and List[i + 3] == "\"" :
            cnt = cnt + 1
            if cnt % 4 == 1 :
                if pos1 == -1 : pos1 = i; continue
                if pos2 == -1 : pos2 = i; continue
                if pos3 == -1 : pos3 = i; continue
                if pos4 == -1 : pos4 = i; continue
                if pos5 == -1 : pos5 = i; continue
            elif cnt % 4 == 2:
                if art1 == -1 : art1 = i; continue
                if art2 == -1 : art2 = i; continue
                if art3 == -1 : art3 = i; continue
                if art4 == -1 : art4 = i; continue
                if art5 == -1 : art5 = i; continue
    print("\n", pos1, pos2, pos3, pos4, pos5)
    print("\n", art1, art2, art3, art4, art5)
    global Id1, Id2, Id3, Id4, Id5
    global Nm1, Nm2, Nm3, Nm4, Nm5
    global Art1, Art2, Art3, Art4, Art5
    Initialize()
    Fun(pos1, art1, List)
    Id1 = Id
    Nm1.set(Name)
    Art1.set(Artist)
    Fun(pos2, art2, List)
    Id2 = Id
    Nm2.set(Name)
    Art2.set(Artist)
    Fun(pos3, art3, List)
    Id3 = Id
    Nm3.set(Name)
    Art3.set(Artist)
    Fun(pos4, art4, List)
    Id4 = Id
    Nm4.set(Name)
    Art4.set(Artist)
    Fun(pos5, art5, List)
    Id5 = Id
    Nm5.set(Name)
    Art5.set(Artist)
```
这段代码有点长，但大部分都是重复的，我们分析一下。
我们把表单放出来看一看，例如Dissension的搜索结果（财迷审判曲，UT）
```
{"result":{"songs":[{"id":1999966349,"name":"Dissension","artists":[{"id":54748338,"name":"itsaleccio","picUrl":null,"alias":[],"albumSize":0,"picId":0,"fansGroup":null,"img1v1Url":"http://p2.music.126.net/6y-UleORITEDbvrOLV0Q8A==/5639395138885805.jpg","img1v1":0,"trans":null}],"album":{"id":155235630,"name":"Dissension","artist":{"id":0,"name":"","picUrl":null,"alias":[],"albumSize":0,"picId":0,"fansGroup":null,"img1v1Url":"http://p2.music.126.net/6y-UleORITEDbvrOLV0Q8A==/5639395138885805.jpg","img1v1":0,"trans":null},"publishTime":1668700800000,"size":1,"copyrightId":743010,"status":1,"picId":109951168074324026,"mark":0},"duration":290706,"copyrightId":743010,"status":0,"alias":[],"rtype":0,"ftype":0,"mvid":0,"fee":8,"rUrl":null,"mark":270464},{"id":1866749947,"name":"Dissension","artists":[{"id":844422,"name":"Dissension","picUrl":null,"alias":[],"albumSize":0,"picId":0,"fansGroup":null,"img1v1Url":"http://p2.music.126.net/6y-UleORITEDbvrOLV0Q8A==/5639395138885805.jpg","img1v1":0,"trans":null}],"album":{"id":131336481,"name":"Why Work For Death / We The Fooled","artist":{"id":0,"name":"","picUrl":null,"alias":[],"albumSize":0,"picId":0,"fansGroup":null,"img1v1Url":"http://p2.music.126.net/6y-UleORITEDbvrOLV0Q8A==/5639395138885805.jpg","img1v1":0,"trans":null},"publishTime":1353340800000,"size":22,"copyrightId":1416336,"status":1,"picId":109951166249307225,"mark":0},"duration":116866,"copyrightId":1416336,"status":0,"alias":[],"rtype":0,"ftype":0,"mvid":0,"fee":8,"rUrl":null,"mark":270336},{"id":1411740614,"name":"Dissension","artists":[{"id":15005023,"name":"Evenian","picUrl":null,"alias":[],"albumSize":0,"picId":0,"fansGroup":null,"img1v1Url":"http://p2.music.126.net/6y-UleORITEDbvrOLV0Q8A==/5639395138885805.jpg","img1v1":0,"trans":null}],"album":{"id":84325237,"name":"Tolerance for Pain","artist":{"id":0,"name":"","picUrl":null,"alias":[],"albumSize":0,"picId":0,"fansGroup":null,"img1v1Url":"http://p2.music.126.net/6y-UleORITEDbvrOLV0Q8A==/5639395138885805.jpg","img1v1":0,"trans":null},"publishTime":1472054400000,"size":10,"copyrightId":743010,"status":1,"picId":109951164577230899,"mark":0},"duration":276813,"copyrightId":743010,"status":0,"alias":[],"rtype":0,"ftype":0,"mvid":0,"fee":8,"rUrl":null,"mark":1319040},{"id":1465116058,"name":"Dissension","artists":[{"id":36120802,"name":"InfraSound Music","picUrl":null,"alias":[],"albumSize":0,"picId":0,"fansGroup":null,"img1v1Url":"http://p1.music.126.net/6y-UleORITEDbvrOLV0Q8A==/5639395138885805.jpg","img1v1":0,"trans":null}],"album":{"id":92897872,"name":"Cronos","artist":{"id":0,"name":"","picUrl":null,"alias":[],"albumSize":0,"picId":0,"fansGroup":null,"img1v1Url":"http://p1.music.126.net/6y-UleORITEDbvrOLV0Q8A==/5639395138885805.jpg","img1v1":0,"trans":null},"publishTime":1595433600000,"size":15,"copyrightId":743010,"status":1,"picId":109951167231342383,"mark":0},"duration":161600,"copyrightId":743010,"status":0,"alias":[],"rtype":0,"ftype":0,"mvid":0,"fee":8,"rUrl":null,"mark":537141376},{"id":1959646416,"name":"DISSENSION (U.S. Version)","artists":[{"id":53213473,"name":"NEMOPHILA","picUrl":null,"alias":[],"albumSize":0,"picId":0,"fansGroup":null,"img1v1Url":"http://p1.music.126.net/6y-UleORITEDbvrOLV0Q8A==/5639395138885805.jpg","img1v1":0,"trans":null}],"album":{"id":147203389,"name":"REVIVE (U.S. Version)","artist":{"id":0,"name":"","picUrl":null,"alias":[],"albumSize":0,"picId":0,"fansGroup":null,"img1v1Url":"http://p1.music.126.net/6y-UleORITEDbvrOLV0Q8A==/5639395138885805.jpg","img1v1":0,"trans":null},"publishTime":1655740800000,"size":11,"copyrightId":1416336,"status":1,"picId":109951167598994169,"mark":0},"duration":241466,"copyrightId":1416336,"status":0,"alias":[],"rtype":0,"ftype":0,"mvid":0,"fee":8,"rUrl":null,"mark":1318912},{"id":1493870931,"name":"Dissension","artists":[{"id":91154,"name":"Dissenter","picUrl":null,"alias":[],"albumSize":0,"picId":0,"fansGroup":null,"img1v1Url":"http://p1.music.126.net/6y-UleORITEDbvrOLV0Q8A==/5639395138885805.jpg","img1v1":0,"trans":null}],"album":{"id":98045629,"name":"Suicide Heights","artist":{"id":0,"name":"","picUrl":null,"alias":[],"albumSize":0,"picId":0,"fansGroup":null,"img1v1Url":"http://p1.music.126.net/6y-UleORITEDbvrOLV0Q8A==/5639395138885805.jpg","img1v1":0,"trans":null},"publishTime":1525017600000,"size":11,"copyrightId":743010,"status":1,"picId":109951165453328865,"mark":0},"duration":141174,"copyrightId":743010,"status":0,"alias":[],"rtype":0,"ftype":0,"mvid":0,"fee":8,"rUrl":null,"mark":270464},{"id":27940350,"name":"Dissention","artists":[{"id":844422,"name":"Dissension","picUrl":null,"alias":[],"albumSize":0,"picId":0,"fansGroup":null,"img1v1Url":"http://p1.music.126.net/6y-UleORITEDbvrOLV0Q8A==/5639395138885805.jpg","img1v1":0,"trans":null}],"album":{"id":2697941,"name":"Of Time and Chronic Disease","artist":{"id":0,"name":"","picUrl":null,"alias":[],"albumSize":0,"picId":0,"fansGroup":null,"img1v1Url":"http://p1.music.126.net/6y-UleORITEDbvrOLV0Q8A==/5639395138885805.jpg","img1v1":0,"trans":null},"publishTime":1378742400000,"size":10,"copyrightId":405025,"status":0,"picId":109951164467988110,"mark":0},"duration":297786,"copyrightId":405025,"status":0,"alias":[],"rtype":0,"ftype":0,"mvid":0,"fee":8,"rUrl":null,"mark":270336},{"id":1332495519,"name":"Dissension","artists":[{"id":87066,"name":"Audiomachine","picUrl":null,"alias":[],"albumSize":0,"picId":0,"fansGroup":null,"img1v1Url":"http://p1.music.126.net/6y-UleORITEDbvrOLV0Q8A==/5639395138885805.jpg","img1v1":0,"trans":null}],"album":{"id":74830026,"name":"A Measure of Darkness","artist":{"id":0,"name":"","picUrl":null,"alias":[],"albumSize":0,"picId":0,"fansGroup":null,"img1v1Url":"http://p1.music.126.net/6y-UleORITEDbvrOLV0Q8A==/5639395138885805.jpg","img1v1":0,"trans":null},"publishTime":1540828800000,"size":25,"copyrightId":602011,"status":0,"picId":109951163718448730,"mark":0},"duration":135616,"copyrightId":2707446,"status":0,"alias":[],"rtype":0,"ftype":0,"mvid":0,"fee":8,"rUrl":null,"mark":537010176},{"id":1991513182,"name":"Dissension","artists":[{"id":48257991,"name":"SimplyCrispy","picUrl":null,"alias":[],"albumSize":0,"picId":0,"fansGroup":null,"img1v1Url":"http://p1.music.126.net/6y-UleORITEDbvrOLV0Q8A==/5639395138885805.jpg","img1v1":0,"trans":null}],"album":{"id":153654543,"name":"Dissension","artist":{"id":0,"name":"","picUrl":null,"alias":[],"albumSize":0,"picId":0,"fansGroup":null,"img1v1Ur"fee":8,"rUrl":null,"mark":270336},{"id":1809937731,"name":"Dissension","artists":[{"id":30333025,"name":"Vincent Gericke","picUrl":null,"alias":[],"albumSize":0,"picId":0,"fansGroup":null,"img1v1Url":"http://p1.music.126.net/6y-UleORITEDbvrOLV0Q8A==/5639395138885805.jpg","img1v1":0,"trans":null}],"album":{"id":121277551,"name":"State of Mind","artist":{"id":0,"name":"","picUrl":null,"alias":[],"albumSize":0,"picId":0,"fansGroup":null,"img1v1Url":"http://p1.music.126.net/6y-UleORITEDbvrOLV0Q8A==/5639395138885805.jpg","img1v1":0,"trans":null},"publishTime":1612713600000,"size":4,"copyrightId":743010,"status":1,"picId":109951165624720372,"mark":0},"duration":312002,"copyrightId":743010,"status":0,"alias":[],"rtype":0,"ftype":0,"mvid":0,"fee":8,"rUrl":null,"mark":270464}],"songCount":300},"code":
```
其中一首歌曲有四个"id":······，分别为歌曲ID、歌手ID、歌单ID、歌单歌手ID。
我们需要用到的是歌曲ID和歌手ID，因此对"id"的数量对4取模，取1和2时的数据位置存储。
找到了数据位置之后，我们用Fun函数来提取信息。（我实在想不到函数名了）
Fun函数里没什么，最基础的循环。
##### 搜索
###### 代码：
```python
def Search():
    Name = entry.get()
    search.Download(Name)
    with open("Tmp.html", encoding = "utf-8") as f:
        Tmp = f.read()
        f.close()
    Parse(Tmp)
```
从搜索框中StringVar中提取文本，下载表单进行解析。
##### 枯燥的GUI搭建
```python
def No_x():
    Label(window, text = "序号", font = ("微软雅黑", 12)).place(x = 75, y = 125, anchor = "center")
    Label(window, text = " 1 ", font = ("微软雅黑", 11)).place(x = 75, y = 160, anchor = "center")
    Label(window, text = " 2 ", font = ("微软雅黑", 11)).place(x = 75, y = 195, anchor = "center")
    Label(window, text = " 3 ", font = ("微软雅黑", 11)).place(x = 75, y = 230, anchor = "center")
    Label(window, text = " 4 ", font = ("微软雅黑", 11)).place(x = 75, y = 265, anchor = "center")
    Label(window, text = " 5 ", font = ("微软雅黑", 11)).place(x = 75, y = 300, anchor = "center")

def Nm_x():
    Label(window, font = ("微软雅黑", 12), text = " 歌曲名 ").place(x = 215, y = 125, anchor = "center")
    Label(window, font = ("微软雅黑", 10), textvariable = Nm1).place(x = 215, y = 160, anchor = "center")
    Label(window, font = ("微软雅黑", 10), textvariable = Nm2).place(x = 215, y = 195, anchor = "center")
    Label(window, font = ("微软雅黑", 10), textvariable = Nm3).place(x = 215, y = 230, anchor = "center")
    Label(window, font = ("微软雅黑", 10), textvariable = Nm4).place(x = 215, y = 265, anchor = "center")
    Label(window, font = ("微软雅黑", 10), textvariable = Nm5).place(x = 215, y = 300, anchor = "center")

def Art_x():
    Label(window, font = ("微软雅黑", 12), text = " 歌手名 ").place(x = 380, y = 125, anchor = "center")
    Label(window, font = ("微软雅黑", 10), textvariable = Art1).place(x = 380, y = 160, anchor = "center")
    Label(window, font = ("微软雅黑", 10), textvariable = Art2).place(x = 380, y = 195, anchor = "center")
    Label(window, font = ("微软雅黑", 10), textvariable = Art3).place(x = 380, y = 230, anchor = "center")
    Label(window, font = ("微软雅黑", 10), textvariable = Art4).place(x = 380, y = 265, anchor = "center")
    Label(window, font = ("微软雅黑", 10), textvariable = Art5).place(x = 380, y = 300, anchor = "center")

def Down_x():
    Label(window, font = ("微软雅黑", 12), text = " 下载 ").place(x = 500, y = 125, anchor = "center")
    ttk.Button(window, text = '下载', command = Down1).place(x = 500, y = 160, anchor = "center")
    ttk.Button(window, text = '下载', command = Down2).place(x = 500, y = 195, anchor = "center")
    ttk.Button(window, text = '下载', command = Down3).place(x = 500, y = 230, anchor = "center")
    ttk.Button(window, text = '下载', command = Down4).place(x = 500, y = 265, anchor = "center")
    ttk.Button(window, text = '下载', command = Down5).place(x = 500, y = 300, anchor = "center")
```
自己看吧，tkinter的知识自己搜。
##### 主函数
```python
def main():
    global Nm1, Nm2, Nm3, Nm4, Nm5
    global Art1, Art2, Art3, Art4, Art5
    global window, entry
    window = Tk()
    window.title("网易云音乐下载器 2.0  Powered by 印皓显")
    window.geometry("600x350")
    window.iconphoto(False, PhotoImage(file = WorkDir + "\\ICON.png"))
    lb = Label(window, text = "网易云音乐下载器", width = 16, height = 1, justify = "center", anchor = "nw", font = ("宋体", 18), fg = "white", bg = "grey", padx = 10, pady = 5)
    lb.place(x = 300, y = 35, anchor = "center")
    Nm1  = StringVar(); Nm2 = StringVar(); Nm3 = StringVar(); Nm4 = StringVar(); Nm5 = StringVar()
    Art1  = StringVar(); Art2 = StringVar(); Art3 = StringVar(); Art4 = StringVar(); Art5 = StringVar()
    Initialize()
    sv1 = StringVar()
    sv1.set("在这里输入要搜索的歌名")
    entry = ttk.Entry(window, width = 40, justify = CENTER, font = ("微软雅黑", 10),  exportselection = 0, textvariable = sv1)
    entry.place(x = 55, y = 72.5)
    png = PhotoImage(file =  WorkDir + "\\Search.png")
    button_search = ttk.Button(window, image = png, command = Search)
    button_search.place(x = 450, y = 68)
    No_x(); Nm_x(); Art_x(); Down_x()
    window.mainloop()
```
注：Nm{x} = StringVar()那一段不能写成这样
```python
Nm1 = Nm2 = Nm3 = Nm4 = Nm5 = StringVar()
```
StringVar每次的返回值指向的对象不一样，如果写成这样的形式会让五个变量指向的对象一样，那五个名字会同步（不相信你试试？）。
最后调用main函数：
```python
if __name__ == "__main__":
    main()
``````
