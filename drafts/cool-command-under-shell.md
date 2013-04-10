## 有用的shell脚本

### 获取URL响应头

	wget --server-response --spider example.com

例子：

	$ wget --server-response --spider www.baidu.com
	Spider mode enabled. Check if remote file exists.
	--2013-04-10 15:10:39--  http://www.baidu.com/
	Resolving www.baidu.com... 220.181.111.147
	Connecting to www.baidu.com|220.181.111.147|:80... connected.
	HTTP request sent, awaiting response...
	  HTTP/1.1 200 OK
	  Date: Wed, 10 Apr 2013 07:10:41 GMT
	  Server: BWS/1.0
	  Content-Length: 10310
	  Content-Type: text/html;charset=utf-8
	  Cache-Control: private
	  Expires: Wed, 10 Apr 2013 07:10:41 GMT
	  Set-Cookie: H_PS_PSSID=1426_1945_1788_2209; path=/; domain=.baidu.com
	  Set-Cookie: BAIDUID=1D5D69149A0F0754D1D0A8636D34D0B2:FG=1; expires=Wed,
	10-Apr-43 07:10:41 GMT; path=/; domain=.baidu.com
	  P3P: CP=" OTI DSP COR IVA OUR IND COM "
	  Connection: Keep-Alive
	Length: 10310 (10K) [text/html]
	Remote file exists and could contain further links,
	but recursion is disabled -- not retrieving.

### 删除文件到垃圾箱（$HOME/lost+found/）

很多时候，在命令模式下删除文件是一件危险的事情，直接`rm`的话，对于误删除就没办法了，将下面一行脚本放入`~/.bashrc`中，通过`del`来删除文件，可以自动将文件或者目录移动到`$HOME/lost+found/`文件夹下。

	# delete file or directory to ~/lost+found {{{
	function del () {
		trashDir="${HOME}/lost+found/"
		[[ -d ${trashDir} ]] || mkdir ${trashDir}
		mv $1 ${trashDir}
	}
	# }}}
