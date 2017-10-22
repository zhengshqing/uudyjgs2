# v1.7更新记录

| 时间       | 修改细节                                     | 修改者  |
| :------- | :--------------------------------------- | ---- |
| 20170214 | 更新接口: <br>connect_start uid_array字段改成stu_id_array<br>新增接口:<br>pad_unbind | neo  |
| 20170215 | 新增板绑定学生id的状态推送<br>exercise_start的uid_array改成stu_id_array | neo  |
| 20170220 | 板在线状态更新和提交答题更新的推送接口中，uid改成stu_id         | neo  |
| 20170221 | 播放器相关的接口，请求与返回均去掉了uid,只需要传stu_id         | neo  |
| 20170222 | 开始上课接口connect_start改成lesson_start,增加结束上课lesson_end | neo  |
| 20170228 | 增加答题卡推送                                  | neo  |
| 20170415 | 增加获取当前绑定在线的板                             | neo  |


# 请求

## 请求参数说明

| 变量           | 类型     | 说明                                       |
| ------------ | ------ | ---------------------------------------- |
| type         | string | 接口类型                                     |
| state        | string | 接口状态                                     |
| uid          | string | 板的id                                     |
| uid_array    | json   | 板的id数组，和stu_id一起，即"uid": stu_id为一对       |
| exe_type_id  | int    | 板交互的模式，0草稿，1互动，2作业                       |
| exe_set_id   | int    | 上课id                                     |
| exe_kind_id  | int    | 互动类型，1选择题，2判断题，3书写题 ,4 随堂练习（答题卡识别）       |
| exe_id       | int    | 互动id                                     |
| stu_id       | int    | 学生id,和板id绑定                              |
| page_number  | int    | 请求加载第几页的书写数据                             |
| page_count   | int    | 服务端返回的总页数                                |
| page_current | int    | 服务端返回的当前页                                |
| content      | json   | 返回内容                                     |
| answer       | int    | 板提交的按键，其中选择题为1,2,3,4(对应ABCD),判断题为5,6(对应对和错) |



## 开始上课

#### 请求参数

```
state: "lesson_start" 
stu_id_array:[567,550]
exe_type_id:0:草稿
            1:互动
            2:作业
exe_set_id:xxx->开始上课的课堂id,草稿默为0
```
#### 调用样例
```
{"state":"lesson_start","exe_type_id":1,"exe_set_id":376,"stu_id_array":[567,550]}
```

#### 返回板绑定状态接口
```

type:"online"
"content":"[{"stu_id":567,"uid":"34348a49"},{"stu_id":550,"uid":""}]"->uid为空，没有绑定板。不为空，则已绑定板，板在线
```
#### 返回结果样例
```
{"content":"[{"stu_id":567,"uid":"34348a49"},{"stu_id":550,"uid":""}]","type":"online"}
```

## 开始互动

#### 请求参数

```
state: "exercise_start"                       
exe_type_id:0:draft     草稿
            1:interact  互动
            2:homework  作业
exe_kind_id:1:选择题
            2:判断题
            3.书写题
            4.随堂练习
exe_id:xxx->正在做的互动的id
```

#### 调用样例
```
{"state":"exercise_start","exe_type_id":1,"exe_id":882,"exe_kind_id":1}
```

#### 返回接口

```
N/A
```

## 结束互动

#### 请求参数

```
state: "exercise_end" 
```
#### 调用样例
```
{"state":"exercise_end"}
```

#### 返回结果
```
N/A
```
## 结束上课

#### 请求参数

```
state: "lesson_end" 
```
#### 调用样例
```
{"state":"lesson_end"}
```

#### 返回结果
```
N/A
```

## 打开播放器

#### 请求参数
```
state:"player_open"
exe_type_id:0:draft
            1:interact
            2:homework"	
exe_set_id:xxx 
exe_id:xxx	        
stu_id:xxx		
```
#### 调用样例
```
{"state":"player_open","exe_type_id":1,"exe_id":886,"stu_id":1566,"exe_set_id":0}
注：exe_set_id不为0的话表示正在上课，则exe_type_id一定不能为0！
```

#### 返回播放器当前书写状态接口
```

type:"player"
state:"new_page"
stu_id:xxx
send_time:xxxxxxxxxxxxx ->13位发送数据的时间戳
send_seq:xxx->发送的顺序
page_count:xx ->书写数据总页数
page_current:xxx->播放器打开的当前页
data_page:xxx ->书写数据过大时拆分成几条发送，data_page为1时，播放器刷新并加载，>1时播放器直接加载书写数据
content:"[{"cmd:"pen/penup",time:xxxxxxxxxxxxx,po:"{x:xxx,y:yyy}"}]"->返回书写数据:                                                                                     cmd:pen/penup->书写/非书写
                                                                      time:书写时的时间
                                                                      po:书写坐标
                                                                      x:x坐标
                                                                      y:y坐标	
```
#### 返回结果样例
```
没有书写数据
{"content":"[]","page_count":0,"page_current":0,"state":"new_page","data_page":1,"type":"player","stu_id":1566}


有书写数据
{"content":"[{"cmd":"pen","po":{"x":-3093,"y":2133},"time":1476687884947}]","page_count":1,"page_current":1,"state":"new_page","data_page":1,"type":"player","stu_id":1566}

书写数据过大时
第一条
{"content":"[{"cmd":"pen","po":{"x":-3093,"y":2133},"time":1476687884947}]","page_count":1,"page_current":1,"state":"new_page","data_page":1,"type":"player","stu_id":1566}

第二条
{"content":"[{"cmd":"pen","po":{"x":-3093,"y":2133},"time":1476687884947}]","page_count":1,"page_current":1,"state":"new_page","data_page":2,"type":"player","stu_id":1566}
以此类推
```


## 关闭播放器
#### 请求参数
```
state: "player_closed" 
stu_id:xxx
```
#### 调用样例
```
{"state":"player_closed","stu_id":756}
```

#### 返回结果
```
N/A
```

## 播放器翻页时加载指定一页书写数据（注，翻到最后一页时是发送player_open请求）
#### 请求参数

```
state:"player_specified_page_data"
exe_type_id:0:draft
            1:interact
            2:homework"	
exe_set_id:xxx
exe_id:xxx
stu_id:xxx
page_number:xxx ->播放器当前页数
```
#### 调用样例
```
{"state":"player_specified_page_data","exe_type_id":1,"exe_set_id":379,"exe_id":886,"stu_id":885,"page_number":0}
```

#### 返回播放器书写历史数据接口
```
content:"[{"cmd:"pen/penup",time:xxxxxxxxxxxxx,po:"{x:xxx,y:yyy}"}]"->返回书写数据:                                                                                     cmd:pen/penup->书写/非书写
                                                                      time:书写时的时间
                                                                      po:书写坐标
                                                                      x:x坐标
                                                                      y:y坐标	
page_count:xxx->书写的总页数
page_current:xxx->播放器打开的当前页
state:"specified_page"
send_time:xxxxxxxxxxxxx ->13位发送数据的时间戳
send_seq:xxx->发送的顺序
data_page:xxx ->书写数据过大时拆分成几条发送，data_page为1时，播放器刷新并加载，>1时播放器直接加载书写数据
type:"player"
stu_id:xxx		
```
#### 返回结果样例
```
有书写数据时
{"content":"[{"cmd":"pen","po":{"x":-3093,"y":2133},"time":1476687884947},{"cmd":"pen","po":{"x":-3091,"y":2120},"time":1476687884963}]","page_count":2,"page_current":0,"state":"specified_page","type":"player","stu_id":885}

书写数据过大时
第一条
{"content":"[{"cmd":"pen","po":{"x":-3093,"y":2133},"time":1476687884947},{"cmd":"pen","po":{"x":-3091,"y":2120},"time":1476687884963}]","page_count":2,"page_current":0,"data_page":1,state":"specified_page","type":"player","stu_id":885}
第二条：
{"content":"[{"cmd":"pen","po":{"x":-3093,"y":2133},"time":1476687884947},{"cmd":"pen","po":{"x":-3091,"y":2120},"time":1476687884963}]","page_count":2,"page_current":0,"data_page":2,state":"specified_page","type":"player","stu_id":885}
以此类推
```



# 解除板绑定

#### 请求参数

```
state: "pad_unbind" 
stu_id_array:[567,550]
```
#### 调用样例
```
{"state":"pad_unbind","stu_id_array":[567,550]}
```

#### 返回板在线状态接口
```

type:"online"
"content":"[{"stu_id":567,"uid":""},{"stu_id":550,"uid":""}]"->uid为空，没有绑定板。不为空，则已绑定板，板在线
```
#### 返回结果样例
```
{"content":"[{"stu_id":567,"uid":""},{"stu_id":550,"uid":""}]","type":"online"}
```


# 获取当前绑定在线的板

#### 请求参数

```
state: "get_bound_pad" 
stu_id: 567
```

#### 调用样例

```
{"state":"get_bound_pad","stu_id":567}
```
#### 查询书写缓存接口

```
{"state":"check_write_cache","stu_id":567}
```

#### 返回结果样例

```
{"content":"[{"stu_id":567,"write_cache_exist":0/1}]","type":"check_write_cache"}
```


#### 返回板在线状态接口

```
type:"online"
"content":"[{"stu_id":550,"uid":""}]"->uid为空，没有绑定板。不为空，则已绑定板，板在线
```

#### 返回结果样例

```
{"content":"[{"stu_id":567,"uid":""}]","type":"online"}
```





# 服务端主动推送

## 板在线状态更新

#### 返回参数
```
type:"online"
content:["stu_id":xxx,"uid":""]离线
/content:["stu_id":xxx,"uid":"34348163"]在线
```
#### 返回样例

```
离线：{"content":"[{"stu_id":xxx,"uid":""}]","type":"online"}
在线：{"content":"[{"stu_id":xxx,"uid":"34348163"}]","type":"online"}
```

## 板提交答案更新
#### 返回参数
```
type:"online"
content:["answer":x,"stu_id":xxx]
```
#### 返回样例

```
{"content":"[{"answer":2,"stu_id":1566}]","type":"online"}
```

## 板书写状态更新
#### 板书写推送
注：只有在播放器的最新一页，即page_count==page_current时才会推送
返回参数
```
content:"[{"cmd:"pen/penup",time:xxxxxxxxxxxxx,po:"{x:xxx,y:yyy}"}]"->返回书写数据:                                                                                     cmd:pen/penup->书写/非书写
                                                                      time:书写时的时间
                                                                      po:书写坐标
                                                                      x:x坐标
                                                                      y:y坐标	
state:"live_page"
send_time:xxxxxxxxxxxxx ->13位发送数据的时间戳
send_seq:xxx->发送的顺序
type:"player"
stu_id:xxx	

```
#### 返回样例
```
书写
{"content":"[{"cmd":"pen","po":{"x":-1920,"y":4713},"time":1476688782586}]","state":"live_page","type":"player","stu_id":885}

非书写：
{"content":"[{"cmd":"penup","po":{"x":-1948,"y":4690},"time":1476688782604}]",,"state":"live_page","type":"player","stu_id":885}
```

#### 板换页
```
page_count:xxx->书写的总页数
page_current：xxx->播放器打开的当前页	
state:"change_page"
type:"player"
stu_id:xxx		
```

#### 返回样例
```
{"page_count":3,"page_current":3,"state":"change_page","type":"player","stu_id":885}
```

## 板上课状态推送
单个板登陆普通用户模式（web或者app)时，此页面或者app会收到其中的板上课状态的更新，以下事件会触发该推送
- 板在上课，登陆了普通用户模式
- 板登陆普通用户模式，然后板开始了上课
- 板登陆普通用户模式，板在上课，然后开始了互动
- 板登陆普通用户模式，板在上课，然后结束了互动
- 板登陆普通用户模式，板在上课，然后结束了上课

#### 上课状态推送
```
"type":"lesson"
"content":"[{"class_id":xxx, ->班级id
             "class_title":"xxx",  ->班级标题
             "exe_id":xxx,
             "exe_set_id":xxx,
             "exe_type_id":xxx,
             "on_lesson":0/1,->0下课 1上课
             "stu_id":xxx
    
}]

```

#### 返回样例
```
上课时（附带上课信息，和班级id,班级title)
{"content":"[{"class_id":81,"class_title":"压力测试班（最新）","exe_id":1313,"exe_set_id":510,"exe_type_id":1,"on_lesson":1,"stu_id":0}]","type":"lesson"}

下课时
{"content":"["on_lesson":0}]","type":"lesson"}


```

## 板书写状态推送
#### 返回参数
```
"type":"player"
"state":"pad_write_stats"
"content":"[{"is_write":1,"stu_id":xxx}]"->"is_write":1表示正在书写
```

#### 返回样例

```
{"content":"[{"is_write":1,"stu_id":xxxx}]","state":"pad_write_status","type":"player"}
```

## 板答题卡识别推送
#### 返回参数
```
"type":"recognition"
"content":[{"recognized_answer":"[{"answer":1,"exe_seq":1,"mark":1,"time":1492569060779}]","stu_id":1566}]->answer:1-5 A-E, exe_seq第几题, mark:1选中，0取消选择
```

#### 返回样例

```
{"content":[{"recognized_answer":"[{"answer":1,"exe_seq":1,"mark":1,"time":1492569060779}]","stu_id":1566}],"type":"recognition"}
```

## 书写笔画数推送(pen up 时推送一次表示书写了一笔)

#### 返回参数

```
"type":"player"
"state":"write_count"
"content":"[{"stu_id":xxxx}]"
```
#### 返回样例
```

```
## 板绑定学生id状态推送
#### 返回参数
```
"type":"online"
"content":"[{"stu_id":xxx, "uid":"xxxx"},{"stu_id":xxx, "uid":"xxxx"} ]"
```
#### 返回样例
```
{"content":"[{"stu_id":567,"uid":"34348a49"},{"stu_id":550,"uid":""}]","type":"online"}
```

# 同步

## 发送
```
连接注册
{"type":"syncAction","acc":"chao"}  acc:帐号

无返回

```
```
发送同步信息
{
        type: "syncAction",
        acc: 帐号,
        content: {
            id: connection.id, 连接ID
            res: 见testing模块接口返回(直接塞)
            chart: 见报表接口数据(直接塞)
        }
}

返回原样, 判断content.id等于自己的connection.id 忽略

注意content为false 课堂结束发送content为false的同步信息
同步时res 为testing模块接口 返回 , chart为null
报表时res为null, chart为{"chart_type":"answers|answers_times|answers_ranking","data":报表接口数据}
```

## 接收
```
{
        type: "action",
        content: {
            id: connection.id, 连接ID
            res: null | 见testing模块接口(直接塞)
            chart:  null | {"chart_type":"","data":见报表接口数据} 
        }
}
注意content为false 即课堂已经结束
同步时res 为testing模块接口 返回 , chart为null
报表时res为null, chart为{"chart_type":"answers|answers_times|answers_ranking","data":报表接口数据} 

```

### 样例
#### 状态
```
{"type":"action","content":{"id":"O-s5-2NuhwrYGLh2AAUr","res":{"testing":542,"testing_stat":"N","testing_start_time":"2016-11-12 12:47:01","index":39,"interact":1476,"interact_stat":"Y","interact_setting_completed":true,"interact_type":"2","interact_type_title":"判断题","interact_time":57112,"doing_exercise_set_id":0,"doing_exercise_id":0,"doing_exercise_set":[],"doing_exercise_index":0,"student_list":[{"id":"2095","gender":"1","firstname":"341234ef","lastname":"","device_uid":"341234ef","student_id":"llc3412345","online":false,"answered":false},{"id":"2094","gender":"1","firstname":"ef123456","lastname":"","device_uid":"ef123456","student_id":"llc123456","online":false,"answered":false}]},"chart":null}}
```
#### 报表
```
{"type":"action","content":{"id":"O-s5-2NuhwrYGLh2AAUr","res":null,"chart":{"chart_type":"answers","data":{"stat":0,"msg":"","data":{"true_love":"5","option":{"5":"对","6":"错"},"answered":0,"student_count":2,"chart":{"5":0,"6":0}}}}}}
```
```
{"type":"action","content":{"id":"O-s5-2NuhwrYGLh2AAUr","res":null,"chart":{"chart_type":"answers_ranking","data":[]}}}
```
```
{"type":"action","content":{"id":"O-s5-2NuhwrYGLh2AAUr","res":null,"chart":{"chart_type":"answers_times","data":{"stat":0,"msg":"","data":{"true_love":0,"option":["03:57:58","07:55:56","11:53:54","15:51:52"],"answered":0,"student_count":2,"chart":{"03:57:58":0,"07:55:56":0,"11:53:54":0,"15:51:52":0}}}}}}
```