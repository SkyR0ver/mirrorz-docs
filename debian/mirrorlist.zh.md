## 自动选择软件源镜像

[mirrorz-302](https://github.com/mirrorz-org/mirrorz-302) 提供 mirrorlist 接口，会根据你的地理位置、网络、ISP 等条件返回按可用性排序的镜像列表，客户端到列表中排在最前面的镜像拉取软件包，当排在前面的镜像不可用时还会自动回退到后续镜像。相比手动配置单一镜像地址，这种方式能自动为你选择网络条件最佳的镜像。

该接口需要 APT 1.6 及以上版本（即 Debian 10 buster 及以上）。

### 传统格式（`/etc/apt/sources.list`）

```{ztmpl lang="properties" input="release src nf mirror_security" path="/etc/apt/sources.list"}
# 默认注释了源码镜像以提高 apt update 速度，如有需要可自行取消注释
{{#sid}}
deb mirror+{{scheme}}://mirrors.cernet.edu.cn/api/apt/mirrorlist/debian/ sid main contrib{{#nf}}{{nonfree}}{{/nf}}
{{src}}deb-src mirror+{{scheme}}://mirrors.cernet.edu.cn/api/apt/mirrorlist/debian/ sid main contrib{{#nf}}{{nonfree}}{{/nf}}
{{/sid}}
{{^sid}}
deb mirror+{{scheme}}://mirrors.cernet.edu.cn/api/apt/mirrorlist/debian/ {{release}} main contrib{{#nf}}{{nonfree}}{{/nf}}
{{src}}deb-src mirror+{{scheme}}://mirrors.cernet.edu.cn/api/apt/mirrorlist/debian/ {{release}} main contrib{{#nf}}{{nonfree}}{{/nf}}

deb mirror+{{scheme}}://mirrors.cernet.edu.cn/api/apt/mirrorlist/debian/ {{release}}-updates main contrib{{#nf}}{{nonfree}}{{/nf}}
{{src}}deb-src mirror+{{scheme}}://mirrors.cernet.edu.cn/api/apt/mirrorlist/debian/ {{release}}-updates main contrib{{#nf}}{{nonfree}}{{/nf}}

deb mirror+{{scheme}}://mirrors.cernet.edu.cn/api/apt/mirrorlist/debian/ {{release}}-backports main contrib{{#nf}}{{nonfree}}{{/nf}}
{{src}}deb-src mirror+{{scheme}}://mirrors.cernet.edu.cn/api/apt/mirrorlist/debian/ {{release}}-backports main contrib{{#nf}}{{nonfree}}{{/nf}}

{{#mirror_security}}
# 以下安全更新软件源为镜像站配置
deb mirror+{{scheme}}://mirrors.cernet.edu.cn/api/apt/mirrorlist/debian-security {{release}}{{security}} main contrib{{#nf}}{{nonfree}}{{/nf}}
{{src}}deb-src mirror+{{scheme}}://mirrors.cernet.edu.cn/api/apt/mirrorlist/debian-security {{release}}{{security}} main contrib{{#nf}}{{nonfree}}{{/nf}}
{{/mirror_security}}
{{^mirror_security}}
# 以下安全更新软件源为官方源配置
deb {{scheme}}://security.debian.org/debian-security {{release}}{{security}} main contrib{{#nf}}{{nonfree}}{{/nf}}
{{src}}deb-src {{scheme}}://security.debian.org/debian-security {{release}}{{security}} main contrib{{#nf}}{{nonfree}}{{/nf}}
{{/mirror_security}}
{{/sid}}
```

### DEB822 格式（`/etc/apt/sources.list.d/debian.sources`）

```{ztmpl lang="yaml" input="release_deb822 src nf mirror_security" path="/etc/apt/sources.list.d/debian.sources"}
{{#sid}}
Types: deb
URIs: mirror+{{scheme}}://mirrors.cernet.edu.cn/api/apt/mirrorlist/debian
Suites: sid
Components: main contrib{{#nf}} non-free non-free-firmware{{/nf}}
Signed-By: /usr/share/keyrings/debian-archive-keyring.gpg

# 默认注释了源码镜像以提高 apt update 速度，如有需要可自行取消注释
{{src}}Types: deb-src
{{src}}URIs: mirror+{{scheme}}://mirrors.cernet.edu.cn/api/apt/mirrorlist/debian
{{src}}Suites: sid
{{src}}Components: main contrib{{#nf}} non-free non-free-firmware{{/nf}}
{{src}}Signed-By: /usr/share/keyrings/debian-archive-keyring.gpg
{{/sid}}
{{^sid}}
Types: deb
URIs: mirror+{{scheme}}://mirrors.cernet.edu.cn/api/apt/mirrorlist/debian
Suites: {{release_deb822}} {{release_deb822}}-updates {{release_deb822}}-backports
Components: main contrib{{#nf}} non-free non-free-firmware{{/nf}}
Signed-By: /usr/share/keyrings/debian-archive-keyring.gpg

# 默认注释了源码镜像以提高 apt update 速度，如有需要可自行取消注释
{{src}}Types: deb-src
{{src}}URIs: mirror+{{scheme}}://mirrors.cernet.edu.cn/api/apt/mirrorlist/debian
{{src}}Suites: {{release_deb822}} {{release_deb822}}-updates {{release_deb822}}-backports
{{src}}Components: main contrib{{#nf}} non-free non-free-firmware{{/nf}}
{{src}}Signed-By: /usr/share/keyrings/debian-archive-keyring.gpg

{{#mirror_security}}
# 以下安全更新软件源为镜像站配置
Types: deb
URIs: mirror+{{scheme}}://mirrors.cernet.edu.cn/api/apt/mirrorlist/debian-security
Suites: {{release_deb822}}-security
Components: main contrib{{#nf}} non-free non-free-firmware{{/nf}}
Signed-By: /usr/share/keyrings/debian-archive-keyring.gpg

{{src}}Types: deb-src
{{src}}URIs: mirror+{{scheme}}://mirrors.cernet.edu.cn/api/apt/mirrorlist/debian-security
{{src}}Suites: {{release_deb822}}-security
{{src}}Components: main contrib{{#nf}} non-free non-free-firmware{{/nf}}
{{src}}Signed-By: /usr/share/keyrings/debian-archive-keyring.gpg
{{/mirror_security}}
{{^mirror_security}}
# 以下安全更新软件源为官方源配置
Types: deb
URIs: {{scheme}}://security.debian.org/debian-security
Suites: {{release_deb822}}-security
Components: main contrib{{#nf}} non-free non-free-firmware{{/nf}}
Signed-By: /usr/share/keyrings/debian-archive-keyring.gpg

{{src}}Types: deb-src
{{src}}URIs: {{scheme}}://security.debian.org/debian-security
{{src}}Suites: {{release_deb822}}-security
{{src}}Components: main contrib{{#nf}} non-free non-free-firmware{{/nf}}
{{src}}Signed-By: /usr/share/keyrings/debian-archive-keyring.gpg
{{/mirror_security}}
{{/sid}}
```
