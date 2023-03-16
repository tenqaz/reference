kubernetes 的个人备忘清单
===

基本命令
-----

### rollout

```shell

# 重启deployment的所有的Pod，会逐个重启
kubectl rollout restart deployment <deployment_name> -n <namespace>
```
