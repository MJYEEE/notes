# Linux下Screen的基本使用方式
---
screen相当于在命令行中开辟一个新的空间（直译为屏幕），你可以将任务放在这个新的空间上

## 新建screen
兴建一个名为name的screen
``` 
screen -S name
```

## 进入screen
进入名为name的screen
```
screen -r name
```

## 删除screen
1.在当前screen下，按下Ctrl+D，删除该screen
2.screen -S name -X quit

## 退出screen
在当前screen下，先按下Ctrl+A，再按下D，退出该screen

## 显示screen列表
```
screen -ls
```
