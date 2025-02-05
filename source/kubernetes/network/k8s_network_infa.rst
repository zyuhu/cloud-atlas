.. _k8s_network_infra:

==================
Kubernetes网络架构
==================

对于Kuernetes用户来说，最为复杂的基础架构恐怕就是网络了。主流的kubernetes容器网络解决方案:

- :ref:`flannel`
- :ref:`calico`
- :ref:`weave`
- :ref:`cilium`

.. note::

   Calico 和 Cilium 的复杂网络路由是通过 :ref:`bird` 实现的:

   - cilium `Using BIRD to run BGP <https://docs.cilium.io/en/v1.9/gettingstarted/bird/>`_

参考
=======

- `The Ultimate Guide To Using Calico, Flannel, Weave and Cilium <https://platform9.com/blog/the-ultimate-guide-to-using-calico-flannel-weave-and-cilium/>`_
