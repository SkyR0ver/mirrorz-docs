## 自动选择软件源镜像

[mirrorz-302](https://github.com/mirrorz-org/mirrorz-302) 提供 mirrorlist 接口，会根据你的地理位置、网络、ISP 等条件返回按可用性排序的镜像列表，客户端到列表中排在最前面的镜像拉取软件包，当排在前面的镜像不可用时还会自动回退到后续镜像。相比手动配置单一镜像地址，这种方式能自动为你选择网络条件最佳的镜像。

该接口需要 APT 1.6 及以上版本（即 Ubuntu 18.04 及以上）。

默认启用“安全更新使用官方索引”，在安全更新的 mirrorlist URL 中添加 `?official_index=1`，优先从官方 ports 源获取索引，再按镜像列表的顺序下载软件包；如果镜像站尚未同步所需软件包，会回退到官方源。官方索引不可用时，APT 也会尝试镜像站的索引。没有可用镜像时，列表仍保留官方源。关闭此选项只会移除该参数，索引和软件包都由 mirrorlist 中的镜像站提供。

Ubuntu 的安全更新需保持为独立条目，仅为 `-security` 条目的 URL 添加此参数，其他条目保持不变。

### 传统格式（`/etc/apt/sources.list`）

```{ztmpl lang="properties" input="release src proposed official_index" path="/etc/apt/sources.list"}
# 默认注释了源码镜像以提高 apt update 速度，如有需要可自行取消注释
deb mirror+{{scheme}}://mirrors.cernet.edu.cn/api/apt/mirrorlist/ubuntu-ports {{release}} main restricted universe multiverse
{{src}}deb-src mirror+{{scheme}}://mirrors.cernet.edu.cn/api/apt/mirrorlist/ubuntu-ports {{release}} main restricted universe multiverse
deb mirror+{{scheme}}://mirrors.cernet.edu.cn/api/apt/mirrorlist/ubuntu-ports {{release}}-updates main restricted universe multiverse
{{src}}deb-src mirror+{{scheme}}://mirrors.cernet.edu.cn/api/apt/mirrorlist/ubuntu-ports {{release}}-updates main restricted universe multiverse
deb mirror+{{scheme}}://mirrors.cernet.edu.cn/api/apt/mirrorlist/ubuntu-ports {{release}}-backports main restricted universe multiverse
{{src}}deb-src mirror+{{scheme}}://mirrors.cernet.edu.cn/api/apt/mirrorlist/ubuntu-ports {{release}}-backports main restricted universe multiverse

# 安全更新软件源
deb mirror+{{scheme}}://mirrors.cernet.edu.cn/api/apt/mirrorlist/ubuntu-ports{{#official_index}}?official_index=1{{/official_index}} {{release}}-security main restricted universe multiverse
{{src}}deb-src mirror+{{scheme}}://mirrors.cernet.edu.cn/api/apt/mirrorlist/ubuntu-ports{{#official_index}}?official_index=1{{/official_index}} {{release}}-security main restricted universe multiverse

# 预发布软件源，不建议启用
{{proposed}}deb mirror+{{scheme}}://mirrors.cernet.edu.cn/api/apt/mirrorlist/ubuntu-ports {{release}}-proposed main restricted universe multiverse
{{proposed}}{{src}}deb-src mirror+{{scheme}}://mirrors.cernet.edu.cn/api/apt/mirrorlist/ubuntu-ports {{release}}-proposed main restricted universe multiverse
```

### DEB822 格式（`/etc/apt/sources.list.d/ubuntu.sources`）

```{ztmpl lang="yaml" input="release_deb822 src proposed official_index" path="/etc/apt/sources.list.d/ubuntu.sources"}
Types: deb
URIs: mirror+{{scheme}}://mirrors.cernet.edu.cn/api/apt/mirrorlist/ubuntu-ports
Suites: {{release_deb822}} {{release_deb822}}-updates {{release_deb822}}-backports
Components: main restricted universe multiverse
Signed-By: /usr/share/keyrings/ubuntu-archive-keyring.gpg

# 默认注释了源码镜像以提高 apt update 速度，如有需要可自行取消注释
{{src}}Types: deb-src
{{src}}URIs: mirror+{{scheme}}://mirrors.cernet.edu.cn/api/apt/mirrorlist/ubuntu-ports
{{src}}Suites: {{release_deb822}} {{release_deb822}}-updates {{release_deb822}}-backports
{{src}}Components: main restricted universe multiverse
{{src}}Signed-By: /usr/share/keyrings/ubuntu-archive-keyring.gpg

# 安全更新软件源
Types: deb
URIs: mirror+{{scheme}}://mirrors.cernet.edu.cn/api/apt/mirrorlist/ubuntu-ports{{#official_index}}?official_index=1{{/official_index}}
Suites: {{release_deb822}}-security
Components: main restricted universe multiverse
Signed-By: /usr/share/keyrings/ubuntu-archive-keyring.gpg

{{src}}Types: deb-src
{{src}}URIs: mirror+{{scheme}}://mirrors.cernet.edu.cn/api/apt/mirrorlist/ubuntu-ports{{#official_index}}?official_index=1{{/official_index}}
{{src}}Suites: {{release_deb822}}-security
{{src}}Components: main restricted universe multiverse
{{src}}Signed-By: /usr/share/keyrings/ubuntu-archive-keyring.gpg

# 预发布软件源，不建议启用

{{proposed}}Types: deb
{{proposed}}URIs: mirror+{{scheme}}://mirrors.cernet.edu.cn/api/apt/mirrorlist/ubuntu-ports
{{proposed}}Suites: {{release_deb822}}-proposed
{{proposed}}Components: main restricted universe multiverse
{{proposed}}Signed-By: /usr/share/keyrings/ubuntu-archive-keyring.gpg

{{proposed}}{{src}}Types: deb-src
{{proposed}}{{src}}URIs: mirror+{{scheme}}://mirrors.cernet.edu.cn/api/apt/mirrorlist/ubuntu-ports
{{proposed}}{{src}}Suites: {{release_deb822}}-proposed
{{proposed}}{{src}}Components: main restricted universe multiverse
{{proposed}}{{src}}Signed-By: /usr/share/keyrings/ubuntu-archive-keyring.gpg
```
