## 自动选择软件源镜像

[mirrorz-302](https://github.com/mirrorz-org/mirrorz-302) 提供 mirrorlist 接口，会根据你的地理位置、网络、ISP 等条件返回按可用性排序的镜像列表，客户端到列表中排在最前面的镜像拉取软件包，当排在前面的镜像不可用时还会自动回退到后续镜像。相比手动配置单一镜像地址，这种方式能自动为你选择网络条件最佳的镜像。

该接口需要 APT 1.6 及以上版本（即 Ubuntu 18.04 及以上）。

### 传统格式（`/etc/apt/sources.list`）

```{ztmpl lang="properties" input="release src proposed mirror_security" path="/etc/apt/sources.list"}
# 默认注释了源码镜像以提高 apt update 速度，如有需要可自行取消注释
deb mirror+{{scheme}}://mirrors.cernet.edu.cn/api/apt/mirrorlist/ubuntu// {{release}} main restricted universe multiverse
{{src}}deb-src mirror+{{scheme}}://mirrors.cernet.edu.cn/api/apt/mirrorlist/ubuntu// {{release}} main restricted universe multiverse
deb mirror+{{scheme}}://mirrors.cernet.edu.cn/api/apt/mirrorlist/ubuntu// {{release}}-updates main restricted universe multiverse
{{src}}deb-src mirror+{{scheme}}://mirrors.cernet.edu.cn/api/apt/mirrorlist/ubuntu// {{release}}-updates main restricted universe multiverse
deb mirror+{{scheme}}://mirrors.cernet.edu.cn/api/apt/mirrorlist/ubuntu// {{release}}-backports main restricted universe multiverse
{{src}}deb-src mirror+{{scheme}}://mirrors.cernet.edu.cn/api/apt/mirrorlist/ubuntu// {{release}}-backports main restricted universe multiverse

{{#mirror_security}}
# 以下安全更新软件源为镜像站配置
deb mirror+{{scheme}}://mirrors.cernet.edu.cn/api/apt/mirrorlist/ubuntu// {{release}}-security main restricted universe multiverse
{{src}}deb-src mirror+{{scheme}}://mirrors.cernet.edu.cn/api/apt/mirrorlist/ubuntu// {{release}}-security main restricted universe multiverse
{{/mirror_security}}
{{^mirror_security}}
# 以下安全更新软件源为官方源配置
deb http://security.ubuntu.com/ubuntu/ {{release}}-security main restricted universe multiverse
{{src}}deb-src http://security.ubuntu.com/ubuntu/ {{release}}-security main restricted universe multiverse
{{/mirror_security}}

# 预发布软件源，不建议启用
{{proposed}}deb mirror+{{scheme}}://mirrors.cernet.edu.cn/api/apt/mirrorlist/ubuntu// {{release}}-proposed main restricted universe multiverse
{{proposed}}{{src}}deb-src mirror+{{scheme}}://mirrors.cernet.edu.cn/api/apt/mirrorlist/ubuntu// {{release}}-proposed main restricted universe multiverse
```

### DEB822 格式（`/etc/apt/sources.list.d/ubuntu.sources`）

```{ztmpl lang="yaml" input="release_deb822 src proposed mirror_security" path="/etc/apt/sources.list.d/ubuntu.sources"}
Types: deb
URIs: mirror+{{scheme}}://mirrors.cernet.edu.cn/api/apt/mirrorlist/ubuntu/
Suites: {{release_deb822}} {{release_deb822}}-updates {{release_deb822}}-backports
Components: main restricted universe multiverse
Signed-By: /usr/share/keyrings/ubuntu-archive-keyring.gpg

# 默认注释了源码镜像以提高 apt update 速度，如有需要可自行取消注释
{{src}}Types: deb-src
{{src}}URIs: mirror+{{scheme}}://mirrors.cernet.edu.cn/api/apt/mirrorlist/ubuntu/
{{src}}Suites: {{release_deb822}} {{release_deb822}}-updates {{release_deb822}}-backports
{{src}}Components: main restricted universe multiverse
{{src}}Signed-By: /usr/share/keyrings/ubuntu-archive-keyring.gpg

{{#mirror_security}}
# 以下安全更新软件源为镜像站配置
Types: deb
URIs: mirror+{{scheme}}://mirrors.cernet.edu.cn/api/apt/mirrorlist/ubuntu/
Suites: {{release_deb822}}-security
Components: main restricted universe multiverse
Signed-By: /usr/share/keyrings/ubuntu-archive-keyring.gpg

{{src}}Types: deb-src
{{src}}URIs: mirror+{{scheme}}://mirrors.cernet.edu.cn/api/apt/mirrorlist/ubuntu/
{{src}}Suites: {{release_deb822}}-security
{{src}}Components: main restricted universe multiverse
{{src}}Signed-By: /usr/share/keyrings/ubuntu-archive-keyring.gpg
{{/mirror_security}}
{{^mirror_security}}
# 以下安全更新软件源为官方源配置
Types: deb
URIs: http://security.ubuntu.com/ubuntu/
Suites: {{release_deb822}}-security
Components: main restricted universe multiverse
Signed-By: /usr/share/keyrings/ubuntu-archive-keyring.gpg

{{src}}Types: deb-src
{{src}}URIs: http://security.ubuntu.com/ubuntu/
{{src}}Suites: {{release_deb822}}-security
{{src}}Components: main restricted universe multiverse
{{src}}Signed-By: /usr/share/keyrings/ubuntu-archive-keyring.gpg
{{/mirror_security}}

# 预发布软件源，不建议启用

{{proposed}}Types: deb
{{proposed}}URIs: mirror+{{scheme}}://mirrors.cernet.edu.cn/api/apt/mirrorlist/ubuntu/
{{proposed}}Suites: {{release_deb822}}-proposed
{{proposed}}Components: main restricted universe multiverse
{{proposed}}Signed-By: /usr/share/keyrings/ubuntu-archive-keyring.gpg

{{proposed}}{{src}}Types: deb-src
{{proposed}}{{src}}URIs: mirror+{{scheme}}://mirrors.cernet.edu.cn/api/apt/mirrorlist/ubuntu/
{{proposed}}{{src}}Suites: {{release_deb822}}-proposed
{{proposed}}{{src}}Components: main restricted universe multiverse
{{proposed}}{{src}}Signed-By: /usr/share/keyrings/ubuntu-archive-keyring.gpg
```
