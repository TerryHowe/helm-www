---
title: "卸载"
weight: 3
---

## 卸载

### 我想删除我本地Helm. 全部文件在什么位置？

连同 `helm` 二进制文件一起，Helm将文件存储在以下位置：

- $XDG_CACHE_HOME
- $XDG_CONFIG_HOME
- $XDG_DATA_HOME

下面这个表格按照操作系统给出了对应的默认文件夹位置：

| Operating System | Cache Path                  | Configuration Path               | Data Path                 |
|------------------|-----------------------------|----------------------------------|---------------------------|
| Linux            | `$HOME/.cache/helm`         | `$HOME/.config/helm`             | `$HOME/.local/share/helm` |
| macOS            | `$HOME/Library/Caches/helm` | `$HOME/Library/Preferences/helm` | `$HOME/Library/helm`      |
| Windows          | `%TEMP%\helm`               | `%APPDATA%\helm`                 | `%APPDATA%\helm`          |

