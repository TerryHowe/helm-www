---
title: "Helm Get Metadata"
---

## helm get metadata

该命令获取指定版本的元数据

```
helm get metadata RELEASE_NAME [flags]
```

### 可选项

```
  -h, --help            metadata 的帮助命令
  -o, --output format   以指定格式打印输出。允许的值：table, json, yaml（默认为 table）
      --revision int    指定发布版本
```

### 从父命令继承的选项

```
      --burst-limit int                 客户端限流配置（默认值100）
      --debug                           启用详细输出
      --kube-apiserver string           Kubernetes API 服务器的地址和端口
      --kube-as-group stringArray       为操作伪装用户组，这个参数可以重复设置指定多个组
      --kube-as-user string             为操作伪装的用户名
      --kube-ca-file string             用于连接 Kubernetes API 服务器的证书颁发机构文件
      --kube-context string             要使用的 kubeconfig 上下文名称
      --kube-insecure-skip-tls-verify   如果为 true，将不会检查 Kubernetes API 服务器证书的有效性。这会使你的 HTTPS 连接不安全
      --kube-tls-server-name string     用于验证 Kubernetes API 服务器证书的服务器名称。如果未提供，则使用连接服务器的主机名
      --kube-token string               用于身份验证的持有者令牌
      --kubeconfig string               kubeconfig 文件的路径
  -n, --namespace string                此请求的命名空间范围
      --qps float32                     与 Kubernetes API 通信时使用的每秒查询数，不包括突发流量
      --registry-config string          注册表配置文件的路径（默认 "~/.config/helm/registry/config.json"）
      --repository-cache string         包含缓存的仓库索引的目录路径（默认 "~/.cache/helm/repository"）
      --repository-config string        包含仓库名称和 URL 的文件路径（默认 "~/.config/helm/repositories.yaml"）
```

### 请参阅

* [helm get](helm_get.md)	 - 下载指定版本的扩展信息

###### spf13/cobra 于 11-Sep-2025 自动生成
