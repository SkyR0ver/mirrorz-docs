## Miniforge 与 Mamba

[Miniforge](https://github.com/conda-forge/miniforge) 是由 conda-forge 社区维护的精简发行版，默认只使用 conda-forge，并同时提供 `conda` 和 `mamba`。安装包可以从 [Miniforge 官方发布页](https://github.com/conda-forge/miniforge/releases)下载；部分镜像站也可能通过单独的 GitHub Release 镜像提供安装包。

Mamba 是与 conda 软件包和环境生态兼容的高性能客户端。当前 Miniforge 已经包含 Mamba，无需再选择 Mambaforge；Mambaforge 已被弃用，并已于 2025 年停止发布新版本。

使用 Mamba 时，可以通过 `.condarc` 中的 `mirrored_channels` 将 conda-forge 指向当前镜像站：

```{ztmpl lang="yaml" path="~/.condarc"}
channels:
  - conda-forge
mirrored_channels:
  conda-forge:
    - {{endpoint}}/cloud/conda-forge
```

可以使用以下命令检查配置是否生效：

```{ztmpl lang="bash"}
mamba config list --json | python -c "import sys, json; info = json.loads(sys.stdin.read()); print(info['mirrored_channels']['conda-forge'])"
```
