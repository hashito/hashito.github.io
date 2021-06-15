---
title: Dockerで不要なの全部消す
date: 2020/08/29 17:47:00
categories:
- [docker]
tags:
- [docker]
- [command]
---


Dockerでcontainerやらが色々と散らかった場合に行いたいコマンド

```
docker system prune  
```

下記のように聞かれます。

```
>WARNING! This will remove:
>  - all stopped containers
>  - all networks not used by at least one container
>  - all dangling images
>  - all dangling build cache
>Are you sure you want to continue? [y/N] 
```

`y`で全て消されます。