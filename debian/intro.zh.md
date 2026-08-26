大部分 Debian 的软件源配置文件使用传统的 One-Line-Style，路径为 `/etc/apt/sources.list`；但是对于容器镜像，从 Debian 12 开始，其软件源配置文件变更为 DEB822 格式，路径为 `/etc/apt/sources.list.d/debian.sources`。一般情况下，将对应文件中 Debian 默认的源地址 `http://deb.debian.org/` 替换为镜像地址即可。

Debian Buster 以上版本默认支持 HTTPS 源。如果遇到无法拉取 HTTPS 源的情况，请先使用 HTTP 源并安装：

```{ztmpl lang="bash"}
{{sudo}}apt install apt-transport-https ca-certificates
```
