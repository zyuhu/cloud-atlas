.. _edge_cloud_infra:

======================
边缘云计算架构
======================

我陆续购买多多代 :ref:`raspberry_pi` 设备，在最初构建 :ref:`priv_cloud_infra` 时，我采用的还是完整的 :ref:`kubernetes_arm` 。不过，我也在思考如何能够尽可能少消耗树莓派的硬件资源来完成更多的计算。轻量级始终是ARM架构(资源有限的系统)的技术关键，类似 :ref:`akraino` ，我逐步想要在ARM架构上构建一个轻量级的边缘计算平台。

.. note::

   最近在部署 :ref:`k3s` 集群时候，偶然发现原来有人和我类似的思路，并且已经实践并分享了方案和经验，请参考 `rpi4cluster.com <https://rpi4cluster.com/>`_ ，方案和文档非常清晰完备。

   我准备借鉴并尝试不同的应用应用迭代，构建适合自己同时能够进一步完善的解决方案。

硬件环境
=========

我购买过多代树莓派产品以及 :ref:`jetson_nano` :

- :ref:`pi_1` - :strike:`构建边缘云计算(监控)` (考虑到组装树莓派集群需要小巧，类似 :ref:`turing_pi` ，所以我还是决定舍弃 :ref:`pi_1` ，改为采用一台低配置 :ref:`pi_4` 来实现管理和监控)
- :ref:`pi_3` - 构建边缘云计算(管控节点)
- :ref:`pi_4` - 构建边缘云计算(计算节点)

  - 其中一台 ``2G`` 内存配置的 :ref:`pi_4` 独立作为监控和管理服务器，提供这个 :ref:`edge_cloud` 的监控

    - 资源有限，并且要有一个 :ref:`prometheus` 体系之外的单独监控，所以该节点运行轻量级监控以及对外通知

    - 采用监控( :ref:`prometheus` 结合跟多网络管理平台 )

  - 在3台 :ref:`pi_4` 作为工作节点( ``worker`` )

    - 由于3个 :ref:`pi_4` 的其中一个只有 ``2G`` 内存，调度只分配监控服务 :ref:`prometheus` / :ref:`grafana` / :ref:`thanos` 来构建集群监控

  - 另外两台 ``8G`` 内存配置的 :ref:`pi_4` 加入 :ref:`k3s` 作为工作节点

    - ``8G`` 节点内存，部署 :ref:`jenkins` (集成在 :ref:`rancher` 中作为 pipeline)

- :ref:`jetson_nano` - 构建边缘云计算( :ref:`machine_learning` )
- :ref:`pi_400` - 作为管理和操作

我将 3个 :ref:`pi_4` 和 3 个 :ref:`pi_3` 堆叠起来，构建一个mini的树莓派集群:

.. figure:: ../../_static/arm/raspberry_pi/pi_cluster/edge_cloud_pi.jpg
   :scale: 60

ARM服务器分布
=============

.. csv-table:: ARM边缘计算主机分配
   :file: edge_cloud_infra/hosts.csv
   :widths: 20, 10, 10, 10, 20, 30
   :header-rows: 1

ARM架构的边缘计算采用了 ``192.168.7.x`` 作为网络IP段，和 :ref:`priv_cloud_infra` 的 ``192.168.6.x`` 隔离，中间采用 3层 :ref:`cisco` 路由

虽然也可以在树莓派上实现 :ref:`arm_kvm` ，但是考虑到边缘计算硬件性能有限，所以采用轻量级 :ref:`kubernetes` 实现 :ref:`k3s` 来构建mini集群，目标是实现:

- 任意调度计算资源实现服务的伸缩、高可用
- 构建边缘计算场景: 传感器数据采集、存储、传输，以及独立的AI计算，结合 :ref:`priv_cloud` 的强大算力，实现云计算的合理分布

网络互联
==============

模拟多机房互联:

- 使用 :ref:`thinkpad_x220` 构建VPN中心节点，实现多机房集中到中心节点连接
- 在每个集群上启动 :ref:`bird` 路由Daemon来维护动态路由，并结合 :ref:`k8s_network_infra` 实现不同集群路由
