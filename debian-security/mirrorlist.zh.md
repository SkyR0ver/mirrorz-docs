## 自动选择软件源镜像

[mirrorz-302](https://github.com/mirrorz-org/mirrorz-302) 提供 mirrorlist 接口，会根据你的地理位置、网络、ISP 等条件返回按可用性排序的镜像列表，客户端到列表中排在最前面的镜像拉取软件包，当排在前面的镜像不可用时还会自动回退到后续镜像。相比手动配置单一镜像地址，这种方式能自动为你选择网络条件最佳的镜像。

该接口需要 APT 1.6 及以上版本（即 Debian 10 buster 及以上）。

默认启用“安全更新使用官方索引”，在安全更新的 mirrorlist URL 中添加 `?official_index=1`，优先从官方源获取索引，再按镜像列表的顺序下载软件包；如果镜像站尚未同步所需软件包，会回退到官方源。官方索引不可用时，APT 也会尝试镜像站的索引。没有可用镜像时，列表仍保留官方源。关闭此选项只会移除该参数，索引和软件包都由 mirrorlist 中的镜像站提供。

### 传统格式（`/etc/apt/sources.list`）

```{ztmpl lang="properties" input="release src nf official_index"}
# 默认注释了源码镜像以提高 apt update 速度，如有需要可自行取消注释
deb mirror+{{scheme}}://mirrors.cernet.edu.cn/api/apt/mirrorlist/debian-security{{#official_index}}?official_index=1{{/official_index}} {{release}}{{security}} main contrib{{#nf}}{{nonfree}}{{/nf}}
{{src}}deb-src mirror+{{scheme}}://mirrors.cernet.edu.cn/api/apt/mirrorlist/debian-security{{#official_index}}?official_index=1{{/official_index}} {{release}}{{security}} main contrib{{#nf}}{{nonfree}}{{/nf}}
```

### DEB822 格式（`/etc/apt/sources.list.d/debian.sources`）

```{ztmpl lang="yaml" input="release_deb822 src nf official_index"}
Types: deb
URIs: mirror+{{scheme}}://mirrors.cernet.edu.cn/api/apt/mirrorlist/debian-security{{#official_index}}?official_index=1{{/official_index}}
Suites: {{release_deb822}}-security
Components: main contrib{{#nf}} non-free non-free-firmware{{/nf}}
Signed-By: /usr/share/keyrings/debian-archive-keyring.gpg

{{src}}Types: deb-src
{{src}}URIs: mirror+{{scheme}}://mirrors.cernet.edu.cn/api/apt/mirrorlist/debian-security{{#official_index}}?official_index=1{{/official_index}}
{{src}}Suites: {{release_deb822}}-security
{{src}}Components: main contrib{{#nf}} non-free non-free-firmware{{/nf}}
{{src}}Signed-By: /usr/share/keyrings/debian-archive-keyring.gpg
```
