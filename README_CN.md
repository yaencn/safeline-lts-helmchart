<h4 align="center">
  SafeLine - 雷池 - 长期支持TLS版本 - 自制HelmChart
</h4>

<p align="center">
  <a target="_blank" href="https://waf-ce.chaitin.cn/">雷池官方网站</a> &nbsp; | &nbsp;
  <a target="_blank" href="https://github.com/yaencn/safeline-lts-helmchart">TLS版GIT仓库</a> &nbsp; | &nbsp;
  <a target="_blank" href="https://github.com/yaencn/safeline-helmchart">预览版GIT仓库</a> &nbsp; | &nbsp;
  <a target="_blank" href="https://github.com/yaencn/safeline-lts-helmchart/blob/main/README_CN.md">中文文档</a>
</p>

## ----- Readme -----

**提醒：**

此分支为LTS(长期支持)稳定版本分支，此分支中的仅包含LTS版本。
如需使用预览版，请访问如下GIT仓库：
https://github.com/yaencn/safeline-helmchart

GIT仓库在中国大陆的加速地址：
- 预览版仓库
https://gitee.com/andyhau/safeline-helmchart.git

- LTS版仓库
https://gitee.com/andyhau/safeline-lts-helmchart.git



## ----- Helm Build -----

```shell
# 检测helmchart-template是否有语法问题
cd safeline
helm template charts/ --output-dir ./result 
```

```shell
helm package ./safeline/charts/ -d ./assets/safeline

helm repo index --merge ./index.yaml --url assets assets/

rm -f ./index.yaml

mv ./assets/index.yaml ./index.yaml

cd ./assets/safeline/

helm coding-push safeline-lts-${app-version}.tgz safeline
```

## ----- Installation Remind -----

**警告：**

该HelmChart中的任何deployment资源清单只能部署一个pod副本!
如果运行多个pod副本，整个WAF服务将出现未知错误.


## ----- Configuration Description -----

WAF控制台web界面可通过nginx-ingress绑定域名,具体参加values.yaml文件。

```yaml
  # 设置雷池WAF控制台通过域名访问，如：demo.waf-ce.chaitin.cn
  ingress:
    # 是否开启雷池WAF控制通过域名访问，默认未开启
    enabled: true
    hostname: waf.local
    ingressClassName: nginx
    pathType: ImplementationSpecific
    path: /
    tls:
      # 是否加载HelmChart外部的HTTPS域名证书的Secret.
      # 如果有则请填写Secret名称，默认不填写及域名仅开启http访问.
      # 如填写如下项，请在运行该HelmChart前创建好对应的Secret。
      secretName: "waf-xxx-com-tls"
```

## ----- HelmChart Install -----

- HelmChart网页地址:
https://g-otkk6267.coding.net/public-artifacts/Charts/safeline-lts/packages

- HelmChart仓库地址:
https://g-otkk6267-helm.pkg.coding.net/Charts/safeline-lts

- 举例：
helm repo add safeline https://g-otkk6267-helm.pkg.coding.net/Charts/safeline-lts