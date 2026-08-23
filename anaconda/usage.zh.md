## 使用方法：包含 Anaconda 官方仓库

如果当前镜像站提供 `pkgs/main`、`pkgs/r` 和 `pkgs/msys2`，可以使用本节配置。此配置包含受 Anaconda 服务条款约束的官方仓库；如果您只需要社区频道，请改用下一节配置。两套配置请勿同时使用。

各系统都可以通过修改用户目录下的 `.condarc` 文件来使用镜像站。

不同系统下的 `.condarc` 目录如下：

- Linux: `${HOME}/.condarc`
- macOS: `${HOME}/.condarc`
- Windows: `C:\Users\<YourUserName>\.condarc`

注：

* Windows 用户无法直接创建名为 `.condarc` 的文件，可先执行 `conda config --set show_channel_urls yes` 生成该文件之后再修改。
* 如果您正在从某一镜像源切换到另一镜像源，请检查镜像站是否同步了您需要的频道，以及该频道是否支持您使用的平台（例如 `linux-64`）。
* 以下配置只加入少量常用的第三方频道，您可以参考下方的列表添加其他频道。

```{ztmpl lang="yaml" path="~/.condarc"}
channels:
  - defaults
show_channel_urls: true
default_channels:
  - {{endpoint}}/pkgs/main
  - {{endpoint}}/pkgs/r
  - {{endpoint}}/pkgs/msys2
custom_channels:
  conda-forge: {{endpoint}}/cloud
  pytorch: {{endpoint}}/cloud
```

使用下列命令清除索引缓存，并安装常用包测试一下。

```{ztmpl lang="bash"}
conda clean -i
conda create -n myenv numpy
```
