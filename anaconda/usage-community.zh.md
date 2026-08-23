## 使用方法：仅使用社区频道

如果当前镜像站不提供 Anaconda 官方仓库，或者您不希望使用 `defaults`，请使用本节配置。配置中的 `nodefaults` 会阻止 conda 回落到 Anaconda 官方仓库。请勿与上一节配置同时使用。

不同系统下的 `.condarc` 路径如下：

- Linux: `${HOME}/.condarc`
- macOS: `${HOME}/.condarc`
- Windows: `C:\Users\<YourUserName>\.condarc`

Windows 用户如果无法直接创建名为 `.condarc` 的文件，可以先执行 `conda config --set show_channel_urls yes` 生成该文件，然后再修改。

```{ztmpl lang="yaml" path="~/.condarc"}
channels:
  - conda-forge
  - nodefaults
show_channel_urls: true
custom_channels:
  conda-forge: {{endpoint}}/cloud
```

使用下列命令清除索引缓存，并安装常用包测试配置。

```{ztmpl lang="bash"}
conda clean -i
conda create -n myenv numpy
```

如需使用 bioconda 等其他社区频道，请先确认当前镜像站提供该频道，再参考下方的第三方源列表添加。
