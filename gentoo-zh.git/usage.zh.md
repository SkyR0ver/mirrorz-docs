## 使用方法

### 首次配置或修改镜像

安装 `eselect-repository`：

```{ztmpl lang="bash"}
{{sudo}}emerge --ask app-eselect/eselect-repository
```

使用相应镜像添加 gentoo-zh overlay：

```{ztmpl lang="bash"}
{{sudo}}eselect repository add gentoo-zh git {{endpoint}}
```

同步 gentoo-zh overlay：

```{ztmpl lang="bash"}
{{sudo}}emaint sync -r gentoo-zh
```

不同镜像的同步进度可能不同；更换 Git 镜像时，建议使用 `eselect-repository` 删除并重新添加仓库。

### 手动配置或更换镜像

首次配置时，创建 `/etc/portage/repos.conf/gentoo-zh.conf`。
更换 Git 镜像时，只需编辑 `/etc/portage/repos.conf/` 中包含 `[gentoo-zh]` 的配置文件。
通过 `eselect-repository` 生成的配置位于 `/etc/portage/repos.conf/eselect-repo.conf`。

完整配置示例：

```{ztmpl lang="ini" path="/etc/portage/repos.conf/gentoo-zh.conf"}
[gentoo-zh]
location = /var/db/repos/gentoo-zh
sync-type = git
sync-uri = {{endpoint}}
auto-sync = yes
```

更换镜像时，将 `sync-uri` 改为 {ztmpl}`{{endpoint}}`，再删除现有的本地仓库：

```{ztmpl lang="bash"}
{{sudo}}rm -rf /var/db/repos/gentoo-zh
```

同步 gentoo-zh overlay：

```{ztmpl lang="bash"}
{{sudo}}emaint sync -r gentoo-zh
```

## 接受测试关键字

gentoo-zh 软件包仅提供 `~ARCH`（测试）关键字，没有 stable 关键字。已全局使用 `~amd64` 的系统可跳过此步骤；使用 stable 关键字的系统需要先接受 `~amd64`。

关于 `~ARCH` 的说明见 [Gentoo Wiki](https://wiki.gentoo.org/wiki//etc/portage/package.accept_keywords#.7EARCH_system-wide)。

`::gentoo-zh` 限定仅作用于该 overlay。由于其中的软件包均使用 `~ARCH`，可接受整个 overlay 的 `~amd64` 关键字，Gentoo 主仓库的软件包不受影响：

```{ztmpl path="/etc/portage/package.accept_keywords/gentoo-zh" append="true"}
*/*::gentoo-zh ~amd64
```

也可逐个接受软件包的测试关键字：

```{ztmpl path="/etc/portage/package.accept_keywords/gentoo-zh" append="true"}
net-im/tencent-qq ~amd64
```

## 安装软件包

```{ztmpl lang="bash"}
{{sudo}}emerge --ask net-im/tencent-qq
```

列出 overlay 提供的软件包：

```{ztmpl lang="bash"}
eix -RO gentoo-zh
```
