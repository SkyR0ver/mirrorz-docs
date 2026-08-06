相应镜像不提供 ebuild，需要搭配 [Gentoo Portage 镜像](../gentoo-portage/)或 [Gentoo Portage Git 镜像](../gentoo-portage.git/)使用。

## Distfiles 配置

在 `/etc/portage/make.conf` 中加入：

```{ztmpl path="/etc/portage/make.conf" append="true"}
GENTOO_MIRRORS="{{endpoint}}"
```

配置好 Portage 与 Distfiles 后，执行 `emerge --sync` 进行更新。

## 二进制包

[Gentoo 官方二进制包仓库（binhost）](https://wiki.gentoo.org/wiki/Project:Binhost)提供预编译并签名的二进制包。

### 配置二进制包仓库

以下配置使用当前的 `23.0` profile，具体路径见 [amd64 二进制包目录](https://distfiles-cdn-origin.gentoo.org/releases/amd64/binpackages/)和 [arm64 二进制包目录](https://distfiles-cdn-origin.gentoo.org/releases/arm64/binpackages/)。

较新的 Stage 3 已在 `/etc/portage/binrepos.conf/` 中预配置官方 binhost。所以只需要编辑 `[gentoo]` 配置中的镜像地址；若没有 `[gentoo]` 配置，则新增它。

```{ztmpl path="/etc/portage/binrepos.conf/gentoo.conf"}
[gentoo]
priority = 1
sync-uri = {{endpoint}}/releases/amd64/binpackages/23.0/x86-64
location = /var/cache/binhost/gentoo
verify-signature = true
```

Gentoo Binhost 项目目前支持使用 GNU 工具链（glibc、GCC 和 binutils）的 amd64 和 arm64。其他架构和工具链也会发布二进制包，但仅限 Release Engineering 构建 Stage 3 所用的包缓存。

`sync-uri` 指向包含 `Packages` 文件的目录。上方配置使用常规 amd64 的 x86-64 二进制包；常规 arm64 系统可将其改为 {ztmpl}`{{endpoint}}/releases/arm64/binpackages/23.0/arm64`。

对于常规 amd64 系统，官方建议 CPU 支持 [x86-64-v3](https://www.gentoo.org/news/2024/02/04/x86-64-v3.html) 时使用对应的二进制包，以获得针对该指令集的优化。检查 CPU 是否支持：

```{ztmpl lang="bash"}
ld.so --help
```

输出中包含 `x86-64-v3 (supported, searched)` 即表示支持。可将配置改为：

```{ztmpl}
sync-uri = {{endpoint}}/releases/amd64/binpackages/23.0/x86-64-v3
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

根据 [Portage binpkg changes](https://www.gentoo.org/support/news-items/2026-05-03-portage-binpkg-changes.html)，新版 Portage 默认验证远程二进制包的签名，并将其缓存到 `location` 指定的目录。官方 binhost 用户不再需要启用 `FEATURES="binpkg-request-signature"`；首次下载时，Portage 会自动运行 `getuto` 创建可信密钥环。

参考 [Gentoo Binary Host Quickstart](https://wiki.gentoo.org/wiki/Gentoo_Binary_Host_Quickstart)，高级配置参考 [Binary package guide](https://wiki.gentoo.org/wiki/Binary_package_guide)。

## Gentoo Prefix Bootstrap 镜像配置

在运行 Bootstrap 脚本之前，可通过设置以下环境变量选择 Bootstrap 过程中使用的镜像。

```{ztmpl lang="bash"}
export GENTOO_MIRRORS="{{endpoint}}"
export SNAPSHOT_URL="{{endpoint}}/snapshots"
# export GNU_URL="http://mirror/gnu"
```

`GNU_URL` 的具体设置可以参考 [GNU 帮助](../gnu/)。

Bootstrap 成功后，若对 Gentoo Portage 和 Distfiles 换源，可参照以上几节，只需将 `/etc` 换成 `$EPREFIX/etc`。
