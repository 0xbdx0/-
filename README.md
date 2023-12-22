\# 基于室友发签到码的对分易自动签到



 

尽管该程序能帮你大多数忙，但是还是又几个缺点的

1：该程序可能回因为cookies生命周期而失效，故而我们需要定期修改cookies

2：studentid可能因为数据库更改而失效，故而我们也需要定期检查studentid

3：**在使用该程序时，需要提前把微信的页面置顶，且要把接受签到码的微信聊天群置顶（重点）**

**![img](https://img2023.cnblogs.com/blog/3359196/202312/3359196-20231222130832324-1108149189.png)**





在某次早上不想去上早上因为不想去上早自习想睡觉的时候，又想睡觉，又又怕室友发签码过期。故而做出此小程序，在使用改小程序之前需要先改几个小程序内的参数：

1：cookies与studentid

**在使用改程序之前，我们需要先设置一下cookies与studentid**

![img](https://img2023.cnblogs.com/blog/3359196/202312/3359196-20231222130154572-384106400.png)



cookies与studentid获取教程：

第一步：先进行网页版[对分易](https://www.duifene.com/)的登录：

![img](https://img2023.cnblogs.com/blog/3359196/202312/3359196-20231222130214043-495787535.png)



第二步：随便进入一个班级,并点击考勤：

![img](https://img2023.cnblogs.com/blog/3359196/202312/3359196-20231222130228730-830566184.png)



第四步：点击签到：

![img](https://img2023.cnblogs.com/blog/3359196/202312/3359196-20231222130409964-200354383.png)





第三步：进入控制面板（chrom浏览器为f12）：

![img](https://img2023.cnblogs.com/blog/3359196/202312/3359196-20231222130427168-631488931.png)





第五步：点击Checkin.ashx包:

![img](https://img2023.cnblogs.com/blog/3359196/202312/3359196-20231222130435799-1157381829.png)



在这里你就可以从headers与payload找到你的studentid与cookies了

![img](https://img2023.cnblogs.com/blog/3359196/202312/3359196-20231222130446939-1549558100.png)



![img](https://img2023.cnblogs.com/blog/3359196/202312/3359196-20231222130453946-130725990.png)



最后将该cookies与studentid复制粘贴进去就可以愉快的使用了

 

该程序需要的库：

powershell：

\```powershell
pip install wxauto time requests

\```

微信版本： 3.7.6.xx
如果没有的话，可以在这个网站下载：
https://wechat-for-windows.en.uptodown.com/windows/download

代码：

```python
from wxauto import *
import requests
import re
import time
import json

wx = WeChat()
has_try = []
list_ = []
cookies = "待更改"#将你的cookies复制在分号内部
studentid = "待更改"#将你的studentid复制在分号内部

def send_request(str_):
    payload = { 
                    'action' : 'studentcheckin',
                    'studentid':'%s' % (studentid),
                    'checkincode': '%s' % (str_)
                }
    
    header = {"User-Agent":"Mozilla/5.0 (Linux; Android 6.0; Nexus 5 Build/MRA58N) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/116.0.0.0 Mobile Safari/537.36"
            ,"Connection":"keep-alive"
            ,"Cookie" : "%s" % (cookies)
            ,"Origin":"https://www.duifene.com"
            ,"Content-Type":"application/x-www-form-urlencoded; charset=UTF-8"
            }
    
    url = "https://www.duifene.com/_CheckIn/CheckIn.ashx"
    
    
    response = requests.post(url=url,data=payload,headers=header)
    response_list = response.json()
    print(response_list['msgbox'])
    if(response_list['msg'] == -4):
        time.sleep(5)
        has_try.pop()
        return 1
    return 0
        

def get_default_windows_messages():
    msgs = wx.GetAllMessage
    return msgs

def tried(str_msgs,str_):
    for i in str_msgs:
        if(str_ == i):
            return 0
        
    has_try.append(str_)
    return 1

def auto_checkin():


    wx.GetSessionList()

    msgs = get_default_windows_messages()
    for msg in msgs:
        if(re.match(r'\d{4}',msg[1])):
            list_.append(msg[1])

    times = len(list_) - 1

    while(times >= 0):
        flag = 1
        while(flag):
            if(tried(has_try,list_[times])):
                flag = send_request(list_[times])
                print(list_[times])
            else:
                flag = 0
            
        times -= 1
        



        

while(1):
    time.sleep(5)
    auto_checkin()
```

 
