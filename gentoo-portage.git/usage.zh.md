## 使用方法

### 从 rsync 切换到 Git 或修改镜像

安装 `eselect-repository`：

```{ztmpl lang="bash"}
{{sudo}}emerge --ask app-eselect/eselect-repository
```

删除现有的 Gentoo ebuild 仓库配置和本地副本，再使用相应镜像添加 Git 仓库：

```{ztmpl lang="bash"}
{{sudo}}eselect repository remove -f gentoo
{{sudo}}eselect repository add gentoo git {{endpoint}}
```

同步 Gentoo ebuild 仓库：

```{ztmpl lang="bash"}
{{sudo}}emaint sync -r gentoo
```

不同镜像的同步进度可能不同；更换 Git 镜像时，建议使用上述方法删除并重新添加仓库。

## 手动从 rsync 切换或更换镜像

系统默认配置文件位于 `/etc/portage/repos.conf/gentoo.conf`，只需修改 `sync-uri` 与 `sync-type`。

通过 `eselect-repository` 生成的配置位于 `/etc/portage/repos.conf/eselect-repo.conf`。

完整配置示例：

```{ztmpl lang="ini" path="/etc/portage/repos.conf/gentoo.conf"}
[gentoo]
location = /var/db/repos/gentoo
sync-type = git
sync-uri = {{endpoint}}
```

首次从 rsync 切换到 Git 或更换镜像时，删除现有的本地仓库并重新同步：

```{ztmpl lang="bash"}
{{sudo}}rm -rf /var/db/repos/gentoo
{{sudo}}emaint sync -r gentoo
```

配置原理和排障方法见 [Portage with Git](https://wiki.gentoo.org/wiki/Portage_with_Git)。
