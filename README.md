# NJU-lib-Downloader
[南京大学馆藏数字化图书平台](http://114.212.7.104:8181/markbook/)的电子书爬虫。
<h2>文档</h2>
[JavaDoc](https://padeoe.github.io/nju-lib-downloader/)
<h2>使用示例</h2>
按在线阅读地址下载
```java
//通过在线阅读地址获取Book对象
Book book = Book.getBookFromUrl("http://114.212.7.104:8181/Jpath_sky/DsrPath.do?code=153BB79FEDBAFB093F90DDD4F90950EA&ssnumber=13488955&netuser=1&jpgreadmulu=1&displaystyle=0&channel=0&ipside=0");

//使用5个线程下载，保存到桌面
book.download("C:\\Users\\Username\\Desktop\\njulibpdf\\", 5);
```
按分类批量下载
```java
//中图法分类 计算机软件类
BookClass itClass = new BookClass("0T0P3010");

//获取分类下所有书
Set <Book> books = itClass.queryAllBooks();

//下载该分类下所有书籍
books.forEach(book -> book.download("C:\\Users\\padeoe\\Desktop\\libpdf", 5));
```

浏览分类
```java
//获取根分类
RootBookClass root = new RootBookClass();

//加载二级分类
root.loadChild();

//输出每个分类id，名字，下属图书数量
root.getChildren().forEach(
        child -> System.out.println(
            child.getId() + " " + child.getName() + " " + child.getBooksSize()
            )
        );
```
图书查询
```java
//查询所有2016年出版的图书
Set<Book> books2016 = new BookSearch().findAllBySQL("出版日期 = '2016'");

或者

//创建空的根分类
RootBookClass root = new RootBookClass();
//查询的同时建立root的子节点
new BookSearch().findAllBySQL("出版日期 ='2016'", root);
//在建立好的root中查找“工业技术图书馆>自动化技术、计算机技术”分类下的图书,支持中图法分类名和分类代号
Set<Book> itbooks2016 = root.getChild("工业技术图书馆").getChild("自动化技术、计算机技术").getBooks();
```

错误恢复
```java
new MissingPageCompletion("G:\\pageDLFail.txt").complete();//指定了错误日志路径
//或
new MissingPageCompletion().complete();//使用默认的错误日志路径
```
<h2>特别感谢</h2>
- [@Nifury](https://github.com/Nifury)
