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
## 答え(成功結果だけ.失敗結果は時刻取れないし仕方ない...)
```
cat testfile | sed -e '/^$/d' -e '/.*DiG.*/i\\n' | sed -e '/^$/{N;/^\n$/D}' | sed -e '1d' | awk 'BEGIN{RS="";FS="\n"}{a[NR]=$0}END{for(i=NR; i>1; i--){print a[i] "\n"}}'
```
