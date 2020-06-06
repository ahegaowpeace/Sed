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
