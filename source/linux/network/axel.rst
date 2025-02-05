.. _axel:

==================
Axel下载工具
==================

我在下载大型软件时候，常常发现网站对单个连接进行限速，导致下载速度过慢。不过，通常网站允许多个连接同时下载，所以，使用下载加速工具可以加快下载速度。

我试用了 Axel 下载工具 (另一个推荐工具是 ``aria2`` ，还支持BitTorrent和Metalink协议下载 )，确实非常高效，简单总结一下：

- 支持 HTTP、HTTPS、FTP 和 FTPS 协议
- 可以使用多个镜像站点下载单个文件
- 轻量级，没有依赖并且使用非常少的 CPU 和内存

.. note::

   Axel下载是直接将所有数据直接写入到目标文件，在下载目录下有2个文件，一个是最终下载文件，一个是控制文件 ``XXX.st`` 。如果下载中断，只要使用相同下载命令，会根据 ``XXX.st`` 控制文件继续完成下载任务。

- Ubuntu发行版提供了alex软件包::

   sudo apt install alex

- 下载方法简单::

   axel https://xxxx/xxxx/file

一些有用的参数:

  - ``-o`` 保存成指定文件名
  - ``-s`` 限制下载速度，例如 ``-s 512000`` 就是限制下载速度 ``512KB/s``
  - ``-n`` 并发下载连接数，例如 ``-n 10`` 采用10个并发下载同一个文件
  - ``-q`` 安静模式，下载时不显示下载进度

参考
======

- `4 个 Linux 下最好的命令行下载管理器/加速器 <https://linux.cn/article-8124-1.html>`_
- `使用 Axel 命令行下载器/加速器加速下载 <https://linux.cn/article-8129-1.html>`_
