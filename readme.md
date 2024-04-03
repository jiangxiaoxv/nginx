# 学习路径

视频（https://www.bilibili.com/video/BV1yS4y1N76R?p=1&vd_source=5e17c856bfb7fe8e3c8387a111dcc329）、（https://www.bilibili.com/video/BV1PQ4y167f1/?spm_id_from=333.337.search-card.all.click&vd_source=5e17c856bfb7fe8e3c8387a111dcc329）
书籍（https://www.aliyundrive.com/s/Xqd21n8xp6Q/folder/621a581a06412ead338a43eb8b0c14a08ca91ee0）

# nginx 从入门到精通（set nu）

1.  docker 安装 centos7，安装 vim， 安装 nginx
    docker pull centos:7
    docker run -it --name mycentos centos:7 /bin/bash
2.  nginx 的优势
    - 是一个高性能的 http 和反向代理，也是一个 smtp 服务器；一个线程，通过记录跟踪每个 I/O 流的状态，来同时管理多个 I/O 流，通过拨开关的方式，来同时传输多个 I/O 流;(epoll 异步非阻塞)
    - IO 多路复用：输入与输出多路复用
    - 时分多路复用：cpu 时钟/中断设计
    - 频发多路复用：
3.  http 协议详解
4.  nginx 部署-yum
5.  nginx 编译参数
6.  基本配置

    - 主配置模块

      worker_processes: auto 默认起几个 nginx 为用户服务；一个 cpu 对应一个 nginx 进程
      error_log /var/log/nginx/error.log notice(日志级别) #错误日志

      // 配置影响 nginx 服务器或与用户的网络连接。有每个进程的最大连接数，选取那种事件驱动模型处理连接请求，是否允许同时接受多个网路连接，开启多个网路连接序列化等。
      events { // 事件驱动模块
      worker_connections 1024;// 可以同时为 1024 个用户服务
      }
      // 网络模块
      http {
      // 访问日志
      acccess_log /var/log/nginx/access.log main; 访问日志
      // 服务模块
      server {

      }
      }

7.  日志配置
    - ngx_http_log_module 内置模块
    - log_format
    - 日志轮转切割
    - 日志缓存 open_log_file_cache；open_log_file_cache max=1000 inactive=20s min_uses=3 valid=1m
8.  日志轮转/切割
    - rpm -ql nginx | grep log; /etc/logrotate.d/nginx;/var/log/nginx
    - /var/log/nginx;
    - 日志分析:
    - PV: 用户访问的页面数量
    - UV: 独立访问用户数，即 Unique Visitor，访问网站的一台电脑客户端为一个访客，根据 IP 地址来区分访客数
9.  - 日志分析

10. nginx web 模块
    - 连接状态模块 stub_status_module；
11. random_index on; // 随机主页
12. sub_filter 姜晓许 "嵩山实验室";
    sub_filter_once off;
    // 一键替换 nginx 内容
13. 文件读取
    - nginx_http_core_module
      - sendfile on | off; 磁盘读取优化
      - tcp_nopush 网络传输效率提升；
      - tcp_nodelay 开启 tcp_nopush 开启后，仅在讲链接转变为长连接的时候才被启用，确认包立即发送，不等待。
14. 文件压缩
    - ngx_http_gzip_module
      - gzip on | off
      - Default gizp off;
      - gzip on;
      - gzip_http_version 1.1;
      - gzip_comp_level 1;
      - gzip_types text/plain text/css image/png;
      - gzip_static on;
15. 页面缓存
    - ngx_http_headers_module
      - expires 起到控制页面缓存
        location / {
        root /usr/share/nginx/html;
        index index.html index.htm;
        expires 20m; # random_index on;
        }
16. 防盗链
    - valid_referers none blocked .\*.localhost;
      if ($invalid_referer) {
      return 403;  
      }
    - 防盗链
17. 访问限制(泛洪)
    - ngx_http_limit_req_module
      - 目的：限制 http 连接
      - 请求频率限制：
    - ngx_http_limit_conn_module
      - 限制 tcp 连接
18. nginx 访问控制
    - 基于主机
      - module: ngx_http_access_module
      - Directives: allow / deny
      - Syntax: allow address | CIDR | unix: | all
        - Context: http, server, location, limit_except
      - 启用控制：
    - 基于用户(username&password)
      - module: ngx_http_auth_basic_module
      - Syntax: auth_basic string | off
        - Context : http, server, location, limit_except
      - 启用控制:
        - 第一种 使用加密文件
          - server {
            auth_basic "请输入你的用户名和密码";
            auth_basic_user_file: /etc/nginx/conf.d/password; #加密文件地址
            }
        - 第二种 使用账户名密码
        -
