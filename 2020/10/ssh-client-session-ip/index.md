# 取得 SSH Session Client 的 IP


用 SSH 連到遠端 Server 後，有時候需要取得自己的 IP 來做某些事 (比如說設定 `DISPLAY` 環境變數)

<!--more-->

可以用 `$SSH_CLIENT` 或 `$SSH_CONNECTION` 取得連線資訊

```shell
$ echo $SSH_CLIENT
> 172.16.1.100 10360 22
```

```shell
$ echo $SSH_CONNECTION
> 172.16.1.100 10360 172.16.1.200 22
```

只列出 Client IP

```shell
$ echo $SSH_CLIENT | awk '{ print $1}'
> 172.16.1.100
```

設定 DIPSLAY 環境變數

```shell
$ export DISPLAY=`echo $SSH_CLIENT | awk '{ print $1}'`:0.0
$ echo $DISPLAY
> 172.16.1.100:0.0
```

## Reference
- https://stackoverflow.com/questions/996231/find-the-ip-address-of-the-client-in-an-ssh-session
