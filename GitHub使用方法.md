## GitHub使用方法

### 上传文件

使用命令行方法：

```
//向仓库上传文件
$ git init
$ git add .
$ git commit -m "some str"
$ git push origin master
```

```
//删除仓库文件
$ git rm -r [文件夹名称]   //删除文件夹
$ git rm [文件名称]        //删除文件
$ git commit -m "Delete some files"
$ git push origin [分支]   //比如master
```

 git remote add origin https://github.com/qyf381389362/qixiwangzhan.git

git clone https://github.com/qyf381389362/qixiwangzhan.git