## 使用方法

### rsync 方式

修改 `/etc/portage/repos.conf/gentoo.conf`，将

```{ztmpl}
sync-uri = rsync://rsync.gentoo.org/gentoo-portage
```

修改为

```{ztmpl}
sync-uri = rsync://{{host}}{{path}}
```

注意在使用前需要检查所选镜像站提供的 rsync 是否可以访问，MirrorZ 并未记录该元数据。
