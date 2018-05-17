# IPFS の学習

## 目的
１サービスでファイルを持たずネットに永続的に保存したい。  
通常だと、サービスが停止した場合に画像などが参照不可になる  
これを解決したい。

## 環境
- mac 10.13.4
- go 1.9.4
- ipfs v0.4.14

## セットアップ

```
go get -u github.com/ipfs/ipfs-update  # インストール
ipfs-update versionsipfs # ダウンロード可能なすべてのバージョンをリストしています。
ipfs-update install v0.4.14 # ダウンロード可能なすべてのバージョンをリストしています。
ipfs init # ノードのピアIDを作成する。
```

## 追加

```
$ echo 'hoge?' > hoge.txt
$ ipfs add hoge.txt
added QmPYU4MjDvkygCyUoo4wrNqaHKo8Z8VUsukaugXgEquBqv hoge.txt
```
## 確認

```
$ ipfs cat QmPYU4MjDvkygCyUoo4wrNqaHKo8Z8VUsukaugXgEquBqv
hoge?
```

```
$ ipfs object get QmPYU4MjDvkygCyUoo4wrNqaHKo8Z8VUsukaugXgEquBqv
{"Links":[],"Data":"\u0008\u0002\u0012\u0006hoge?\n\u0018\u0006"}
```

## 取り出し
```
$ ipfs cat QmPYU4MjDvkygCyUoo4wrNqaHKo8Z8VUsukaugXgEquBqv > hoge.txt
```

## demon起動
IPFSのノードが立ち上がり追加されたものが見れる
```
ipfs daemon
```

## demon起動(クローズド)

```
ipfs daemon --offline
```

```
ipfs swarm peers # Errorになる
```
### webui
daemonを立ち上げた状態で[webul](http://127.0.0.1:5001/webui)にアクセス


### http

http:/127.0.0.1:8080/ipfs/{hash}

`http:/127.0.0.1:8080/ipfs/QmW2WQi7j6c7UgJTarActp7tDNikE4B2qXtFCfLPdsgaTQ/cat.jpg`  
などでアクセスできる  
猫が表示されるはず

## Naming
変更した場合に一位の名前でアクセスできるようになる。  

```
$ ipfs name publish QmQLB8nMQuwtn9pEqNek5uJR1zx6yenctAucod9hZkVbRy
Published to QmafzekqHPiM25SZcXdkjvzNAPqJuU8dCLZqPqWaWXAHaH: /ipfs/QmQLB8nMQuwtn9pEqNek5uJR1zx6yenctAucod9hZkVbRy
```
アクセスは/ipns/でアクセス最後に/も忘れずに

`http:/127.0.0.1:8080/ipns/QmafzekqHPiM25SZcXdkjvzNAPqJuU8dCLZqPqWaWXAHaH/`

## PIN　（キャッシュを消さない用にする）
これでGCの対象にならないっぽい
```
ipfs pin add QmQLB8nMQuwtn9pEqNek5uJR1zx6yenctAucod9hZkVbRy
```

## 参考URL
- [本家](https://ipfs.io/docs/getting-started/)
- [IPFS入門 : 新たなP2Pハイパーメディア分散プロトコル](https://postd.cc/an-introduction-to-ipfs/)
- [テスト環境用にクローズドなIPSFノードを立ち上げる方法](https://qiita.com/biga816/items/faf919ffba53e525442c)
- [#blockchain_train_journal IPFSを理解する【翻訳】](https://kotet.github.io/2017/11/09/getting-to-know-ipfs.html)
- [IPFS](https://wiki.archlinux.jp/index.php/IPFS)

## 触ってみた感想
- BitTorrentなどに似てるので参照が少ない物などはアクセスがほぼできなくなる？
- (2018/05/17)の段階で https://ipfs.io/ipfs/$hash これで一度もレスが帰ってきた事がない・・・
- P2PでのNodeなのでAWS等の通信料課金だとそれなりのお金が飛ぶ
