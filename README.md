# k3sup 🚀 (said 'ketchup')

<img src="docs/assets/k3sup.png" width="20%" alt="k3sup logo">

本项目fork自alexellis的[k3sup](https://github.com/alexellis/k3sup)

k3sup 是一个轻量级实用程序，可以在任何本地或远程虚拟机上使用 k3s 从零到启动 k8s 集群。
只需要 ssh 访问和 k3sup 二进制文件，就能获得 kubectl 访问权限。

## `脚本支持使用国内服务器进行下载，解决部分地区网络环境异常导致无法github，安装失败的问题。`


## 使用说明 ✅

该工具 k3sup 是一个客户端应用程序，您可以在自己的计算机上运行。它使用 SSH 连接到远程服务器，并在当前机器的磁盘上创建一个本地 KUBECONFIG 文件。
MacOS，Windows和Linux（包括ARM）客户端均可使用该工具。

## 先决条件

某些 Linux 主机需配置允许 sudo 运行而无需重复密码：


```bash
# sudo visudo

# Then add to the bottom of the file
# replace "alex" with your username i.e. "ubuntu"
alex ALL=(ALL) NOPASSWD: ALL
```

在大多数情况下，Ubuntu 和他的发行版云映像不需要此步骤。

如果只需部署在本机，则可以使用shell运行 k3sup install --local 进行本地安装，不使用 SSH。

配置运行兼容操作系统（如 Ubuntu、Debian、Raspbian 或其他操作系统）的新机器。需确保以将客户机的 SSH 密钥复制到了目标部署机器。

> 例： 可以使用 ssh-copy-id user@IP 将 SSH 密钥复制到远程虚拟机。

### 👑 使用 `k3sup` 部署 Kubernetes 

* 运行 `k3sup`:

```sh
export IP=192.168.0.1
k3sup install --ip $IP --user ubuntu

# Or use a hostname and SSH key for EC2
export HOST="ec2-3-250-131-77.eu-west-1.compute.amazonaws.com"
k3sup install --host $HOST --user ubuntu \
  --ssh-key $HOME/ec2-key.pem
```

`install` 的子选项:

* `--cluster` - 使用嵌入式 etcd（嵌入式 HA）以集群模式启动此服务器
* `--skip-install` - 如果已经安装了 K3s，只需运行此命令即可获取 kubeconfig
* `--ssh-key` - 为远程登录的 SSH 密钥指定特定路径
* `--local` - 在不使用 ssh 的情况下执行本地安装
* `--local-path` - 默认值为 ./kubeconfig - 设置要保存群集的文件 kubeconfig 。默认情况下，此文件将被覆盖。
* `--merge` - 将配置合并到现有文件中而不是覆盖（例如，要将配置添加到默认的kubectl配置中，请使用 --local-path ~/.kube/config --merge ）。
* `--net-switch` - 默认使用中国大陆服务器(rancher-mirror.rancher.cn)进行下载，设置为false则使用github仓库。
* `--context` -  默认为 default - 设置 kubeconfig 上下文的名称。
* `--ssh-port` - 默认值为 22 ，可以指定一个备用端口， 如：2222 
* `--no-extras` - 禁用“ServiceLB”和“Traefik”
* `--k3s-extra-args` - 可选的额外参数传递给 k3s 安装程序，用引号括起来，即 --k3s-extra-args '--disable traefik' 或者 --k3s-extra-args '--docker' 。如果有多个参数，请在单引号内组合 --k3s-extra-args '--disable traefik --docker' 。
* `--k3s-version` - 设置K3s的特定版本。例 `v1.21.1`
* `--k3s-channel` - 根据发布通道设置特定版本的K3S，例 `stable`
- `--ipsec` - 强制使用 k3s 的可选额外参数：  `--flannel-backend` option: `ipsec`
* `--print-command` - 打印出命令，通过SSH发送到远程计算机
* `--datastore` - 用于将 SQL 连接字符串传递给 k3s --datastore-endpoint 的标志。必须使用 k3s 要求的格式。可参考[文档](https://rancher.com/docs/k3s/latest/en/installation/ha/).

通过运行 `k3sup install --help` 查看更多安装选项。 

* 安装后测试:

```bash
export KUBECONFIG=`pwd`/kubeconfig
kubectl get node
```

