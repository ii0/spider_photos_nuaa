# 使用Python写爬虫程序爬取全校所有人的照片、学号、姓名等信息

## 背景
偶然的一个发现，学校教务处网站有一个小小的漏洞，会泄露全校所有学生的学号、姓名、及照片。这个漏洞存在于新版教务处里，当登录教务处之后，点击“学生个人查询”-->“基本查询”，会出现当前登录学生的所有个人资料，同时附带个人的证件照，并且证件照之上附
带个人的姓名和学号信息，类似于下图：

![screen1](https://github.com/Root-lee/spider_photos_nuaa/blob/master/screen1.png)

漏洞便在这个照片之上。
我们右击图片查看图片属性：

![screen2](https://github.com/Root-lee/spider_photos_nuaa/blob/master/screen2.png)

然后将图片地址粘贴在浏览器的地址栏然后点回车，在浏览器里我们得到了我们的证件照。
我们注意到图片的地址是这种形式的：

*.../EASys/Controls/ImageHandler.ashx?ImageName=151220101&BinaryType=xh*

地址里的有一串代码：

ImageName=151220701

后面这串数字就是你的学号。

**下面重点来了：**
更改这个学号为另一个，比如此处我们将其改成：151220702 
然后点击回车，你会惊奇的发现现在浏览器里显示的是学号为151220702这名学生的照片！
而这张图片不仅仅是证件照，在其左下角还标注着此人的学号和姓名。

## 想法
1. 这个新版的教务处还未正式上线，有机会可以向负责任提交这个漏洞。
2. 每个人的证件照都是心中难灭的伤，可以用python爬虫将全校学生的证件照全部抓取下来，做一套api，开发一个用学号查询证件照的软件。（应该挺好玩，但影响似乎不太好。）
3. 不仅国内的大学，包括一些著名的公司，都不太注重网站的安全措施，很容易留下漏洞，被别有用心的人利用，造成极大的损失，尤其是你的数据库里还包含着大量的用户信息时，网站责任人的失责会造成极其重大的损失。真诚希望每个组织在这方面都能有所改进。

## 进展
1. date：2016.3.8  使用python写的爬虫软件初步完成图片抓取工作，可以抓取任意学号（前提是学号存在）的证件照。
2. date: 2016.3.10 可以爬取任意年级任意学院的所有学生信息，但速度有待提升，下次可以考虑多线程。
3. date: 2016.3.18 将Request对象改成“Session”，使得可以像普通浏览器一样自动保持Cookies。而不用每次cookie失效都要手动输入最新的cookie。
4. date: 2016.3.24 修复BUG，该BUG导致每次get请求都加上初始的headers导致Cookie依然会失效。增加了判断班级存在与否语句：如果连续三次无法得到学生信息，则默认后面没有学生信息了，开始下一个班级遍历，此做法可以保证程序时间不浪费在无用数据上。
5. date：2016.3.24 简化及修正了图片保存时的逻辑代码