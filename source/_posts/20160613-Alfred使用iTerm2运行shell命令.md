---
title: Alfred使用iTerm2运行shell命令
date: 2016-06-13 17:52:13
tags: alfred
---

alfred默认是使用MacOSX自带的Terminal运行shell命令,没有用iTerm2,不爽。。。
上网搜了下，有些同学说将iTerm2设置成默认的终端应用就行，我设置之后还是不行，
于是自行google之，搜到了一个脚本，替换alfred的custom脚本之后就可以了。
DONE:)

注意：需要将iTerm.app移动到/Applications目录下

##老的脚本
```
on alfred_script(q)
    tell application "Terminal"
        activate
        do script q
    end tell
end alfred_script
```


##新的脚本
```
-- This is v0.6 of the custom script for AlfredApp for iTerm 2.9+
-- Please see https://github.com/stuartcryan/custom-iterm-applescripts-for-alfred/
-- for the latest changes.

-- Please note, if you store the iTerm binary in any other location 
    than the Applications Folder
-- please ensure you update the two locations below 
    (in the format of : rather than / for folder dividers)
-- this gets around issues with AppleScript not handling things well 
    if you have two iTerm binaries on your system... which can happen :D

on alfred_script(q)
    if application "iTerm2" is running or application "iTerm" is running then
        run script "
            on run {q}
                tell application \":Applications:iTerm.app\"
                    activate
                    try
                        select first window
                        set onlywindow to false
                    on error
                        create window with default profile
                        select first window
                        set onlywindow to true
                    end try
                    tell the first window
                        if onlywindow is false then
                            create tab with default profile
                        end if
                        tell current session to write text q
                    end tell
                end tell
            end run
        " with parameters {q}
    else
        run script "
            on run {q}
                tell application \":Applications:iTerm.app\"
                    activate
                    try
                        select first window
                    on error
                        create window with default profile
                        select first window
                    end try
                    tell the first window
                        tell current session to write text q
                    end tell
                end tell
            end run
        " with parameters {q}
    end if
end alfred_script
```
