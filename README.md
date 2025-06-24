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
The LTS version starts from 8.8.0-lts and also supports international version deployment.
To use the preview version, please visit the following GIT repository:
https://github.com/yaencn/safeline-helmchart

- LTS Repository for github
https://github.com/yaencn/safeline-lts-helmchart

Acceleration address of GIT warehouse in Chinese Mainland:

- Preview Repository
https://gitee.com/andyhau/safeline-helmchart.git

- LTS Repository
https://gitee.com/andyhau/safeline-lts-helmchart.git

- International version deployment support
International version deployment support for the x86_64 architecture is supported starting from appVersion 8.8.0. If you need to deploy the international version, please modify the value.yaml file parameters to the following values:

`global.image.registry=chaitin`

`global.image.region="-g"`

## ----- HelmChart Install -----

- HelmChart Web URL:
https://g-otkk6267.coding.net/public-artifacts/Charts/safeline-lts/packages

- HelmChart Repo URL:
https://g-otkk6267-helm.pkg.coding.net/Charts/safeline-lts

- Sample：
```shell
# add repo
helm repo add safeline-lts https://g-otkk6267-helm.pkg.coding.net/Charts/safeline-lts
# install sample
helm install safeline-lts --namespace safeline \
  --set global.ingress.enabled=true \
  --set global.ingress.hostname="waf.local" \
  safeline/safeline

# install the International version
helm install safeline-lts --namespace safeline \
  --set global.ingress.enabled=true \
  --set global.ingress.hostname="waf.local" \
  --set global.image.registry=chaitin \
  --set global.image.region="-g" \
  safeline/safeline

# upgrade
helm -n safeline upgrade safeline-lts safeline-lts/safeline-lts
# fetch chart
helm fetch --version 1.0.0 safeline-lts/safeline-lts
# uninstall
helm -n safeline uninstall safeline-lts
```


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
  global:
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