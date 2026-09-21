## 使用方法

在使用前，请确保你的**系统、架构**都在[受支持的 ELTS 列表](https://www.freexian.com/lts/extended/)中。

Debian 10（Buster）及以上版本默认支持 HTTPS 源。Debian 10（Buster）以前的系统如需使用 HTTPS 源需要安装 `apt-transport-https` 包。

如果遇到无法拉取 HTTPS 源的情况，请先使用 HTTP 源并安装通用 CA 证书：

```{ztmpl lang="bash"}
{{sudo}}apt install ca-certificates
```

提醒：旧版 Debian 需要对应更新根证书，否则可能无法访问大多数镜像站。详情可参见 TUNA issues 上的[相应讨论](https://github.com/tuna/issues/issues/1342#issuecomment-931412628)。

首先安装 Freexian 的 APT 源密钥：

```{ztmpl lang="bash"}
wget https://deb.freexian.com/extended-lts/archive-key.gpg -O /tmp/elts-archive-key.gpg
mv /tmp/elts-archive-key.gpg /etc/apt/trusted.gpg.d/freexian-archive-extended-lts.gpg
```

此后，**删除** `/etc/apt/sources.list` 中的所有的 Debian 源（可能包括 `foo`, `foo-updates`, `foo-backports`, `foo-security` 等多个来源，其中 `foo` 为版本代号），替换为：

```{ztmpl input="release nf"}
deb {{endpoint}} {{release}} main contrib{{#nf}}{{nonfree}}{{/nf}}
```
