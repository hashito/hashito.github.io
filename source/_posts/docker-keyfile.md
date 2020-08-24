---
title: Dockerで鍵ファイルを環境変数で受け取り、実行時に展開する方法
date: 2020/08/22 17:57:00
categories:
- [docker]
tags:
- [docker]
- [key]
---

# 概要

秘密鍵のフォルダをマウントさせると色々と面倒なので引数化することにした。

# 手順
## 1.key ファイルを置換

```
sed -z 's/\n/\\n/g' id_ras
```

## 2.dockerファイルに初期値として入力

```Dockerfile
FROM ubuntu

ENV key="-----BEGIN OPENSSH PRIVATE KEY-----\nb3BlbnNzaC1rZXktdjEAAAAABG5vbmUAAAAEbm9uZQAAAAAAAAABAAACFwAAAAdzc2gtcn\nNhAAAAAwEAAQAAAgEAxrGqH4V9Fu1BeFnUo1n77a8qYXUs/9LECnb4lwUeenOXE3Y3LL7G\nuTcttn+B9Q6bzrN3BvDLjo3VKKtWI/X5pRuSf3wML8OMt8CDta0k4bexskmIzEzN/6yTWv\nssc12VLfXrQBtvVNnASb8r9cRKsDxjGmtG7w9eSz0x1ah7HR+Mbuva83hOBVutv+og4fN3\nmEvJMHL9B2OmtO1q+psSFzmu1xoDRhiDk406lUUWQBOwpyn8PjB1X3hrhFkosDpts099lT\nOul3KzWM+2O5+cnLhIw45TA+hXknltp7bg8cFpEdSOePDsLNrSFtTGibq4wtwT/cxXEDwG\nD9UPo6QpRWNs+wl76Be8BZ8E/xpZK4wLB94J7lt4OApH+8ERpGqA+zDz/kuZND6DPFbCDT\nLr1bmOcEqQykhCKno7vim7oPRPLjsEAc80pjlDYc2NFAp5+WoalboettXYRejXTTWNZXPN\nnqq40mhq+c+aIn+LHsyvdLvdYoCjqRx3RrQGZpITnJ5owdR13IwB70Wo1axEOb/PuBwTTu\n5ajWV4Nh6szc0xbkrEl7XrZDOfD5qxXr5EjpkOPy0vvWg932qpIKhhZplDjYzHnY5U+Zic\ngAK9Th7nQJz5jPoThV9dOX7DVLfPChx+f4ite69HbjI6g0Bm9AfPkJzkobqqR6rBR0Wc1C\n0AAAdQn9L4f5/S+H8AAAAHc3NoLXJzYQAAAgEAxrGqH4V9Fu1BeFnUo1n77a8qYXUs/9LE\nCnb4lwUeenOXE3Y3LL7GuTcttn+B9Q6bzrN3BvDLjo3VKKtWI/X5pRuSf3wML8OMt8CDta\n0k4bexskmIzEzN/6yTWvssc12VLfXrQBtvVNnASb8r9cRKsDxjGmtG7w9eSz0x1ah7HR+M\nbuva83hOBVutv+og4fN3mEvJMHL9B2OmtO1q+psSFzmu1xoDRhiDk406lUUWQBOwpyn8Pj\nB1X3hrhFkosDpts099lTOul3KzWM+2O5+cnLhIw45TA+hXknltp7bg8cFpEdSOePDsLNrS\nFtTGibq4wtwT/cxXEDwGD9UPo6QpRWNs+wl76Be8BZ8E/xpZK4wLB94J7lt4OApH+8ERpG\nqA+zDz/kuZND6DPFbCDTLr1bmOcEqQykhCKno7vim7oPRPLjsEAc80pjlDYc2NFAp5+Woa\nlboettXYRejXTTWNZXPNnqq40mhq+c+aIn+LHsyvdLvdYoCjqRx3RrQGZpITnJ5owdR13I\nwB70Wo1axEOb/PuBwTTu5ajWV4Nh6szc0xbkrEl7XrZDOfD5qxXr5EjpkOPy0vvWg932qp\nIKhhZplDjYzHnY5U+ZicgAK9Th7nQJz5jPoThV9dOX7DVLfPChx+f4ite69HbjI6g0Bm9A\nfPkJzkobqqR6rBR0Wc1C0AAAADAQABAAACAG3GWqdvqNyx2CoV91UIshdvX4rYojP0zjq5\n4D4PpfchRaaK+ZDPFhveUHMznyk1GP/qRyiegNgRpGMDxmO30mVWBmpIrrL05xneUuZc8r\nOCObq2xc2Z4XYQcpkhjD1wxqrN41tXzPqkE4irBi6SdHFJ67b87gPGCeKnvJC+tMYyV/Qw\nepdpMDHlpOkTAXfUe4640D7kSMd8Vu4+/YvXgPcz91UAGi7v/EHZFTTDJrfgKQkyORpiy3\nYjocNNPx2eKl2W3VtBYoRp6ox2tcfbNzue1RS13UebZkaWr+6pKz5mDRa8yLoo4VMm0kuq\n4sXVU57U2HawHqnpf6/flvRiDio+BYREEKxXSJIGeGzmp1MdyAEgFXkfpBYwcK1yxz+bFo\nNC32pNLQifOjMvbBvJvuY2bv3CEscWvUAYpVZHgBeJsx1Tx6ZnX8T7cPZ8IZW/Uhn6rt3o\n2tpR4vbY3iVsKZjGwc/vY8UY4S0RSpSP0WBFYWLkHTe00SJLsaIJgAhITooiyYj3SGRGW5\n3BsRU7KQqjUtl47HOh9W7Bl4IbqrUWMW58P/fGrmN/N8wwSTNsoaz5gZ/G0fX+Q6NOslVM\n0Il02oSWWBo60Dg4MXE7/1IW9K0a7QLRg52HN+E5KZvewgYajHbTxOj2PU+2Rhk0y5gzDL\nX3kfMba6Cbm/6AueJBAAABAQCEJKqLu4PhbZCPHKhveVuodMWrRZT9Gt7zl4ES8ahy8KzP\ne8Cd5tPoq3mDubuUCG6WuvtqQSSro1fkrjGbQ7Qt9h8cKGftgnmncMDI5Ghml/TtjUUGnb\nWW3A6VTKwyU+U6QRG5EM+2VNyoe8D8/c7OlVXZcc+0QTr0Ccne1a5FtzxAzxK+IdkY3m2q\nDs4HX4ZWRm6DhEhqxr0VM+5ifX2f8mcvw8r/b86Ooh2+JXSdqXdl7ckCYMfjC0T/osZYpk\nTQKLn95HKd05GcebiuJlWKUWQPVoAN42UrfrM4Pt8znXQZhaoAlQjfUlYsT99mgDfgZabQ\nMo6cI9+bXCkSPDhnAAABAQDzXYaKSngUEuWmKfzwFUetWOQUKVItFEyC/kQyGOASJz1OzG\n+U7kgJa0ZnwDgMAAzF41fJFRSGQXC2DZDETkoBW8nLf7q3Iaan+N+hxozeQg/aaaT+zz07\nsDNwKVhaP0kEj8/Pj9zVJXe76YjBRAFODXagZTUlGiq9knfPBd9AAMADxslUB6zmmNlegv\nNEOmz+LxkhkDZV2Ejz4dhhJ2oTxGvaV+pAIhQvdh225ytZIba+3YmcrzKq/rcB5/yGrzQE\nWgnB38mSmSk+mWq/yiFgsAJxGpi/N8CyeK62wds6Rn2NJQqvlZbkiBtVgBpHfVXUl94z6L\nZaWVIwoRgCX0GLAAABAQDRAm37JGznWEnVto8C3jxMAiBWca7sdWm+N77GJKRtdiXHNkiZ\nxOZ26XMYE4OMUV7qsdqBl7MzqXVly+FJNtISb+RxhIuKgyFP+fNQ0rtuAE15APi1eqp+r8\nJosZaqGmDfRcrjZFFXuU4irmjpyeLZO8hykwRkaVDqzPR8dfBan3Uy/CkjqLPlqnE2kcGD\niiee6oOE7ydtHkKyefIjBBVSqblgmRteqluSY+pA2lnt3f7qCZ2is+xALSj1U/Kzz628XU\nmHDfJko5V3sgT9X+aPyXvTrPgWKSbnVvbOt3SuFBwjhiF5Tqs+/qGSQFpNYePk5qLhvqTO\nKzQYv/Uj2IgnAAAAGWhhc2hpdG9ATWFjQm9vay1Qcm8ubG9jYWwB\n-----END OPENSSH PRIVATE KEY-----\n"
ADD ./run.sh /root/
RUN chmod a+x /root/run.sh && \
    mkdir /root/.ssh

CMD [ "/root/run.sh" ]
```


## 3.run.shで出力してあげる

```
#!/bin/bash

echo $key |sed -z 's/\\n/\n/g' >/root/.ssh/id_ras

sleep 1500
```

## 4.ビルドして確認

```
docker build -t test .
docker exec -it test /bin/bash

>cd /root/.ssh
>cat id_ras
```

参考：https://orebibou.com/ja/home/201607/20160714_003/