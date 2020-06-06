## やりたいこと
Digの結果について整形したい。WHENを含むレコードが存在する。digコマンドの結果毎のブロックを時刻を元に昇順や降順でソートしたい。
## testfile
大元ファイル
## failed
```
$ cat testfile | sed -n 'N;N;N;N;/.*DiG.*\n.*server.*\n.*global.*\n.*reached.*/p'
```
## success
```
$ cat testfile | sed 'N;N;N;N;/.*DiG.*\n.*server.*\n.*global.*\n.*reached.*/d'
```
