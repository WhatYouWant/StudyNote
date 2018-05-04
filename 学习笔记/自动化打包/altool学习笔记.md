#使用altool上传ipa
使用代码，解决手动上传ipa的问题。

##准备
altool工具路径
```
/Applications/Xcode.app/Contents/Applications/Application\ Loader.app/Contents/Fraeworks/ITunesSoftwareService.framework/Versions/A/Support/altool
```
可以在~/.bash_profile中设置别名，以省去每次输入这么一长串路径的麻烦：

```
alias altool='/Applications/Xcode8.1.app/Contents/Applications/Application\ Loader.app/Contents/Frameworks/ITunesSoftwareService.framework/Versions/A/Support/altool'

source ~/.bash_profile
```

##1.验证ipa

```
altool -v -f /xxx/xxx.ipa -u xxx@xx.com -p xxx -t ios
```
附：/xxx/xxx.ipa为ipa路径 xxx@xx.com为用户名 xxx为密码
成功结果：
```
No errors validating archive at /xxx/xxx.ipa
```
错误结果:

```
*** Error: Unable to validate archive '/Users/kimilin/Downloads/BoHong.ipa': ( "Error Domain=ITunesConnectionOperationErrorDomain UserInfo={ NSLocalizedRecoverySuggestion=Unable to process app at this time due to a general error, NSLocalizedFailureReason=iTunes Store operation failed. }" ... 
```
也可以在命令最后加上 --output-format xml，可以获得plist形式的输出。
##上传ipa

```
altool --upload-app -f /xxx/xx.ipa -t ios -u xxx@xx.com -p xxx
```


