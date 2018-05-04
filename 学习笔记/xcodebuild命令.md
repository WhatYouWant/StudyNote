#xcodebuild命令
CI 也就是持续集成，是一种软件开发实践。通过自动化构建来将软件系统集成后尽早交付测试来发现问题。

编译workspace
```
	xcodebuild -workspace $projectName.xcworkspace -scheme $projectName -configuration $buildConfig clean build SYMROOT=$buildAppToDir
```
编译project
```
	xcodebuild -target $projectName -configuration $buildConfig clean build SYMROOT=$buildAppToDir
```

打包ipa
```
xcrun -sdk iphoneos PackageApplication -v $appDir/$projectName.app -o $appDir/$ipaName.ipa #将app打包成ipa
```

