---
title: "Python Spider Js Render"
# description:
date: 2021-06-29T14:54:23+08:00
draft: false
# tags:
# - ...
# categories:
# - ...
---

开源爬虫库很多，功能都差不多，比如大部分都不支持JS动态加载的页面:)，但是现在大部分网页都是JS动态加载的。后文以Python为例介绍几种支持JS动态加载网页的爬取方法。

##  什么是爬虫？
##  如何在爬虫时，支持JS动态加载的页面？
### 定制开发---性能高但开发成本及灵活性低
> 原理: 定向分析所要爬取的网页，用Python模拟JS的行为
### 使用一些浏览器自动化框架/库来爬取站点---通用性高开发成本低且灵活但效率低
> 原理: 模拟浏览器处理JS。利用外部JS Engine执行JS，并获取其结果
#### 1.Selenium WebDriver
> Selenium 是一系列工具和库的综合项目，这些工具和库支持 web 浏览器的自动化。
> WebDriver 以本地化方式驱动浏览器，就像用户在本地或使用 Selenium 服务器的远程机器上所做的那样，这标志着浏览器自动化的飞跃。
> Selenium WebDriver 指的是语言绑定和各个浏览器控制代码的实现。 这通常被称为 WebDriver。

[Requestium](https://github.com/tryolabs/requestium)
#### 2.dryscrape
> dryscrape is a lightweight web scraping library for Python. It uses a headless Webkit instance to evaluate Javascript on the visited pages. This enables painless scraping of plain web pages as well as Javascript-heavy “Web 2.0” applications like Facebook.
dryscrape 是一个轻量级的 Python 网页抓取库。它使用无头 Webkit 实例来评估访问页面上的 Javascript。这使得可以轻松抓取普通网页以及像 Facebook 这样的 Javascript 密集型“Web 2.0”应用程序。
``` python
def get_url_dynamic2(url):
    driver=webdriver.Firefox() #调用本地的火狐浏览器，Chrom 甚至 Ie 也可以的
    driver.get(url) #请求页面，会打开一个浏览器窗口
    html_text=driver.page_source
    driver.quit()
    #print html_text
    return html_text
get_text_line(get_url_dynamic2(url)) #将输出一条文本
```
#### 3.Splash
> Splash is a javascript rendering service. It's a lightweight browser with an HTTP API, implemented in Python using Twisted and QT.
Splash 是一个 javascript 渲染服务。它是一个带有 HTTP API 的轻量级浏览器，使用 Twisted 和 QT 在 Python 中实现。
最简单的使用方法是docker起一个Splash容器，然后调用Splash HTTP接口，详见[官方说明](https://splash.readthedocs.io/en/stable/install.html)。

##### 1.Pull the image:
```
$ sudo docker pull scrapinghub/splash
```
##### 2.Start the container:
```
$ sudo docker run -it -p 8050:8050 --rm scrapinghub/splash
```
##### 3.Splash is now available at 0.0.0.0 at port 8050 (http).

#### 4.[Pyppeteer](https://github.com/pyppeteer/pyppeteer)
puppeteer JavaScript（无头）chrome/chromium 浏览器自动化库的非官方 Python 端口
具体用法见[官方说明](https://github.com/pyppeteer/pyppeteer#installation)
