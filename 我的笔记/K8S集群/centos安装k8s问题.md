yum安装提示“没有可用软件包”

https://blog.csdn.net/shuiyuetianwy/article/details/86070213

centos安装docker显示 No package docker-ce available
https://blog.csdn.net/qq_25760623/article/details/88657491





```
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://64uob1jx.mirror.aliyuncs.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```



# [VMware安装Centos7超详细过程（图文）](https://www.cnblogs.com/jpfss/p/10911463.html)







# 从零开始搭建Kubernetes集群（四、搭建K8S Dashboard）

 https://www.jianshu.com/p/6f42ac331d8a 





# [Kubernetes 环境搭建](https://www.cnblogs.com/cjsblog/p/11877014.html)





# 使用Kubernetes和Docker

 https://www.jianshu.com/p/a7616ef950a4 





# [Centos7.6部署k8s v1.16.4高可用集群(主备模式)](https://www.kubernetes.org.cn/6632.html)





 https://www.kubernetes.org.cn/doc-11 

 Kubernetes中文社区 



 http://docs.kubernetes.org.cn/774.html#i 



# [Kubeadm安装Kubernetes环境](https://www.cnblogs.com/ericnie/p/7749588.html)