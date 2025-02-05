.. _kubernetes_deployment:

======================
Kubernetes部署
======================

虽然 :ref:`install_run_minikube` 让我们能够轻而易举获得一个可用的单机测试环境，但是实际上生产环境部署需要实现极其复杂的高可用、高性能的容灾系统。本章节将模拟生产环境的部署交付工作，并为下一阶段的冗灾和故障演练打下基础。

最新的实践采用 :ref:`z-k8s_env` 准备的KVM虚拟化环境，实践中引用的服务器列表请参考 :ref:`priv_cloud_infra`

.. toctree::
   :maxdepth: 1

   container_runtimes.rst
   bootstrap_kubernetes_single/index
   bootstrap_kubernetes_ha/index
   stateless_application/index
   operator/index
   etcd/index
   deploy_pod/index
   nginx_ingress.rst
   docker_registry.rst
   helm.rst
   draft.rst
   kustomize.rst
   kuberbuilder.rst
   kubebox/index
   k8s_hosts_in_mbp/index
