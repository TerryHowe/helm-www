---
title: "故障排除"
weight: 4
---

## 故障排除

### 我收到了关于 "Unable to get an update from the "stable" chart repository" 的警告

运行 `helm repo list`。如果显示你的 `stable` 仓库指向 `storage.googleapis.com` URL，你需要更新该仓库。
2020年11月13日，Helm Charts 仓库在经过一年的弃用期后[停止支持](https://github.com/helm/charts#deprecation-timeline)。
归档文件可在 `https://charts.helm.sh/stable` 获取，但不再接收更新。

您可以运行以下命令来修复您的仓库：

```console
$ helm repo add stable https://charts.helm.sh/stable --force-update  
```

`incubator` 仓库同样如此，归档文件可在 https://charts.helm.sh/incubator 获取。
您可以运行以下命令来修复它：

```console
$ helm repo add incubator https://charts.helm.sh/incubator --force-update  
```

### 我收到了警告 'WARNING: "kubernetes-charts.storage.googleapis.com" is deprecated for "stable" and will be deleted Nov. 13, 2020.'

旧的Google helm chart 仓库已被新的 Helm chart 仓库替换。

运行以下命令来永久修复此问题：

```console
$ helm repo add stable https://charts.helm.sh/stable --force-update  
```

如果您对 `incubator` 遇到类似的错误，运行此命令：

```console
$ helm repo add incubator https://charts.helm.sh/incubator --force-update  
```

### 当我添加 Helm 仓库时，收到错误 'Error: Repo "https://kubernetes-charts.storage.googleapis.com" is no longer available'

Helm Chart 仓库在[一年的弃用期](https://github.com/helm/charts#deprecation-timeline)后不再受支持。
这些仓库的归档文件可在 `https://charts.helm.sh/stable` 和 `https://charts.helm.sh/incubator` 获取，
但它们不再接收更新。除非您指定 `--use-deprecated-repos`，否则 `helm repo add` 命令不允许您添加旧的 URL。

### 在 GKE (Google Container Engine) 我遇到了 "No SSH tunnels currently open"

```
Error: Error forwarding ports: error upgrading connection: No SSH tunnels currently open. Were the targets able to accept an ssh-key for user "gke-[redacted]"?
```

另一个错误消息的形式：

```
Unable to connect to the server: x509: certificate signed by unknown authority
```

这个问题是说您本地的Kubernetes 配置文件需要有正确的证书。

当你在GKE上创建集群时，它会为您提供一个证书，包括SSL证书和证书颁发机构。
这些需要放在Kubernetes配置文件中（默认位置: `~/.kube/config`），保证 `kubectl` 和 `helm` 可以访问他们。

### 从Helm 2迁移后， `helm list` 仅显示部分（或者不显示）我的发布版本

您很可能忽略了一个事实：Helm 3 现在使用集群的命名空间来确定版本范围。 这意味着所有涉及版本的命令您都必须：

* 在活动的kubernetes上下文中需要依赖当前的命名空间 (如 `kubectl config view --minify` 命令所述)，
* 使用 `--namespace`/`-n` 参数指定正确命名空间，或者
* 对于 `helm list` 命令，指定 `--all-namespaces`/`-A` 参数

这适用于 `helm ls`、 `helm uninstall` 以及其他所有涉及版本的 `helm` 命令。


### 为什么在macOS上`/etc/.mdns_debug`文件可以访问？

我们了解到macOS上的一个案例是Helm会试图访问`/etc/.mdns_debug`文件。
如果文件存在，Helm会在文件句柄执行的时候保持打开状态。

这是因为macOS的 MDNS 库。它尝试去加载这个文件读取debug设置（如果已经启用）。
这个文件句柄可能不会保持打开，且这个问题已经报告给了苹果。 然而这种行为是macOS导致的，并不是Helm。

如果你不想让Helm加载这个文件，你可以将Helm编译成一个静态库而不使用主机网络堆栈。
这样做会导致Helm的二进制文件大小膨胀，但是会阻止这个文件打开。

这个问题最初被标记为潜在的安全问题。但后来已经确定，这种行为不存在任何缺陷或漏洞。

### helm 仓库使用时添加仓库失败

在Helm 3.3.1及之前版本，`helm repo add <reponame> <url>`在你添加已经存在的仓库时不会输入内容。
如果仓库已经存在，`--no-update` 参数会报错。

在Helm3.3.2及之后版本，试图添加一个已存在的仓库时会报以下错误：

`Error: repository name (reponame) already exists, please specify a different name`

现在这个默认行为是相反的。`--no-update` 现在被忽略。当您想替换（覆盖）已有仓库时，您可以使用 `--force-update`。

这是由于一个安全修复做出的重大更改，详情见[Helm 3.3.2 release notes](https://github.com/helm/helm/releases/tag/v3.3.2)。

### 开启 Kubernetes 客户端日志

调试Kubernetes客户端打印日志时，可以使用[klog](https://pkg.go.dev/k8s.io/klog) 参数。使用`-v`可以设置日志级别应用于大多数场景。

例如：

```
helm list -v 6
```

### Tiller停止安装且无法访问

Helm 发布之前可以从 <https://storage.googleapis.com/kubernetes-helm/> 获取。如["Announcing
get.helm.sh"](https://helm.sh/blog/get-helm-sh/)中所述，官方地址2019年6月已变更。
[GitHub Container Registry](https://github.com/orgs/helm/packages/container/package/tiller)
可以获取所有旧的Tiller镜像。

如果你想从原来的存储位置下载Helm旧版本，你会发现找不到了：

```
<Error>
    <Code>AccessDenied</Code>
    <Message>Access denied.</Message>
    <Details>Anonymous caller does not have storage.objects.get access to the Google Cloud Storage object.</Details>
</Error>
```

[原有Tiller镜像地址](https://gcr.io/kubernetes-helm/tiller) 会在2021年8月开始删除镜像。我们已经增加了可用地址在
[GitHub Container Registry](https://github.com/orgs/helm/packages/container/package/tiller)。
比如下载v2.17.0，用

`https://get.helm.sh/helm-v2.17.0-linux-amd64.tar.gz` 替换

`https://storage.googleapis.com/kubernetes-helm/helm-v2.17.0-linux-amd64.tar.gz`

使用Helm v2.17.0初始化：

`helm init —upgrade`

或者如果需要另一个版本，使用 --tiller-image 替换默认位置安装特定的Helm v2 版本：

`helm init --tiller-image ghcr.io/helm/tiller:v2.16.9`

**注意** Helm维护人员建议迁移到当前支持的Helm版本。Helm v2.17.0 是Helm 2的最终版本；Helm
v2自2020年11月起便不再支持，具体细节请查看[Helm 2 和 Charts Project 已不再支持](https://helm.sh/blog/helm-2-becomes-unsupported/)。
自那以后，Helm已经被发现很多的CVE，这些漏洞已经在Helm v3修复，但不会再给Helm v2打补丁。现在查看
[Helm当前已发布的建议列表](https://github.com/helm/helm/security/advisories?state=published)并计划
[迁移到 Helm v3](https://helm.sh/docs/topics/v2_v3_migration/#helm)吧。
