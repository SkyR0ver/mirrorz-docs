## 使用方法

### 命令替换

用以下命令替换 `/etc/yum.repos.d` 下的文件

```{ztmpl lang="bash"}
{{sudo}}sed -e 's|^metalink=|#metalink=|g' \
    -e 's|^#baseurl=http://download.example/pub/fedora/linux|baseurl={{endpoint}}|g' \
    -i.bak \
    /etc/yum.repos.d/fedora.repo \
    /etc/yum.repos.d/fedora-updates.repo
```

### 手动替换

**`fedora` 仓库 (/etc/yum.repos.d/fedora.repo)**

```{ztmpl lang="ini"}
[fedora]
name=Fedora $releasever - $basearch
baseurl={{endpoint}}/releases/$releasever/Everything/$basearch/os/
#metalink=https://mirrors.fedoraproject.org/metalink?repo=fedora-$releasever&arch=$basearch
enabled=1
countme=1
metadata_expire=7d
repo_gpgcheck=0
type=rpm
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-fedora-$releasever-$basearch
skip_if_unavailable=False
```

**`updates` 仓库 (/etc/yum.repos.d/fedora-updates.repo)**

```{ztmpl lang="ini"}
[updates]
name=Fedora $releasever - $basearch - Updates
baseurl={{endpoint}}/updates/$releasever/Everything/$basearch/
#metalink=https://mirrors.fedoraproject.org/metalink?repo=updates-released-f$releasever&arch=$basearch
enabled=1
countme=1
repo_gpgcheck=0
type=rpm
gpgcheck=1
metadata_expire=6h
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-fedora-$releasever-$basearch
skip_if_unavailable=False
```
