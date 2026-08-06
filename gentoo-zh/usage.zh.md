## Distfiles 配置

在 `/etc/portage/make.conf` 中追加：

```{ztmpl path="/etc/portage/make.conf" append="true"}
GENTOO_MIRRORS="${GENTOO_MIRRORS} {{endpoint}}"
```

相应镜像只提供 gentoo-zh overlay 相关的 Distfiles，因此只应追加到现有配置。`::gentoo` 的 Distfiles 配置见 [Gentoo 帮助](../gentoo/)。

## 二进制包

[gentoo-zh binhost](https://github.com/gentoo-zh/binhost) 目前提供已签名的 amd64 二进制包，分为 stable 与 unstable 两个频道：

- stable 使用 Gentoo 主树的稳定软件包，只对 `::gentoo-zh` 接受 `~amd64`。
- unstable 全局使用 `~amd64`。

二进制包还附带 gentoo-zh 软件包运行期依赖所需的部分 `::gentoo` 软件包，不替代 Gentoo 官方 binhost。

### 导入签名公钥

```{ztmpl lang="bash"}
{{sudo}}emerge sec-keys/openpgp-keys-gentoozh
{{sudo}}getuto
{{sudo}}gpg --homedir /etc/portage/gnupg --import /usr/share/openpgp-keys/gentoozh.asc
{{sudo}}gpg --homedir /etc/portage/gnupg --batch --yes --pinentry-mode loopback \
    --passphrase-file /etc/portage/gnupg/pass \
    --lsign-key 6A0726AF1476A2F382C6AC6638A0234EC16AD42E
{{sudo}}gpg --homedir /etc/portage/gnupg --check-trustdb
```

`getuto` 用于建立 `/etc/portage/gnupg` 和 Portage Local Trust Key，必须在导入公钥前执行。验签以 `portage` 用户执行，因此需要预先生成 `trustdb`。

### 选择频道

系统使用默认的 stable 关键字配置时，对应 stable 频道。

系统全局使用 unstable（`~ARCH`）关键字时，对应 unstable 频道。

关于 `~ARCH` 的说明见 [Gentoo Wiki](https://wiki.gentoo.org/wiki//etc/portage/package.accept_keywords#.7EARCH_system-wide)。

### 配置二进制包仓库

stable 频道使用以下配置：

```{ztmpl lang="ini" path="/etc/portage/binrepos.conf/gentoo-zh.conf"}
[gentoo-zh]
priority = 10
sync-uri = {{endpoint}}/binpkgs/x86-64
location = /var/cache/binhost/gentoo-zh
verify-signature = true
```

使用 unstable 频道时，将 `sync-uri` 改为：

```{ztmpl}
sync-uri = {{endpoint}}/unstable/binpkgs/x86-64
```

### 安装二进制包

在有合适的二进制包时自动下载并使用：

```{ztmpl path="/etc/portage/make.conf" append="true"}
FEATURES="${FEATURES} getbinpkg"
```

如果没有合适的二进制包，Portage 会照常从源码编译。

单次使用二进制包安装：

```{ztmpl lang="bash"}
{{sudo}}emerge --ask --getbinpkg <package>
```

`verify-signature = true` 只要求验证该仓库的签名。全局启用 `FEATURES="binpkg-request-signature"` 时，还需要为本机通过 `FEATURES="buildpkg"` 构建的包配置签名。

频道区别及公钥下载方法见 [gentoo-zh binhost FAQ](https://distfiles.gentoozh.org/faq)，高级配置参考 [Binary package guide](https://wiki.gentoo.org/wiki/Binary_package_guide)。
