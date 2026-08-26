## 使用方法

### 传统格式（`/etc/apt/sources.list`）

```{ztmpl lang="properties" input="release src nf mirror_security" path="/etc/apt/sources.list"}
# 默认注释了源码镜像以提高 apt update 速度，如有需要可自行取消注释
{{#sid}}
deb {{endpoint}}/ sid main contrib{{#nf}}{{nonfree}}{{/nf}}
{{src}}deb-src {{endpoint}}/ sid main contrib{{#nf}}{{nonfree}}{{/nf}}
{{/sid}}
{{^sid}}
deb {{endpoint}}/ {{release}} main contrib{{#nf}}{{nonfree}}{{/nf}}
{{src}}deb-src {{endpoint}}/ {{release}} main contrib{{#nf}}{{nonfree}}{{/nf}}

deb {{endpoint}}/ {{release}}-updates main contrib{{#nf}}{{nonfree}}{{/nf}}
{{src}}deb-src {{endpoint}}/ {{release}}-updates main contrib{{#nf}}{{nonfree}}{{/nf}}

deb {{endpoint}}/ {{release}}-backports main contrib{{#nf}}{{nonfree}}{{/nf}}
{{src}}deb-src {{endpoint}}/ {{release}}-backports main contrib{{#nf}}{{nonfree}}{{/nf}}

{{#mirror_security}}
# 以下安全更新软件源为镜像站配置
deb {{endpoint}}-security {{release}}{{security}} main contrib{{#nf}}{{nonfree}}{{/nf}}
{{src}}deb-src {{endpoint}}-security {{release}}{{security}} main contrib{{#nf}}{{nonfree}}{{/nf}}
{{/mirror_security}}
{{^mirror_security}}
# 以下安全更新软件源为官方源配置
deb https://security.debian.org/debian-security {{release}}{{security}} main contrib{{#nf}}{{nonfree}}{{/nf}}
{{src}}deb-src https://security.debian.org/debian-security {{release}}{{security}} main contrib{{#nf}}{{nonfree}}{{/nf}}
{{/mirror_security}}
{{/sid}}
```

### DEB822 格式（`/etc/apt/sources.list.d/debian.sources`）

```{ztmpl lang="yaml" input="release_deb822 src nf mirror_security" path="/etc/apt/sources.list.d/debian.sources"}
{{#sid}}
Types: deb
URIs: {{endpoint}}
Suites: sid
Components: main contrib{{#nf}} non-free non-free-firmware{{/nf}}
Signed-By: /usr/share/keyrings/debian-archive-keyring.gpg

# 默认注释了源码镜像以提高 apt update 速度，如有需要可自行取消注释
{{src}}Types: deb-src
{{src}}URIs: {{endpoint}}
{{src}}Suites: sid
{{src}}Components: main contrib{{#nf}} non-free non-free-firmware{{/nf}}
{{src}}Signed-By: /usr/share/keyrings/debian-archive-keyring.gpg
{{/sid}}
{{^sid}}
Types: deb
URIs: {{endpoint}}
Suites: {{release_deb822}} {{release_deb822}}-updates {{release_deb822}}-backports
Components: main contrib{{#nf}} non-free non-free-firmware{{/nf}}
Signed-By: /usr/share/keyrings/debian-archive-keyring.gpg

# 默认注释了源码镜像以提高 apt update 速度，如有需要可自行取消注释
{{src}}Types: deb-src
{{src}}URIs: {{endpoint}}
{{src}}Suites: {{release_deb822}} {{release_deb822}}-updates {{release_deb822}}-backports
{{src}}Components: main contrib{{#nf}} non-free non-free-firmware{{/nf}}
{{src}}Signed-By: /usr/share/keyrings/debian-archive-keyring.gpg

{{#mirror_security}}
# 以下安全更新软件源为镜像站配置
Types: deb
URIs: {{endpoint}}-security
Suites: {{release_deb822}}-security
Components: main contrib{{#nf}} non-free non-free-firmware{{/nf}}
Signed-By: /usr/share/keyrings/debian-archive-keyring.gpg

{{src}}Types: deb-src
{{src}}URIs: {{endpoint}}-security
{{src}}Suites: {{release_deb822}}-security
{{src}}Components: main contrib{{#nf}} non-free non-free-firmware{{/nf}}
{{src}}Signed-By: /usr/share/keyrings/debian-archive-keyring.gpg
{{/mirror_security}}
{{^mirror_security}}
# 以下安全更新软件源为官方源配置
Types: deb
URIs: https://security.debian.org/debian-security
Suites: {{release_deb822}}-security
Components: main contrib{{#nf}} non-free non-free-firmware{{/nf}}
Signed-By: /usr/share/keyrings/debian-archive-keyring.gpg

{{src}}Types: deb-src
{{src}}URIs: https://security.debian.org/debian-security
{{src}}Suites: {{release_deb822}}-security
{{src}}Components: main contrib{{#nf}} non-free non-free-firmware{{/nf}}
{{src}}Signed-By: /usr/share/keyrings/debian-archive-keyring.gpg
{{/mirror_security}}
{{/sid}}
```

为了方便快速配置，此处一并附上了 debian-security 的配置，一般来说，镜像站会同时提供 debian-security，为了更准确的信息您可以前往 [Debian Security 帮助](../debian-security/) 确认。

不过，一般来说，为了更及时地获得安全更新，不推荐使用镜像站安全更新软件源，因为镜像站往往有同步延迟。参考 https://www.debian.org/security/faq.en.html#mirror

> The purpose of security.debian.org is to make security updates available as quickly and easily as possible.
>
> Encouraging the use of unofficial mirrors would add extra complexity that is usually not needed and that can cause frustration if these mirrors are not kept up to date.
