Ubuntu 旧版本的软件源、镜像

该仓库包含了所有 Ubuntu 以前发布过的软件仓库、镜像 ISO，但不包含 Ubuntu 衍生版的 ISO。

### 软件源

Ubuntu 18.04 LTS 及以上版本默认支持 HTTPS 源。Ubuntu 18.04 LTS 以前的系统如需使用 HTTPS 源需要安装 `apt-transport-https` 包。

如果遇到无法拉取 HTTPS 源的情况，请先使用 HTTP 源并安装通用 CA 证书：

```{ztmpl lang="bash"}
{{sudo}}apt install ca-certificates
```

提醒：旧版 Ubuntu 需要对应更新根证书，否则可能无法访问大多数镜像站。详情可参见 TUNA issues 上的[相应讨论](https://github.com/tuna/issues/issues/1342#issuecomment-931412628)。

```{ztmpl input="release src proposed" path="/etc/apt/sources.list"}
# 默认注释了源码镜像以提高 apt update 速度，如有需要可自行取消注释
deb {{endpoint}}/ubuntu/ {{release}} main restricted universe multiverse
{{src}}deb-src {{endpoint}}/ubuntu/ {{release}} main restricted universe multiverse
deb {{endpoint}}/ubuntu/ {{release}}-updates main restricted universe multiverse
{{src}}deb-src {{endpoint}}/ubuntu/ {{release}}-updates main restricted universe multiverse
deb {{endpoint}}/ubuntu/ {{release}}-backports main restricted universe multiverse
{{src}}deb-src {{endpoint}}/ubuntu/ {{release}}-backports main restricted universe multiverse
deb {{endpoint}}/ubuntu/ {{release}}-security main restricted universe multiverse
{{src}}deb-src {{endpoint}}/ubuntu/ {{release}}-security main restricted universe multiverse

# 预发布软件源，不建议启用
{{proposed}}deb {{endpoint}}/ubuntu/ {{release}}-proposed main restricted universe multiverse
{{proposed}}{{src}}deb-src {{endpoint}}/ubuntu/ {{release}}-proposed main restricted universe multiverse
```

更多版本代号可参考 https://wiki.ubuntu.com/Releases

### 镜像

请前往 {ztmpl}`{{endpoint}}/releases/` 下载。

非 AMD64(x86_64), Intel x86 架构的镜像请前往 {ztmpl}`{{endpoint}}/releases/ports/releases/` 下载。
