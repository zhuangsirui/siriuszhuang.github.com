---
layout: post
title: 有用的shell脚本
description: shell使我们的挚友，它可以帮助我们完成很多事情。
category: blog
---

## 获取URL响应头

	curl -I example.com

Example

	$ curl -I www.baidu.com
	HTTP/1.1 200 OK
	Date: Wed, 10 Apr 2013 07:26:50 GMT
	Server: BWS/1.0
	Content-Length: 10310
	Content-Type: text/html;charset=utf-8
	Cache-Control: private
	Expires: Wed, 10 Apr 2013 07:26:50 GMT
	Set-Cookie: H_PS_PSSID=1459_1945_1788_2209; path=/; domain=.baidu.com
	Set-Cookie: BAIDUID=587EFAACE82D224F77F56E5701E927A1:FG=1; expires=Wed,
	10-Apr-43 07:26:50 GMT; path=/; domain=.baidu.com
	P3P: CP=" OTI DSP COR IVA OUR IND COM "
	Connection: Keep-Alive

## 删除文件到垃圾箱（$HOME/lost+found/）

很多时候，在命令模式下删除文件是一件危险的事情，直接`rm`的话，对于误删除就没办法了，将下面一行脚本放入`~/.bashrc`中，通过`del`来删除文件，可以自动将文件或者目录移动到`$HOME/lost+found/`文件夹下。

	function del () {
		trashDir="${HOME}/lost+found/"
		[[ -d ${trashDir} ]] || mkdir ${trashDir}
		mv $1 ${trashDir}
	}

## 修改文件名

很多时候，我们只希望修改文件的后缀，如果文件名称很长，或者在一个很深的目录的话，就会出现这样的情况：

	$ mv ~/path/to/my/self/file.php ~/path/to/my/self/file.rb

其实，只用加上`{}`，就可以方便的重命名了：

	$ mv ~/path/to/my/self/file.{php,rb}    # 修改后缀
	$ mv ~/path/to/my/self/{file,file2}.php # 修改文件名

同样的适用于`cp`

	$ cp ~/path/to/my/self/file.{php,rb}    # 修改后缀
	$ cp ~/path/to/my/self/{file,file2}.php # 修改文件名

## 只查看当前目录中的目录

	$ ls -d */

## 获取外网IP

	$ curl ifconfig.me

## 在项目中查找代码

有时候，我们会想要查出一个函数在哪些位置被调用过，如果不借用IDE的话，可以使用`grep`

	$ grep -rn 'public function test' protected/tests
	protected/tests/unit/AlbumTest.php:22:    public function testCreateAlbum() {
	protected/tests/unit/AlbumTest.php:36:    public function testDeleteAlbum() {
	protected/tests/unit/PhotoTest.php:33:    public function testCreatePhoto() {
	protected/tests/unit/PhotoTest.php:47:    public function testDeletePhoto() {
	protected/tests/unit/QuestionTest.php:24:    public function testCreate() {
	protected/tests/unit/UserModelTest.php:26:    public function testRegister() {

加上`-A`查看下文，`-B`查看上文，`-C`选项可以查看上下文。具体用法只能`man grep`了。
