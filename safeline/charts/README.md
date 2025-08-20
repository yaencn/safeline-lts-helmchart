# Helm Chart for SafeLine

## Prerequisites

- Kubernetes cluster storage support RWX.

## Installation

- HelmChart GitRepo URL:
https://github.com/yaencn/safeline-lts-helmchart.git

https://gitee.com/andyhau/safeline-lts-helmchart.git

- HelmChart Web URL:
https://helm.yaencn.com/

- HelmChart Repo URL:

https://helm.yaencn.com/charts

- Install the SafeLine helm chart with a release name `safeline-lts`:
```bash
# add repo
helm repo add yaencn https://helm.yaencn.com/charts
# install sample
helm install safeline --namespace safeline --set global.ingress.enabled=true --set global.ingress.hostname="waf.local"  yaencn/safeline-lts
# upgrade
helm -n safeline upgrade safeline yaencn/safeline-lts
# fetch chart
helm fetch --version 1.0.0 yaencn/safeline-lts
```

## Uninstallation

To uninstall/delete the `safeline` deployment:
```bash
helm -n safeline uninstall safeline
```


## Installation Remind

Reminder:

This branch is the LTS (Long Term Support) stable version branch, and only contains LTS versions.
To use the preview version, please visit the following GIT repository:
https://github.com/yaencn/safeline-helmchart

- LTS Repository for github
https://github.com/yaencn/safeline-lts-helmchart

Acceleration address of GIT warehouse in Chinese Mainland:

- Preview Repository
https://gitee.com/andyhau/safeline-helmchart.git

- LTS Repository
https://gitee.com/andyhau/safeline-lts-helmchart.git

**提醒：**

此分支为LTS(长期支持)稳定版本分支，此分支中的仅包含LTS版本。
如需使用预览版，请访问如下GIT仓库：
https://github.com/yaencn/safeline-helmchart

GIT仓库在中国大陆的加速地址：
- 预览版仓库
https://gitee.com/andyhau/safeline-helmchart.git

- LTS版仓库
https://gitee.com/andyhau/safeline-lts-helmchart.git

------Installation Remind------

**Warning:** 

Any deployment resource in this HelmChart can only deploy one pod replica!
If multiple pod replicas are run, the entire WAF service will encounter unknown errors.
Acceleration address of GIT warehouse in Chinese Mainland:

**警告：**

该HelmChart中的任何deployment资源清单只能部署一个pod副本!
如果运行多个pod副本，整个WAF服务将出现未知错误.




Add WAF console web interface to bind domain names through nginx-ingress.

Specifically participate in the values.yaml file.

增加WAF控制台web界面可通过nginx-ingress绑定域名,具体参加values.yaml文件。

```yaml
  # 设置雷池WAF控制台通过域名访问，如：demo.waf-ce.chaitin.cn
  global:
    ingress:
      # 是否开启雷池WAF控制通过域名访问，默认未开启
      enabled: true
      hostname: waf.local
      ingressClassName: nginx
      pathType: ImplementationSpecific
      path: /
      tls:
        # 是否加载HelmChart外部的HTTPS域名证书Secret,如果有请填写Secret名称，默认不填写及域名仅开启http访问.
        # 如填写如下项，请在运行该HelmChart前创建好对应的Secret。
        secretName: "waf-xxx-com-tls"
```