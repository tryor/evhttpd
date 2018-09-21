[English](README_en.md)

## evhttpd
evhttpd 是一个基于libev网络库，用C++语言编写的一个轻量级HTTP服务框架。此代码是几年前学习C++时闲着蛋疼整出来的，不知道有没有什么问题，仅共大家娱乐。若应用在生产环境，出问题别怪我。（o´・ェ・｀o）

## 使用evhttpd需要的条件:
* GCC > 4.7
* libev > 4.0 
* cmake > 3.10


## 操作系统
=======
* linux


## 安装依赖
 wget http://dist.schmorp.de/libev/libev-4.24.tar.gz
 tar xzvf libev-4.24.tar.gz 
 cd libev-4.24
 ./configure
 make & make install
 
## 安装evhttpd库
 wget https://github.com/tryor/evhttpd/releases/download/0.0.4/libevhttp-0.0.4.tar.gz
 tar xzvf libevhttp-0.0.4.tar.gz
 cd libevhttp-0.0.4
 ./configure
 make & make install
 

 
## 例子
```C++
#include <evhttp/HttpServer.h>
#include <evhttp/Config.h>
#include <evhttp/HttpServletFactory.h>
using namespace std;

class HelloHttpServlet : public HttpServlet{
public:
    HelloHttpServlet(){}
    virtual ~HelloHttpServlet(){}
    virtual void service(Request& req, Response& resp){
        resp.setContentType("text/html; charset=utf-8");
        resp.write("Hello World!");
    }
};

class TestHttpServletFactory : public HttpServletFactory{
public:
    TestHttpServletFactory(){}
    virtual ~TestHttpServletFactory(){}
    virtual HttpServlet* create(const char* path){
        return new HelloHttpServlet();
    }
    void free(HttpServlet* servlet){
        delete servlet;
    }
};

int main(int argc, char** argv) {
    Config config;
    config.workProcessCount = 3;
    TestHttpServletFactory servletFactory;
    HttpServer httpServer(3080, &servletFactory, &config);
    if(!httpServer.start()){
        return 1;
    }
    HttpServer::loop();
}
```

## 更多：
@see (https://github.com/tryor/evhttpd/releases/download/0.0.4/evhttpd_example.zip)

## 注意
如果找不到libev或libevhttp库，可试着执行下：ldconfig /usr/local/lib 或 ldconfig /usr/local/lib64


