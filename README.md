<h4 align="center">
  SafeLine - TLS versions - HelmChart - Unofficial production
</h4>

<p align="center">
  <a target="_blank" href="https://waf-ce.chaitin.cn/">SafeLine Website</a> &nbsp; | &nbsp;
  <a target="_blank" href="https://github.com/yaencn/safeline-lts-helmchart">TLS Repository</a> &nbsp; | &nbsp;
  <a target="_blank" href="https://github.com/yaencn/safeline-helmchart">Preview Repository</a> &nbsp; | &nbsp;
  <a target="_blank" href="https://github.com/yaencn/safeline-lts-helmchart/blob/main/README_CN.md">中文文档</a>
</p>

## ----- Readme -----

**Reminder:**

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

helm coding-push safeline-lts-${app-version}.tgz safeline-lts
```

## ----- Installation Remind -----

**Warning:** 

Any deployment resource in this HelmChart can only deploy one pod replica!
If multiple pod replicas are run, the entire WAF service will encounter unknown errors.

## ----- Configuration Description -----

Add WAF console web interface to bind domain names through nginx-ingress.
Specifically participate in the values.yaml file.

```yaml
  # Set up the SafeLine WAF console to be accessed through a domain name.
  # For example: demowaf-ce.chaitin.cn
  ingress:
    # Whether to enable SafeLine WAF control for domain access.
    # It is not enabled by default
    enabled: true
    hostname: waf.local
    ingressClassName: nginx
    pathType: ImplementationSpecific
    path: /
    tls:
      # Whether to load the Secret of the HTTPS domain name certificate outside HelmChart. 
      #If yes, please fill in the Secret name. By default, it is not filled in and the domain name only enables http access.
      # If you fill in the following items, please create the corresponding Secret before running the HelmChart.
      secretName: "waf-xxx-com-tls"
```

## ----- HelmChart Install -----

- HelmChart Web URL:
https://g-otkk6267.coding.net/public-artifacts/Charts/safeline-lts/packages

- HelmChart Repo URL:
https://g-otkk6267-helm.pkg.coding.net/Charts/safeline-lts

- Sample：

```shell
helm repo add safeline https://g-otkk6267-helm.pkg.coding.net/Charts/safeline-lts
helm fetch --version 7.1.1 safeline-lts/safeline-lts
```