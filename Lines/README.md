```
$ cat testcss
.form {
	font-size: 40px;
}
$ cat testcss | sed 'N;N;s/\(\.form {\n.*font-size: \).*\n}/\120px;\n}/'
.form {
	font-size: 20px;
}
```
