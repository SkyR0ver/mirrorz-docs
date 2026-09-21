在 Ubuntu 24.04 之前，Ubuntu 的软件源配置文件使用传统的 One-Line-Style，路径为 `/etc/apt/sources.list`；从 Ubuntu 24.04 开始，Ubuntu 的软件源配置文件变更为 DEB822 格式，路径为 `/etc/apt/sources.list.d/ubuntu.sources`。

## 我正在使用什么版本的 Ubuntu？

可以通过以下命令查看当前系统版本：

```bash
cat /etc/os-release
```

例如在 Ubuntu 24.04 下，文件内容如下：

```conf
PRETTY_NAME="Ubuntu 24.04.1 LTS"
NAME="Ubuntu"
VERSION_ID="24.04"
VERSION="24.04.1 LTS (Noble Numbat)"
VERSION_CODENAME=noble
ID=ubuntu
ID_LIKE=debian
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
UBUNTU_CODENAME=noble
LOGO=ubuntu-logo
```

`VERSION_ID` 后面的版本号即为当前系统版本，`VERSION_CODENAME` 后面的是代号。代号在软件源配置文件中会被使用。**请注意在下面的配置中选择符合你的机器的版本号，在替换配置前确认代号一致，否则之后的更新操作可能导致系统出现问题。**

Ubuntu 18.04 LTS 及以上版本默认支持 HTTPS 源。如果遇到无法拉取 HTTPS 源的情况，请先使用 HTTP 源并安装通用 CA 证书：

```{ztmpl lang="bash"}
{{sudo}}apt install ca-certificates
```
