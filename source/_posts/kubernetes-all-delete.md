---
title: kubernetesでpotやらserviceを全削除する方法
date: 2020/08/17 6:42:00
categories:
- [開発環境]
- [インフラ]
tags:
- [kubernetes]
---
kubernetesでpotやらserviceを全削除する方法  

```
for each in $(kubectl get ns -o jsonpath="{.items[*].metadata.name}" | grep -v kube-system);
do
  kubectl delete ns $each
done
```

特定のnamespaceであればこちらが有効らしい。
個人的には下記をよく使う。

```
kubectl delete daemonsets,replicasets,services,deployments,pods,rc --all
```

したのコメントにもあるが、イングレスが抜けているっぽい。追加すると下記のような感じ？

```
kubectl delete daemonsets,replicasets,services,deployments,pods,rc,ing --all
```

https://stackoverflow.com/questions/33509194/command-to-delete-all-pods-in-all-kubernetes-namespaces