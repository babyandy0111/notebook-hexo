---
title: how to build x86_64 image on M1
date: 2022-06-16 17:57:29
categories:
tags:
---

透過M1會建立arm64版本， 透過 docker inspect IMAGE_ID可以看到
```shell
$ docker image inspect 0382b9b17bdb
{
  ...
  "Architecture": "arm64",
        "Variant": "v8",
        "Os": "linux",
        "Size": 223036168,
        "VirtualSize": 223036168,
  ...
}
```


所以建立imagge時，改成
```shell
$ docker buildx build --platform=linux/amd64 . -t xxx
```

<br>

之後就可按照正常的docker tag、docker push進行操作