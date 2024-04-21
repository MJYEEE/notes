# Linux下Screen的基本使用方式
---
screen相当于在命令行中开辟一个新的空间（直译为屏幕），你可以将任务放在这个新的空间上

## screen的状态
- Attached：表示当前screen正在作为主终端使用，为活跃状态。
- Detached：表示当前screen正在后台使用，为非激发状态。


## 新建screen
兴建一个名为name的screen
``` 
screen -S name
```
name可以省略，只剩下ID，没有名字。

``screen -S name``screen启动后，会创建一个窗口，并在其中打开一个系统默认的shell，一般都会是bash。所以你敲入命令screen之后，会立刻又返回到命令提示符，仿佛什么也没有发生似的，其实你已经进入Screen的世界了。


另外可以在screen命令之后加入你喜欢的参数，使之直接打开你指定的程序，例如``screen -S name bash run.sh``


## 进入screen
进入名为name的screen
```
screen -r name
```

## 删除screen
- 在当前screen下，按下Ctrl+D，删除该screen
- 在当前screen下，输入exit关闭screen
- 删除指定screen： ``screen -S name -X quit``
  

## 退出screen
在当前screen下，先按下Ctrl+A，再按下D，退出该screen，此时这个screen处于Detached状态

## 显示screen列表
```
screen -ls
```

## 进入screen
```
screen -r name
```

## 显示当前screen的id和name
```
echo $STY
```

---
## 使用screen恢复会话时出现There is no screen to be resumed matching错误解决办法
**原因**：后来发现是因为突然断网，虽然重新连接，但之前的screen还是处于打开状态（1个screen无法同时打开2次）,非正常退出状态，所以无法重新打开screen。
**解决办法**：首先使用``screen -d name(id)``,先退出screen，然后再重新进入。


## 进入screen后发现乱码
**原因**：编码问题。
**解决方法**：用``screen -U -S name``创建screen，用``screen -U -r screen``进入screen。