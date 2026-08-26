### 自动选择软件源镜像

[mirrorz-302](https://github.com/mirrorz-org/mirrorz-302) 提供 mirrorlist 接口，会根据你的地理位置、网络、ISP 等条件返回按可用性排序的镜像列表，客户端到列表中排在最前面的镜像拉取软件包，当排在前面的镜像不可用时还会自动回退到后续镜像。相比手动配置单一镜像地址，这种方式能自动为你选择网络条件最佳的镜像。

例如 `BaseOS` 仓库（`/etc/yum.repos.d/rocky.repo` 或 `Rocky-*.repo`）：

```{ztmpl lang="ini"}
[baseos]
name=Rocky Linux $releasever - BaseOS
#baseurl=http://dl.rockylinux.org/$contentdir/$releasever/BaseOS/$basearch/os
mirrorlist=https://mirrors.cernet.edu.cn/api/rpm/mirrorlist/rocky/$contentdir/$releasever/BaseOS/$basearch/os
enabled=1
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-rockyofficial
```

也可以使用命令，将各仓库配置中的 `mirrorlist=` 替换为指向 mirrorz-302 的 `mirrorlist=`（如无特殊需求也可直接替换默认配置）：

```{ztmpl lang="bash"}
{{sudo}}sed -e 's|^mirrorlist=http://dl.rockylinux.org/$contentdir|mirrorlist=https://mirrors.cernet.edu.cn/api/rpm/mirrorlist/rocky|g' \
         -i.bak \
         /etc/yum.repos.d/rocky*.repo
```

配置完成后请运行 `{{sudo}}dnf makecache` 更新缓存。
