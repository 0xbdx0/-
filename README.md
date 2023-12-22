# 基于室友发签到码的对分易自动签到



 

尽管该程序能帮你大多数忙，但是还是又几个缺点的

1：该程序可能回因为cookies生命周期而失效，故而我们需要定期修改cookies

2：studentid可能因为数据库更改而失效，故而我们也需要定期检查studentid

3：在使用该程序时，需要提前把微信的页面置顶，且要把接受签到码的微信聊天群置顶（重点）







在某次早上不想去上早上因为不想去上早自习想睡觉的时候，又想睡觉，又又怕室友发签码过期。故而做出此小程序，在使用改小程序之前需要先改几个小程序内的参数：

1：cookies与studentid

在使用改程序之前，我们需要先设置一下cookies与studentid





cookies与studentid获取教程：

第一步：先进行网页版[对分易](https://www.duifene.com/)的登录：





第二步：随便进入一个班级,并点击考勤：





第四步：点击签到：







第三步：进入控制面板（chrom浏览器为f12）：







第五步：点击Checkin.ashx包:





在这里你就可以从headers与payload找到你的studentid与cookies了









最后将该cookies与studentid复制粘贴进去就可以愉快的使用了

 

该程序需要的库：

powershell：

```powershell
pip install wxauto time requests

```

 

