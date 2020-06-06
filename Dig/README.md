## やりたいこと
Digの結果について整形したい。WHENを含むレコードが存在する。digコマンドの結果毎のブロックを時刻を元に昇順や降順でソートしたい。
## testfile
大元ファイル
```
$ cat testfile
; <<>> DiG 9.11.4-P2-RedHat-9.11.4-16.P2.el7_8.6 <<>> @10.0.10.123 hoge.com A
; (1 server found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 22584
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 1, ADDITIONAL: 2

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
;; QUESTION SECTION:
;hoge.com.			IN	A

;; ANSWER SECTION:
hoge.com.		86400	IN	A	172.16.0.1

;; AUTHORITY SECTION:
hoge.com.		86400	IN	NS	ns.hoge.com.

;; ADDITIONAL SECTION:
ns.hoge.com.		86400	IN	A	172.16.0.2

;; Query time: 3 msec
;; SERVER: 10.0.10.123#53(10.0.10.123)
;; WHEN: 水  6月 03 07:18:55 UTC 2020
;; MSG SIZE  rcvd: 86


; <<>> DiG 9.11.4-P2-RedHat-9.11.4-16.P2.el7_8.6 <<>> @10.0.10.123 hoge.com A
; (1 server found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 2958
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 1, ADDITIONAL: 2

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
;; QUESTION SECTION:
;hoge.com.			IN	A

;; ANSWER SECTION:
hoge.com.		86400	IN	A	172.16.0.1

;; AUTHORITY SECTION:
hoge.com.		86400	IN	NS	ns.hoge.com.

;; ADDITIONAL SECTION:
ns.hoge.com.		86400	IN	A	172.16.0.2

;; Query time: 3 msec
;; SERVER: 10.0.10.123#53(10.0.10.123)
;; WHEN: 水  6月 03 07:18:56 UTC 2020
;; MSG SIZE  rcvd: 86
```
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
