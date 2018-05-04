#PlistBuddy学习笔记
PlistBuddy是Mac自带的专门解析plist的小工具，由于PlistBuddy并不在Mac默认的Path里，所以需要使用绝对路径来引用这个工具。

数据类型:
string, array, dict, bool, real, integer, date, data

命令格式:
    Help - Prints this information
    Exit - Exits the program, changes are not saved to the file
    Save - Saves the current changes to the file
    Revert - Reloads the last saved version of the file
    Clear [<Type>] - Clears out all existing entries, and creates root of Type
    Print [<Entry>] - Prints value of Entry.  Otherwise, prints file
    Set <Entry> <Value> - Sets the value at Entry to Value
    Add <Entry> <Type> [<Value>] - Adds Entry to the plist, with value Value
    Copy <EntrySrc> <EntryDst> - Copies the EntrySrc property to EntryDst
    Delete <Entry> - Deletes Entry from the plist
    Merge <file.plist> [<Entry>] - Adds the contents of file.plist to Entry
    Import <Entry> <file> - Creates or sets Entry the contents of file

查看帮助
```
$/usr/libexec/PlistBuddy --help
```

打印plist文件
```
$/usr/libexec/PlistBuddy -c "print" info.plist
```

添加普通字段
```
$/usr/libexec/PlistBuddy -c 'Add :Version string 1.0' info.plist
```

添加数组;先添加key值，再添加value值。
```
$/usr/libexec/PlistBuddy -c 'Add :Application array' info.plist

$/usr/libexec/PlistBuddy -c 'Add :Application: string app1' info.plist
$/usr/libexec/PlistBuddy -c 'Add :Application: string app2' info.plist
```

添加字典；先添加key值，再添加value值。
```
$/usr/libexec/PlistBuddy -c 'Add :Person dict' info.plist

$/usr/libexec/PlistBuddy -c 'Add :Person:Name string yansu' info.plist
$/usr/libexec/PlistBuddy -c 'Add :Person:sex string boy' info.plist
```

打印字段相应的值

```
$/usr/libexec/PlistBuddy -c 'Print :Person' info.plist
```
在array中根据下标打印某个特定的值

```
$/usr/libexec/PlistBuddy -c 'Print ：Application:2' info.plist
```

删除字段相应的值

```
$/usr/libexec/PlistBuddy -c 'Delete :Version' info.plist
```

修改某个字段相应的值

```
$/usr/libexec/PlistBuddy -c 'Set :Application:1 string "thi is app1"' info.plist
```

