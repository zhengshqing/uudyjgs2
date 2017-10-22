| 版本   | 功能列表                                     |
| ---- | ---------------------------------------- |
| 1.7  | 接口地址由test.ubanjiaoyu.com/api/v1b8/，改为：dev17.ubanjiaoyu.com/organize/api/  即：域名/学校子域名（默认organize）/api |

| 更新时间       | 更新内容                                     | 更新人    |
| ---------- | ---------------------------------------- | ------ |
| 2017-5-12  | 新增接口：9.8修改习题答案、12.19修改自定义随堂练习答案          | lims   |
| 2017-7-4   | 1.student/get_exercise_list_by_student增加拍照照片URL 2.report/interact_questions 增加拍照3.report/exercise_set_questions增加拍照照片URL4.testing/get_status增加拍照照片URL | VANS   |
| 2017-7-4   | report/upload_questions_title_img   上传照片接口 | vans   |
| 2017-7-10  | report/del_questions_title_img 删除图片接口    | vans   |
| 2017-7-11  | testing/interact_set中将true_love字段修改为correct_answer | Peyton |
| 2017-7-18  | 增加微课接口：17.1结束微课，17.2开始微课，17.3上传图片，17.4上传音频，17.5获取微课数据 | vans   |
| 2017-7-18  | 17.6 小程序微信登录检测                           | vans   |
| 2017-07-21 | 17.7 检测能否参赛17.8 点赞 17.9q取消点赞 17.10投票 17.11 获取视频广场列表17.12获取个人视频列表 17.13获取个人名片 17.14修改个人名片 17.15 获取某人的精彩视频17.16获取比赛列表17.17 删除微课 | vans   |
| 2017-07-26 | 12.24 设置曾经在线人数 12.25 获取曾经在线人数            | vans   |
| 2017-07-31 | 17.18 修改微课信息                             | vans   |
| 2017-08-17 | 4.9 获取个人信息                               | vans   |
| 2017-08-18 | 12.2查看课堂状态 增加字段'jyeoo_subject'代表当前开课科目，在菁优网支持才会出现12.1 开始互动参数增加       返回值doing_exercise_mode 增加状态 3、4                                                     12.16 开始随堂练习参数增加 | vans   |



## 0全局说明&约定

### 0.1 参数: 所有接口必须带token,（token在登录接口sign/in获取）

### 0.2 返回: 无特别说明下默认为json格式 {"msg":"", "stat":0, "data": "" }

```
   string msg: 返回文字性信息;
        int stat: 返回状态代码参考0.3

        以下文档只对 **data** 作说明;
        所有参数及返回均为utf-8编码http协议, 大小写敏感
```

### 0.3 返回状态说明

|  返回码  | 说明                             |
| :---: | :----------------------------- |
|  -2   | 推荐升级客户端                        |
|  -1   | 接口关闭                           |
|   0   | 成功                             |
|  403  |                                |
|  404  |                                |
|  500  |                                |
| 10000 | 缺少access_token或者access_token无效 |
| 10001 | invalid account                |
| 20000 | 缺少version_name参数               |
| 20001 | 没有该文件                          |

## 1通用

### 1.1登录

sign/in

请求

| 参数      | 类型     | 是否必须 | 说明                                     |
| ------- | ------ | ---- | -------------------------------------- |
| account | string | Y    | 账号（一般格式为:学校子域名@账号。如：route@12345678902） |
| passwd  | string | Y    | 密码                                     |

返回

```
{
    "token": "78a4811e2cce353a818b93a1b0e23668",//秘钥
    "school_title": "有优特攻队",//学校名称
    "school_subdomain": "chao", //学校子域名
    "id": 113, //用户ID
    "school_id": 2,
    "account": "@chao",
    "email": "",
    "mobile": "18520781391",
    "firstname": " 呆",//名称
    "lastname": "李",//姓氏
    "gender": 0,//性别：0代表女，1代表男
    "state": 99,//老师的状态码 -1:禁用, 0:未启用, 1:正常, 99:学校超级管理
    "sign_time": 1490601111 //登录的服务器时间戳
  }
```

### 1.2取得token身份资料

sign/token_check

请求

| 参数   | 类型   | 是否必须 | 说明   |
| ---- | ---- | ---- | ---- |
|      |      |      |      |

返回

```
{
    "school_id": 2,
    "school_title": "有优特攻队",
    "logo": "",
    "school_subdomain": "chao",
    "id": 113,
    "account": "@chao",
    "email": "",
    "mobile": "18520781391",
    "firstname": " 呆",
    "lastname": "李",
    "state": 99, //老师的状态码 -1:禁用, 0:未启用, 1:正常, 99:学校超级管理员
    "gender": 0, //性别：0代表女，1代表男
    "update_time": 1490601077, //最后活动的服务器时间戳
    "now": 1490601144 //现在的服务器时间戳
  }
```

### 1.3获取所有学校（不包含禁用学校）

sign/schools

请求

| 参数      | 类型     | 是否必须 | 说明        |
| ------- | ------ | ---- | --------- |
| keyword | string | N    | 搜索关键字，默认空 |

返回

```
[
  {
      "id": "31",
      "title": "新路由测试学校",
      "years": "3",
      "descript": "",
      "url": "",
      "phone": "13999999999",
      "address": "南山区高新园",
      "email": "",
      "logo": "",
      "state": "1",
      "subdomain": "route"
    },
    {
      "id": "2",
      "title": "有优特攻队",
      "years": "3",
      "descript": "**有优特攻队**",
      "url": "www.ubanjiaoyu.com",
      "phone": null,
      "address": null,
      "email": null,
      "logo": "",
      "state": "1",
      "subdomain": "chao"
    },
    ……
  ]
```

### 1.4获取单个学校

sign/school

请求

| 参数        | 类型     | 是否必须 | 说明    |
| --------- | ------ | ---- | ----- |
| subdomain | string | Y    | 学校子域名 |

返回

```
{
    "id": "31",
    "title": "新路由测试学校",
    "years": "3",
    "descript": "",
    "url": "",
    "phone": "13999999999",
    "address": "南山区高新园",
    "email": "",
    "logo": "",
    "state": "1",
    "subdomain": "route"
  }
```



## 2学校课程

### 2.1课程列表(不包含禁用课程)

course/rows

请求

| 参数      | 类型     | 是否必须 | 说明                    |
| ------- | ------ | ---- | --------------------- |
| keyword | string | N    | 搜索关键字                 |
| page    | int    | N    | 页码                    |
| state   | string | N    | 默认为null，为"all"时返回所有课程 |



返回

    {
        "total": 5,
        "count": 5,
        "page_index": 1,
        "page_size": 20,
        "page_count": 1,
        "result": [
          {
            "id": "15",
            "school_id": "2",
            "title": "数学",
            "state": "1"
          },
          {
            "id": "14",
            "school_id": "2",
            "title": "语文",
            "state": "1"
          }
        ]
      }

### 2.2课程列表（不分页）

course/all

请求

| 参数      | 类型     | 是否必须 | 说明   |
| ------- | ------ | ---- | ---- |
| keyword | string | N    | 关键字  |

返回

     [
        {
          "id": "15",
          "school_id": "2",
          "title": "数学",
          "state": "1"
        },
        {
          "id": "14",
          "school_id": "2",
          "title": "语文",
          "state": "1"
        }
      ]

### 2.3课程内容

course/row

请求

| 参数   | 类型   | 是否必须 | 说明   |
| ---- | ---- | ---- | ---- |
| id   | int  | Y    | 课程ID |

返回

    {
        "id": "14",
        "school_id": "2",
        "title": "语文",
        "state": "1"
      }

### 2.4课程新增

course/create

请求

| 参数    | 类型     | 是否必须 | 说明   |
| ----- | ------ | ---- | ---- |
| title | string | Y    | 课程标题 |

返回

    34  //返回新增课程ID
### 2.5课程编辑

course/update

请求

| 参数    | 类型     | 是否必须 | 说明   |
| ----- | ------ | ---- | ---- |
| id    | int    | Y    | 课程   |
| title | string | Y    | 课程标题 |

返回

    1  //更改的记录数, 注意, 如果更改后的内容跟原来的一样, 会自动忽略, 更改记录数为0
### 2.6课程删除

course/del

请求

| 参数   | 类型   | 是否必须 | 说明   |
| ---- | ---- | ---- | ---- |
| id   | int  | Y    | 课程ID |

返回

    1 //删除的记录数
### 2.7课程恢复

course/rec

请求

| 参数   | 类型   | 是否必须 | 说明   |
| ---- | ---- | ---- | ---- |
| id   | int  | Y    | 课程ID |

返回

    1 //更改的记录数, 注意, 如果更改后的内容跟原来的一样, 会自动忽略, 更改记录数为0


### 2.8获取教师课程

course/staff

请求

| 参数       | 类型   | 是否必须 | 说明    |
| -------- | ---- | ---- | ----- |
| staff_id | int  | Y    | 教职工ID |

返回

    [
        {
          "id": "2",
          "class_id": "43",
          "class_title": "有优战队",
          "course_id": "2",
          "course_title": "英语"
        }
      ]

## 3班级

### 3.1班级列表

classes/rows

请求

| 参数      | 类型      | 是否必须 | 说明                   |
| ------- | ------- | ---- | -------------------- |
| keyword | string  | N    |                      |
| page    | int     | N    |                      |
| all     | boolean | N    | 默认为false，为true返回所有班级 |

返回

```
{
    "total": 37,
    "count": 10,
    "page_index": 1,
    "page_size": 10,
    "page_count": 4,
    "result": [
      {
        "student_count": "1",
        "id": "152",
        "school_id": "2",
        "year": "2016",
        "title": "hhh",
        "descript": "zzsz",
        "status": "1",
        "staff_id": "0"
      },
      {
        "student_count": "2",
        "id": "138",
        "school_id": "2",
        "year": "2017",
        "title": "2017（1）班",
        "descript": "2017.01",
        "status": "1",
        "staff_id": "0"
      }
    ]
  }
```

### 3.2班级内容

classes/row

请求

| 参数   | 类型   | 是否必须 | 说明   |
| ---- | ---- | ---- | ---- |
| id   | int  | Y    | 班级ID |

返回

```
{
    "student_count": "2",
    "id": "138",
    "school_id": "2",
    "year": "2017",
    "title": "2017（1）班",
    "descript": "2017.01",
    "status": "1",
    "staff_id": "0"
  }
```

### 3.3班级创建

classes/create

请求

| 参数    | 类型     | 是否必须 | 说明   |
| ----- | ------ | ---- | ---- |
| year  | string | Y    | 届别   |
| title | string | Y    | 名称   |
| desc  | string | Y    | 备注   |

返回

```
33 //新增班级ID
```

### 3.4班级修改

classes/update

请求

| 参数    | 类型     | 是否必须 | 说明   |
| ----- | ------ | ---- | ---- |
| year  | string | Y    | 届别   |
| title | string | Y    | 名称   |
| desc  | string | Y    | 备注   |
| id    | int    | Y    | 班级ID |

返回

```
1 //修改的数据数量
```

### 3.5班级删除（只修改状态）

classes/del

请求

| 参数   | 类型   | 是否必须 | 说明   |
| ---- | ---- | ---- | ---- |
| id   | int  | Y    | 班级ID |

返回

```
1 //删除的数据数量
```

### 3.6班级列表(不分页)

classes/all

请求

| 参数     | 类型   | 是否必须 | 说明                                       |
| ------ | ---- | ---- | ---------------------------------------- |
| status | int  | N    | 状态，默认status=1，查询老师当前任课班级，传值status=2，查询所有班级包括历史班级 |



返回

```
 [
    {
      "id": "120",
      "title": "测试",
      "status": "1",
      "cwc_id": "4",
      "stu_count": "3",
      "exe_count": "2"
    },
    {
      "id": "43",
      "title": "有优战队",
      "status": "1",
      "cwc_id": "3",
      "stu_count": "37",
      "exe_count": "1"
    }
  ]
```

### 3.7获取班级和课程关联ID

classes/course_with_class

请求

| 参数      | 类型   | 是否必须 | 说明   |
| ------- | ---- | ---- | ---- |
| classid | int  | Y    | 班级ID |

返回

```
3 //班级和课程关联ID，不存在返回0
```

### 3.8班级课程记录

classes/class_record

请求

| 参数      | 类型   | 是否必须 | 说明   |
| ------- | ---- | ---- | ---- |
| classid | int  | Y    | 班级ID |
| page    | int  | N    |      |

返回

```
{
    "classname": "有优战队",
    "students": [
      {
        "title": "2017-03-21 10:21:45 有优战队",
        "tasktitle": "物理",
        "start_time": "2017-03-21 10:21:45"
      }
    ],
    "maxpage": 1,
    "count": "1",
    "currentpage": 1
  }
```




## 4 教师管理（Staff）

### 4.1 教师列表

staff/rows

请求

|   参数    |   类型   | 是否必须 | 说明        |
| :-----: | :----: | :--: | :-------- |
|  page   |  int   |  N   | 页码        |
| keyword | string |  N   | 搜索关键词，默认空 |

返回

```
{
    "total": 3,
    "count": 3,
    "page_index": 1,
    "page_size": 20,
    "page_count": 1,
    "result": [
      {
        "id": "185",
        "school_id": "31",
        "client_id": "0",
        "account": "12345678902",
        "passwd": "12345678902",
        "mobile": "12345678902",
        "lastname": "伍",
        "firstname": "老师",
        "descript": "为了自个测试用",
        "gender": "0",
        "state": "1",
        "wechat_openid": "",
        "email": "",
        "sign_time": "2017-03-27 15:50:43",
        "cached": "{\"cwc_id\":0,\"testing_id\":0,\"doing_exercise_model\":0,\"interact_id\":0,\"player\":[],\"full_screen\":0,\"showplayer\":0,\"doing_exercise_set_id\":0,\"doing_exercise_set_title\":\"\",\"doing_exercise_set\":[],\"doing_exercise_index\":0,\"teacher_id\":0,\"exercise_set_record_id\":541}"
      },
      ...
    ]
  }
```

### 4.2 学校内的所有老师（待修改为对应课程的老师）

staff/all

请求

|   参数    |   类型   | 是否必须 | 说明    |
| :-----: | :----: | :--: | :---- |
| keyword | string |  N   | 搜索关键词 |

返回

```
[
    {
      "id": "185",
      "school_id": "31",
      "client_id": "0",
      "account": "12345678902",
      "passwd": "12345678902",
      "mobile": "12345678902",
      "lastname": "伍",
      "firstname": "老师",
      "descript": "为了自个测试用",
      "gender": "0",
      "state": "1",
      "wechat_openid": "",
      "email": "",
      "sign_time": "2017-03-27 15:50:43",
      "cached": "{\"cwc_id\":0,\"testing_id\":0,\"doing_exercise_model\":0,\"interact_id\":0,\"player\":[],\"full_screen\":0,\"showplayer\":0,\"doing_exercise_set_id\":0,\"doing_exercise_set_title\":\"\",\"doing_exercise_set\":[],\"doing_exercise_index\":0,\"teacher_id\":0,\"exercise_set_record_id\":541}"
    },
]
```

### 4.3 教师详情

staff/row

请求

|  参数  |  类型  | 是否必须 | 说明   |
| :--: | :--: | :--: | :--- |
|  id  | int  |  Y   | 教师id |

返回

```$xslt
{
    "id": "185",
    "school_id": "31",
    "client_id": "0",
    "account": "12345678902",
    "passwd": "12345678902",
    "mobile": "12345678902",
    "lastname": "伍",
    "firstname": "老师",
    "descript": "为了自个测试用",
    "gender": "0",
    "state": "1",
    "wechat_openid": "",
    "email": "",
    "sign_time": "2017-03-27 15:50:43",
    "cached": "{\"cwc_id\":0,\"testing_id\":0,\"doing_exercise_model\":0,\"interact_id\":0,\"player\":[],\"full_screen\":0,\"showplayer\":0,\"doing_exercise_set_id\":0,\"doing_exercise_set_title\":\"\",\"doing_exercise_set\":[],\"doing_exercise_index\":0,\"teacher_id\":0,\"exercise_set_record_id\":541}"
    "wx_nickname": "vans",
    "wx_headimgurl": "http://wx.qlogo.cn/mmopen/PiajxSqBRaELmkFvBMQ72ecSicKAOWCg4jjqRzeAONdyAsSkZlFk9geWZSmp6Kyp4l3HkkxibQ",
    "course_title": "语文",
    "school_title": "有优特攻队"
  }
```

### 4.4 新增教师（待新增课程字段）

staff/create

请求

|    参数     |   类型   | 是否必须 | 说明         |
| :-------: | :----: | :--: | :--------- |
| firstname | string |  Y   | 名          |
| lastname  | string |  Y   | 姓          |
|  mobile   |  int   |  Y   | 手机号        |
|   email   | string |  N   | 邮箱         |
|  account  |  int   |  Y   | 账号（同手机号）   |
|  passwd   | string |  Y   | 密码         |
|  gender   |  int   |  Y   | 性别 1：男 0：女 |
|   desc    | string |  N   | 备注，简介      |

返回

```$xslt
188 新增的教师id
```

错误返回

|  返回码  | 说明    |
| :---: | :---- |
| 15003 | 账号已存在 |
| 13000 | 参数错误  |

### 4.5 修改教师（待新增课程字段）

staff/update

请求

|    参数     |   类型   | 是否必须 | 说明         |
| :-------: | :----: | :--: | :--------- |
|    id     |  int   |  Y   | 老师id       |
| firstname | string |  Y   | 名          |
| lastname  | string |  Y   | 姓          |
|  mobile   |  int   |  Y   | 手机号        |
|   email   | string |  N   | 邮箱         |
|  account  |  int   |  Y   | 账号（同手机号）   |
|  passwd   | string |  Y   | 密码         |
|  gender   |  int   |  Y   | 性别 1：男 0：女 |
|   desc    | string |  N   | 备注，简介      |

返回

```$xslt
1 被更新的行数
```

错误返回

|  返回码  | 说明    |
| :---: | :---- |
| 15003 | 账号已存在 |
| 13000 | 参数错误  |
| 15101 | 老师不存在 |

### 4.6 禁用教师

staff/del

请求

|  参数  |  类型  | 是否必须 | 说明   |
| :--: | :--: | :--: | :--- |
|  id  | int  |  Y   | 教师id |

返回

```$xslt
1 被更新的行数
```

错误返回

|  返回码  | 说明    |
| :---: | :---- |
| 15101 | 老师不存在 |

### 4.7 启用教师

staff/release

请求

|  参数  |  类型  | 是否必须 | 说明   |
| :--: | :--: | :--: | :--- |
|  id  | int  |  Y   | 教师id |

返回

```$xslt
1 被更新的行数
```

错误返回

|  返回码  | 说明    |
| :---: | :---- |
| 15101 | 老师不存在 |

### 4.8 彻底删除教师

staff/del_permanent

请求

|  参数  |  类型  | 是否必须 | 说明   |
| :--: | :--: | :--: | :--- |
|  id  | int  |  Y   | 教师id |

返回

```$xslt
1 被删除的行数
```

错误返回

|  返回码  | 说明    |
| :---: | :---- |
| 15101 | 老师不存在 |



### 4.9 获取个人信息

personal/index

请求

|  参数  |  类型  |      |      |
| :--: | :--: | :--: | :--- |
|      |      |      |      |

返回

```$xslt
{
    "stat": 0,
    "msg": "",
    "data": {
        "stu_id": 5587,
        "school_id": 47,
        "open_id": "o5Zz0vlt3QYAi8cTMPDFbYOaPOU0",
        "head_image": "http://dev17.ubanjiaoyu.com/static/organize/images/cs1.jpg",
        "wx_head_image": "http://wx.qlogo.cn/mmopen/hgXWbMaaqmDFy3oO8wmIvhw3kSRlc3Ma8HwCBLQ1C9IXs3jr0wLOdVVwI37HzyQ1ak7UwiblPyFjeATPicdsXbyNumiaP4hTcjic/0",
        "lastname": "孙",
        "firstname": "老师",
        "gender": 0,
        "wechat_num": "",
        "email": "",
        "mobile": "12345678901",
        "descript": "",
        "teach_age": 2,
        "job_title": "",
        "account": "12345678901",
        "school_name": "优伴教育",
        "school_mobile": "075586719385",
        "school_email": "",
        "class_name": "高二(1)班,高二(3)班,高二(2)班",
        "device_uid": "",
        "student_id": "99934592",
        "course_name": "语文",
        "jyeoo_course": [
            "HIGH_CHINESE" // 菁优网任课科目，在菁优网不支持的科目将不会显示
        ]
    }
}
```



## 5班级课程	

### 5.1班级课程列表（不分页）

class_course/rows

请求



| 参数       | 类型   | 是否必须 | 说明   |
| -------- | ---- | ---- | ---- |
| class_id | int  | Y    | 班级ID |

返回

```
[
    {
      "id": "2",
      "class_id": "43",
      "course_id": "2",
      "staff_id": "114"
    },
    {
      "id": "22",
      "class_id": "43",
      "course_id": "4",
      "staff_id": "0"
    }
  ]
```

### 5.2班级内容和课程内容

class_course/row

请求

| 参数   | 类型   | 是否必须 | 说明        |
| ---- | ---- | ---- | --------- |
| id   | int  | Y    | 班级课程关联表ID |

返回

```
{
    "id": "4",
    "class_id": "120",
    "course_id": "4",
    "staff_id": "113",
    "title": "测试",
    "firstname": null,
    "lastname": null
  }
```

### 5.3班级新增课程

class_course/create

请求

| 参数        | 类型   | 是否必须 | 说明   |
| --------- | ---- | ---- | ---- |
| class_id  | int  | Y    | 班级ID |
| course_id | int  | Y    | 课程ID |
| staff_id  | int  | Y    | 员工ID |

返回

```
32 //新增 班级和课程的关联ID
```

### 5.4班级删除课程

class_course/del

请求

| 参数   | 类型   | 是否必须 | 说明        |
| ---- | ---- | ---- | --------- |
| id   | int  | Y    | 班级课程关联表ID |

返回

```
1 //返回删除的数量
```




## 6学生管理（Student）

### 6.1 学生列表

student/rows

请求

|    参数     |   类型   | 是否必须 | 说明                               |
| :-------: | :----: | :--: | :------------------------------- |
| class_id  |  int   |  N   | 班级id，默认0，默认返回当前学校的所有班级           |
|   page    |  int   |  N   | 页码，默认1                           |
|  keyword  | string |  N   | 搜索关键词，默认空                        |
|  get_all  |  bool  |  N   | 是否返回所有状态的学生，默认false，默认返回正常状态下的学生 |
| page_size |  int   |  N   | 每页数量，默认20条                       |

返回

```
{
    "total": 75,
    "count": 20,
    "page_index": 1,
    "page_size": 20,
    "page_count": 4,
    "result": [
      {
        "class_id": "141",
        "class_title": "压力测试班",
        "class_year": "2017",
        "id": "1674",
        "gender": "0",
        "title": "1075",
        "student_id": "1075",
        "mobile": "",
        "status": "1"
      },
      ...
    ]
  }
```

错误返回

|  返回码  | 说明    |
| :---: | :---- |
| 15100 | 班级不存在 |

### 6.2 学生详情

student/row

请求

|   参数    |  类型  | 是否必须 | 说明                               |
| :-----: | :--: | :--: | :------------------------------- |
|   id    | int  |  Y   | 学生id                             |
| get_all | bool |  N   | 是否返回所有状态的学生，默认false，默认返回正常状态下的学生 |

返回

```$xslt
{
    "class_title": "压力测试班",
    "class_year": "2017",
    "id": "1674",
    "client_id": "0",
    "class_id": "141",
    "in_time": "2017-03-10 14:09:07",
    "out_time": null,
    "status": "1",
    "descript": null,
    "device_uid": "3434a45f",
    "student_id": "1075",
    "title": "1075",
    "gender": "0",
    "mobile": "",
    "school_id": "0"
  }
```

错误返回

|  返回码  | 说明    |
| :---: | :---- |
| 15103 | 学生不存在 |

### 6.3 新增学生

student/create

请求

|     参数     |   类型   | 是否必须 | 说明         |
| :--------: | :----: | :--: | :--------- |
|  class_id  |  int   |  Y   | 班级id       |
|   mobile   |  int   |  Y   | 手机号        |
|   title    | string |  Y   | 姓名         |
|   gender   |  int   |  Y   | 性别 1：男 0：女 |
| student_id |  int   |  Y   | 学号         |

返回

```$xslt
188 新增的学生id
```

错误返回

|  返回码  | 说明      |
| :---: | :------ |
| 15100 | 班级不存在   |
| 15004 | 学生学号已存在 |

### 6.4 修改学生

student/update

请求

|     参数     |   类型   | 是否必须 | 说明         |
| :--------: | :----: | :--: | :--------- |
|     id     |  int   |  Y   | 学生id       |
|  class_id  |  int   |  Y   | 班级id       |
|   mobile   |  int   |  Y   | 手机号        |
|   title    | string |  Y   | 姓名         |
|   gender   |  int   |  Y   | 性别 1：男 0：女 |
| student_id |  int   |  Y   | 学号         |

返回

```$xslt
1 被更新的行数
```

错误返回

|  返回码  | 说明      |
| :---: | :------ |
| 15103 | 学生不存在   |
| 15004 | 学生学号已存在 |

### 6.5 禁用学生

student/del

请求

|  参数  |  类型  | 是否必须 | 说明   |
| :--: | :--: | :--: | :--- |
|  id  | int  |  Y   | 学生id |

返回

```$xslt
1 被更新的行数
```

错误返回

|  返回码  | 说明    |
| :---: | :---- |
| 15103 | 学生不存在 |

### 6.6 启用学生

student/recover

请求

|  参数  |  类型  | 是否必须 | 说明   |
| :--: | :--: | :--: | :--- |
|  id  | int  |  Y   | 学生id |

返回

```$xslt
1 被更新的行数
```

错误返回

|  返回码  | 说明    |
| :---: | :---- |
| 15103 | 学生不存在 |

### 6.7 返回班级学生数

student/count

请求

|    参数    |  类型  | 是否必须 | 说明   |
| :------: | :--: | :--: | :--- |
| class_id | int  |  Y   | 班级id |

返回

```$xslt
75 班级学生数
```

错误返回

|  返回码  | 说明    |
| :---: | :---- |
| 15100 | 班级不存在 |

### 6.8 获取某学生某节课的回答习题情况

student/get_exercise_list_by_student 

请求方式：post

作者：vans

请求参数：

|          参数           |   类型   | 是否必须 | 说明   |
| :-------------------: | :----: | :--: | :--- |
| doing_exercise_set_id |  int   |  Y   | 班级id |
|        stu_id         | string |  Y   | 学生ID |

返回

```$xslt
 {
    "stat": 0,
    "msg": "获取成功",
    "data": {
        "use_timestamp_avg": 7.8, // 该学生平均使用秒数
        "active_rate": 100, // 该学生活跃度
        "correct_rate": 100, // 学生正确率
        "interact_lists": [ // 互动列表
            {	"interact_id":123456 // 互动ID
                "question_lists": [ // 每个互动的题目列表
                    {
                    	"title_img_url" :["http://dev17.ubanjiaoyu.com/upload/111.png",
                    					  "http://dev17.ubanjiaoyu.com/upload/111.png"
                    	], // 增加习题拍照的照片，只有纯互动和随堂自定义模式才有，其他模式为空
                    	"thum_title_img_url":[
                          					"http://dev17.ubanjiaoyu.com/upload/111.png",
                    					    "http://dev17.ubanjiaoyu.com/upload/111.png"
                    	],  // 习题拍照缩略图
                        "student_answerd": "C,D", // 学生回答答案
                        "type": 4, // 题目类型：1代表单选题 2代表判断题 3代表书写题 4代表多选题
                        "correct_answer": [ // 正确答案：选择题模式：12345代表ABCDE,判断题模式：12代表对错，书写题模式10代表提交
                            1,
                            2,
                            3
                        ],
                        "option": { // 选项代表意思
                            "1": "A",
                            "2": "B",
                            "3": "C",
                            "4": "D",
                            "5": "E"
                        },
                        "re_answer": "A,B,C", //已翻译正确答案
                        "is_correct": 0, // 是否回答正确 1代表正确 0代表错误
                        "question_title": "<p>多选题</p>" // 题目
                    },
                    {
                        "student_answerd": "提交",
                        "type": 3,
                        "correct_answer": [
                            10
                        ],
                        "option": {
                            "10": "提交"
                        },
                        "re_answer": "提交",
                        "is_correct": 1,
                        "question_title": "<p>书写题</p>"
                    },
                    {
                        "student_answerd": "错",
                        "type": 2,
                        "correct_answer": [
                            1
                        ],
                        "option": {
                            "1": "对",
                            "2": "错"
                        },
                        "re_answer": "对",
                        "is_correct": 0,
                        "question_title": "<p>判断题</p>"
                    },
                    {
                        "student_answerd": "C",
                        "type": 1,
                        "correct_answer": [
                            2
                        ],
                        "option": {
                            "1": "A",
                            "2": "B",
                            "3": "C",
                            "4": "D"
                        },
                        "re_answer": "B",
                        "is_correct": 0,
                        "question_title": "<p>单选题</p>"
                    }
                ],
                "question_type": { // 题目类型统计 除了随堂练习，其他互动这个参数为null
                    "radio_type_count": 1, // 单选
                    "checkbox_type_count": 1, // 多选
                    "tf_type_count": 1, // 对错
                    "writing_type_count": 1 // 书写
                },
                "use_timestamp": 22, // 这轮互动使用时间s
                "answer_writing_id": [ // 书写的id
                    5581
                ],
                "interact_type": 2, // 互动类型0:纯互动，1:习题互动，2:随堂互动 
                "question_answerd_count": 4 // 已回答题目个数
            },
            {
                "question_lists": [
                    {
                        "correct_answer": [
                            1
                        ],
                        "type": 1,
                        "option": {
                            "1": "A",
                            "2": "B",
                            "3": "C",
                            "4": "D"
                        },
                        "student_answerd": "A",
                        "is_correct": 1,
                        "question_title": "<p>单选题</p>"
                    }
                ],
                "question_type": null,
                "use_timestamp": 0,
                "answer_writing_id": [
                    5582
                ],
                "interact_type": 1,
                "question_answerd_count": 1
            }
        ]
    }
}
```
75 班级学生数
```

错误返回

|  返回码  | 说明    |
| :---: | :---- |
| 15100 | 班级不存在 |



## 7课后报表

### 7.1显示所有上课记录

report/testing_history

请求

| 参数       | 类型     | 是否必须 | 说明                                      |
| -------- | ------ | ---- | --------------------------------------- |
| class_id   | int    | N    | 班级id，不传表示返回当前老师课堂记录，传了表示当前id下的所有记录（包括更换科目的记录） |
| page     | int    | N    | 页码，默认0                                  |
| keyword  | string | N    | 搜索关键字，默认空                               |
| pagesize | int    | N    | 每页数量，默认9                                |

返回
​```$xslt
{
    "total": "100", // 记录总数
    "count": 9, // 当页数量
    "page_index": 1, //当前页码
    "page_size": 9, //每页数量
    "page_count": 12, //总共页数
    "result": [
      {
        "id": "1803",
        "title": "2017-04-14 15:26:35 优伴团队",
        "finish_time": "2017-04-14 15:28:03",
        "start_time": "2017-04-14 15:26:35",
        "cwc_id": "62",
        "interact_count": "0",
        "class_title": "优伴团队",
        "course_title": "有优大讲堂",
        "answer_count": "0",
        "hit": 0, //活跃度
        "times": "00:01:28"
      },
      ...
    ]
}
```



## 7报表管理（report）

### 7.2生成互动一下和习题互动的报表

report/generate

请求

| 参数          | 类型   | 是否必须 | 说明   |
| ----------- | ---- | ---- | ---- |
| interact_id | int  | Y    | 互动id |

返回
```$xslt
{
     [
        {
          "accuracy": {
            "correct_answer": [
              2
            ],//正确答案
            "answered_correct_count": 1, //正确回答人数
            "option": {
              "1": "A",
              "2": "B",
              "3": "C",
              "4": "D"
            }, //当前互动可选项（选择题是1A2B3C4D，判断题是1对2错，书写题是10提交）
            "answered": 2,//正确答案编号
            "student_count": 12,//学生总人数
            "chart": {
              "1": 1,
              "2": 1,
              "3": 0,
              "4": 0
            } //每个选项的人数
          },
          "answer_statistics": {
            "student_count": 12, //学生人数
            "answered_count": 2, //回答人数
            "answer_rate": 0.17, //作答率
            "accuracy": 0.08
          },//表格的四个数据
          "answer_detail":{
            "total_time":12, // 总用时s
            "average_time":6, // 平均用时s
            "correct_student_id":[
            	5632  // 回答正确的学生ID
            ],
            "answered_student_id":[
            	0, // 已回答的学生ID
            	1
            ],
            "correct_student_rate":8, // 正确率
            "answered_student_rate":15, // 回答率
            "interact_correct_number":1, // 回答正确的人数
            "interact_answerd_number":2 // 已回答的人数
          },
          "ranking": [
            {
              "name": "伍晓波",
              "gender": "1",
              "answer": "B",
              "begin_time": "2017-04-14 09:30:32",
              "exercise_type_id": "1",
              "correct_answer": "B",
              "used_time": "00:00:03",
              "is_correct":0, // 是否回答正确 1代表正确 0代表错误
              "use_timestamp":6, // 使用时间戳
              "sort":2 // 排名 以回答正确错误，再到使用时间升序排列
            },
            {
              "name": "曾鸣",
              "gender": "1",
              "answer": "A",
              "begin_time": "2017-04-14 09:30:31",
              "exercise_type_id": "1",
              "correct_answer": "B",
              "used_time": "00:00:02"
            }
          ] //排行榜
        }
     ]
}
```

### 7.3生成随堂练习的报表

report/generate_for_recognition

请求

| 参数          | 类型   | 是否必须 | 说明   |
| ----------- | ---- | ---- | ---- |
| interact_id | int  | Y    | 互动id |

返回
```$xslt
{
[
    {
      "accuracy": {
        "correct_answer": [
          2
        ],//正确答案
        "answered_correct_count": 1, //正确回答人数
        "option": {
          "1": "A",
          "2": "B",
          "3": "C",
          "4": "D"
        }, //当前互动可选项（选择题是1A2B3C4D，判断题是1对2错，书写题是10提交）
        "answered": 2,//正确答案编号
        "student_count": 12,//学生总人数
        "chart": {
          "1": 1,
          "2": 1,
          "3": 0,
          "4": 0
        } //每个选项的人数
      },
      "answer_statistics": {
        "student_count": 12, //学生人数
        "answered_count": 2, //回答人数
        "answer_rate": 0.17, //作答率
        "accuracy": 0.08
      },//表格的四个数据
    },
    ...
  ]
}
```

### 7.4返回对应互动的报表（直接从数据库拿已生成好的报表）

report/get_all

请求

| 参数          | 类型   | 是否必须 | 说明   |
| ----------- | ---- | ---- | ---- |
| interact_id | int  | Y    | 互动id |

返回
```$xslt
习题互动和互动一下 同7.2
随堂练习 同7.3
```

### 7.5查看课堂的所有互动的报表

report/get_history

请求

| 参数                    | 类型   | 是否必须 | 说明   |
| --------------------- | ---- | ---- | ---- |
| doing_exercise_set_id | int  | Y    | 课堂id |

返回
```$xslt
[
   {
      "id": "7680", // 互动id
      "type_id": "4", //互动类型，1选择题，2判断题，3书写题，4随堂练习
      "exe_id": "0", //互动用到的习题id（若是exe_id>0,type_id<4，则是习题互动）
      "report": [
        习题互动和互动一下 同7.2
        随堂练习 同7.3
      ]
   },
   ...
]
```

### 7.6返回课堂的基本信息

report/brief_info

请求

| 参数                    | 类型   | 是否必须 | 说明   |
| --------------------- | ---- | ---- | ---- |
| doing_exercise_set_id | int  | Y    | 课堂id |

返回
```$xslt
{
    "id": "1751", // 课堂id
    "teacher_id": "185", 
    "start_time": "2017-04-12 16:32:18",
    "finish_time": "2017-04-12 16:40:14",
    "student_list": "[{\"id\":\"1851\",\"gender\":\"1\",\"title\":\"\\u738b\\u4e07\\u6770\",\"device_uid\":\"\",\"student_id\":\"12345678\",\"online\":0,\"answered\":0,\"submited\":0},{\"id\":\"1843\",\"gender\":\"1\",\"title\":\"\\u66fe\\u9e23\",\"device_uid\":\"\",\"student_id\":\"19861001\",\"online\":0,\"answered\":0,\"submited\":0},{\"id\":\"1852\",\"gender\":\"1\",\"title\":\"\\u5362\\u5b8f\\u6d32\",\"device_uid\":\"\",\"student_id\":\"19870629\",\"online\":0,\"answered\":0,\"submited\":0},{\"id\":\"1845\",\"gender\":\"0\",\"title\":\"\\u5b59\\u73fa\",\"device_uid\":\"\",\"student_id\":\"19890310\",\"online\":0,\"answered\":0,\"submited\":0},{\"id\":\"1841\",\"gender\":\"1\",\"title\":\"\\u5362\\u5b8f\\u6960\",\"device_uid\":\"\",\"student_id\":\"19891115\",\"online\":0,\"answered\":0,\"submited\":0},{\"id\":\"1847\",\"gender\":\"0\",\"title\":\"\\u90c1\\u4f73\",\"device_uid\":\"\",\"student_id\":\"19910422\",\"online\":0,\"answered\":0,\"submited\":0},{\"id\":\"1844\",\"gender\":\"0\",\"title\":\"\\u6234\\u5251\\u96ef\",\"device_uid\":\"\",\"student_id\":\"19920816\",\"online\":0,\"answered\":0,\"submited\":0},{\"id\":\"1849\",\"gender\":\"1\",\"title\":\"\\u674e\\u8499\\u601d\",\"device_uid\":\"\",\"student_id\":\"19920922\",\"online\":0,\"answered\":0,\"submited\":0},{\"id\":\"1846\",\"gender\":\"1\",\"title\":\"\\u94b1\\u57f9\\u822a\",\"device_uid\":\"\",\"student_id\":\"19930613\",\"online\":0,\"answered\":0,\"submited\":0},{\"id\":\"1850\",\"gender\":\"1\",\"title\":\"\\u59dc\\u5229\\u6d01\",\"device_uid\":\"\",\"student_id\":\"19940727\",\"online\":0,\"answered\":0,\"submited\":0},{\"id\":\"1842\",\"gender\":\"0\",\"title\":\"\\u4f0d\\u6653\\u71d5\",\"device_uid\":\"\",\"student_id\":\"19940801\",\"online\":0,\"answered\":0,\"submited\":0},{\"id\":\"1848\",\"gender\":\"1\",\"title\":\"\\u4f0d\\u6653\\u6ce2\",\"device_uid\":\"\",\"student_id\":\"19960727\",\"online\":0,\"answered\":0,\"submited\":0}]",
    "title": "2017-04-12 16:32:18 优伴团队",
    "state": "Y",
    "cwc_id": "62",
    "class_id": "153",
    "voice": null //录音文件路径，暂时是null
  }
```

### 7.7返回最新的一堂课的报表（用于班级动态）

report/get_last

请求

| 参数     | 类型   | 是否必须 | 说明       |
| ------ | ---- | ---- | -------- |
| cwc_id | int  | Y    | 班级课程关联id |

返回
```$xslt
{
    "id": "1806", //课堂id
    "teacher_id": "185", 老师id
    "start_time": "2017-04-14 15:28:21", 
    "finish_time": "2017-04-14 17:20:34",
    "title": "2017-04-14 15:28:21 优伴团队",
    "state": "Y", //课堂状态
    "cwc_id": "62",
    "class_id": "153",
    "voice": null, //录音文件路径，暂时是null
    "using_time": "6733", //课堂用时（格式化前）
    "course_title": "有优大讲堂", //课程名称
    "staff_fname": "老师", //老师名称
    "staff_lname": "伍", //老师姓氏
    "class_title": "优伴团队",
    "ans_count": "2", //总共参与人数
    "total_time": "01:52:13", //课堂用时（格式化后）
    "stu_count": 12, //学生总人数
    "choose_count": "4", //选择题题数
    "choose_time": "00:04:41", //选择题总用时
    "tf_count": "5", //判断题题数
    "tf_time": "00:45:39", //判断题总用时
    "write_count": "3", //书写题题数
    "write_time": "00:09:02", //书写题总用时
    "practice_count": 0, //随堂练习题数
    "practice_time": "00:00:00", //随堂练习总用时
    "prac_time": 0, //随堂练习总用时（秒数）
    "exe_interact_time": 0, //习题互动总用时（秒数）
    "pure_interact_time": 3562, //纯互动总用时（秒数）
    "index": [1,2,3,4,5,6,7,8,9,10,11,12], //题号
    "active": [8,8,8,16,8,0,0,16,0,0,0,0] //每道题的活跃度 
  }
```

### 7.8返回对应课堂的报表

report/get_total_report 

请求

| 参数                    | 类型   | 是否必须 | 说明   |
| --------------------- | ---- | ---- | ---- |
| doing_exercise_set_id | int  | Y    | 课堂id |

返回
```$xslt
{
    "id": "1806", //课堂id
    "teacher_id": "185", 老师id
    "start_time": "2017-04-14 15:28:21", 
    "finish_time": "2017-04-14 17:20:34",
    "title": "2017-04-14 15:28:21 优伴团队",
    "state": "Y", //课堂状态
    "cwc_id": "62",
    "class_id": "153",
    "stu_once_online_list": "[\"5632\",\"5631\"]",// 曾经在线过的人生
    "voice": null, //录音文件路径，暂时是null
    "using_time": "6733", //课堂用时（格式化前）
    "course_title": "有优大讲堂", //课程名称
    "staff_fname": "老师", //老师名称
    "staff_lname": "伍", //老师姓氏
    "class_title": "优伴团队",
    "ans_count": "2", //总共参与人数
    "total_time": "01:52:13", //课堂用时（格式化后）
    "stu_count": 12, //学生总人数
    "choose_count": "4", //选择题题数
    "choose_time": "00:04:41", //选择题总用时
    "tf_count": "5", //判断题题数
    "tf_time": "00:45:39", //判断题总用时
    "write_count": "3", //书写题题数
    "write_time": "00:09:02", //书写题总用时
    "practice_count": 0, //随堂练习题数
    "practice_time": "00:00:00", //随堂练习总用时
    "prac_time": 0, //随堂练习总用时（秒数）
    "exe_interact_time": 0, //习题互动总用时（秒数）
    "pure_interact_time": 3562, //纯互动总用时（秒数）
    "index": [1,2,3,4,5,6,7,8,9,10,11,12], //题号
    "active": [8,8,8,16,8,0,0,16,0,0,0,0] //每道题的活跃度 
  }
```

### 7.9返回历史查看的互动报表及详情

report/get_history_for_interact

请求

| 参数          | 类型   | 是否必须 | 说明   |
| ----------- | ---- | ---- | ---- |
| interact_id | int  | Y    | 互动id |

返回
```$xslt
{
    "interact_info": {
      "id": "7680", //互动id
      "doing_exercise_set_id": "1751", //课堂id
      "exercise_id": "0", //习题id
      "exercise_set_id": "142", //习题集id
      "custom_exercise_set": null, //随堂练习自定义内容
      "content": "",//题目内容
      "exercise_type_id": "4",//互动类型，1选择题，2判断题，3书写题，4随堂练习
      "correct_answer": null, //单题的正确答案
      "correct_answer_set": "{\"374\":\"10\",\"375\":\"10\",\"376\":\"10\",\"377\":\"10\",\"379\":\"10\",\"380\":\"10\",\"381\":\"2\"}", //随堂练习的正确答案
      "student_online_list": null, //学生列表（去brief_info拿）
      "state": "S", //互动状态
      "report": "", //报表，习题互动和互动一下 同7.2 随堂练习 同7.3
      "used_time": "00:00:02" //互动耗时
    },
    "exe_set": [
      {
        "id": "374",
        "client_id": "185",
        "exercise_type_id": "3",
        "content": "<p><span style=\"font-family: simsun, Tahoma, sans-serif; line-height: 21px; font-size: 36px; background-color: rgb(255, 255, 255);\">楚楚的生日在三月三十日，请问是哪年的三月三十日？</span></p>",
        "correct_answer": "10",
        "status": "1",
        "create_time": "2017-03-31 14:05:39",
        "update_time": "2017-03-31 14:05:39",
        "staff_id": "185"
      },
      ...
    ],//习题内容
    "answer_info": [] //学生作答历史，主要用于随堂练习
  }
```

### 7.10返回课堂的信息

report/get_history_set_info

请求

| 参数     | 类型   | 是否必须 | 说明   |
| ------ | ---- | ---- | ---- |
| set_id | int  | Y    | 课堂id |

返回
```$xslt
{
    "id": "1751", // 课堂id
    "teacher_id": "185",
    "student_list": "[{\"id\":\"1851\",\"gender\":\"1\",\"title\":\"\\u738b\\u4e07\\u6770\",\"device_uid\":\"\",\"student_id\":\"12345678\",\"online\":0,\"answered\":0,\"submited\":0},{\"id\":\"1843\",\"gender\":\"1\",\"title\":\"\\u66fe\\u9e23\",\"device_uid\":\"\",\"student_id\":\"19861001\",\"online\":0,\"answered\":0,\"submited\":0},{\"id\":\"1852\",\"gender\":\"1\",\"title\":\"\\u5362\\u5b8f\\u6d32\",\"device_uid\":\"\",\"student_id\":\"19870629\",\"online\":0,\"answered\":0,\"submited\":0},{\"id\":\"1845\",\"gender\":\"0\",\"title\":\"\\u5b59\\u73fa\",\"device_uid\":\"\",\"student_id\":\"19890310\",\"online\":0,\"answered\":0,\"submited\":0},{\"id\":\"1841\",\"gender\":\"1\",\"title\":\"\\u5362\\u5b8f\\u6960\",\"device_uid\":\"\",\"student_id\":\"19891115\",\"online\":0,\"answered\":0,\"submited\":0},{\"id\":\"1847\",\"gender\":\"0\",\"title\":\"\\u90c1\\u4f73\",\"device_uid\":\"\",\"student_id\":\"19910422\",\"online\":0,\"answered\":0,\"submited\":0},{\"id\":\"1844\",\"gender\":\"0\",\"title\":\"\\u6234\\u5251\\u96ef\",\"device_uid\":\"\",\"student_id\":\"19920816\",\"online\":0,\"answered\":0,\"submited\":0},{\"id\":\"1849\",\"gender\":\"1\",\"title\":\"\\u674e\\u8499\\u601d\",\"device_uid\":\"\",\"student_id\":\"19920922\",\"online\":0,\"answered\":0,\"submited\":0},{\"id\":\"1846\",\"gender\":\"1\",\"title\":\"\\u94b1\\u57f9\\u822a\",\"device_uid\":\"\",\"student_id\":\"19930613\",\"online\":0,\"answered\":0,\"submited\":0},{\"id\":\"1850\",\"gender\":\"1\",\"title\":\"\\u59dc\\u5229\\u6d01\",\"device_uid\":\"\",\"student_id\":\"19940727\",\"online\":0,\"answered\":0,\"submited\":0},{\"id\":\"1842\",\"gender\":\"0\",\"title\":\"\\u4f0d\\u6653\\u71d5\",\"device_uid\":\"\",\"student_id\":\"19940801\",\"online\":0,\"answered\":0,\"submited\":0},{\"id\":\"1848\",\"gender\":\"1\",\"title\":\"\\u4f0d\\u6653\\u6ce2\",\"device_uid\":\"\",\"student_id\":\"19960727\",\"online\":0,\"answered\":0,\"submited\":0}]",
    "title": "2017-04-14 15:28:21 优伴团队",
    "state": "Y",
    "cwc_id": "62",
    "class_id": "153",
    "voice": null //录音文件路径，暂时是null
    "used_time": "01:52:13", //课堂用时
    "interact_count": 12, //互动次数
    "interact": [
      "7940",
      "7946",
      "7950",
      "7953",
      "7954",
      "7956",
      "7959",
      "7960",
      "7966",
      "7967",
      "7968",
      "8008"
    ] //每一轮互动的id
  }
```



### 7.11返回学生维度报表信息

Report/all_stu_report_by_exercise_set"question_count": 2,

请求方式：post

作者：vans

请求参数

| 参数                    | 类型   | 是否必须 | 说明   |
| --------------------- | ---- | ---- | ---- |
| doing_exercise_set_id | int  | Y    | 课堂id |

返回

```$xslt
{
    "stat": 0,
    "msg": "获取成功",
    "data": {
        "exercise_set_active_rate": 21,
        "exercise_count": 9,
        "student_active_info": [// 已经按活跃的排序了
            {
                "stu_id": "5584",
                "gender": 1,
                "name": "钱培韩",
                "student_id": "201703",
                "answered_count": 7, // 回答个数
                "stu_active_rate": 78, // 活跃度
                "use_timestamp": 283,  // 使用时间
                "stu_correct_rate": 50, // 正确率
                "question_count": 2, // 题目个数
                "once_online": 1, //是否曾经在线
                "correct_count": 1, // 正确个数
                "sort": 1 // 排名序号
            },
            {
                "stu_id": "5583",
                "gender": 1,
                "name": "王万杰",
                "student_id": "201702",
                "answered_count": 1,
                "stu_active_rate": 11,
                "use_timestamp": 200,
                "sort": 2
            },
            {
                "stu_id": "5582",
                "gender": 0,
                "name": "李梦思",
                "student_id": "201701",
                "answered_count": 0,
                "stu_active_rate": 0,
                "use_timestamp": 0,
                "sort": 3
            },
            {
                "stu_id": "5582",
                "gender": 0,
                "name": "李梦思",
                "student_id": "201701",
                "answered_count": 0,
                "stu_active_rate": 0,
                "use_timestamp": 0,
                "sort": 3
            }
        ]
    }
}
```







## 8习题集

### 8.1习题集列表

exercise_set/rows

请求

| 参数       | 类型     | 是否必须 | 说明                             |
| -------- | ------ | ---- | ------------------------------ |
| keyword  | string | N    | 搜索                             |
| page     | int    | N    | 页码                             |
| class_id | int    | N    | 默认为0。0时返回所有未删除的习题集，其他返回该班级的习题集 |

返回

~~~
{
    "total": 10,
    "count": 10,
    "result": [
      {
        "id": "129",
        "title": "2018年高考模拟测试第一卷",
        "create_time": "2017-03-24 18:15:19",
        "update_time": null,
        "exercise_count": "0"
      },
      {
        "id": "5",
        "title": "因式分解",
        "create_time": "2016-10-13 22:52:19",
        "update_time": "2017-03-23 11:16:37",
        "exercise_count": "5"
      }
    ]
  }
~~~



### 8.2习题集内容

exercise_set/row

请求

| 参数     | 类型   | 是否必须 | 说明    |
| ------ | ---- | ---- | ----- |
| set_id | int  | Y    | 习题集ID |

返回

```
{
    "id": "139",
    "client_id": "155",
    "title": "ccc",
    "create_time": "2017-03-25 17:22:50",
    "update_time": null,
    "status": "1",
    "class_id": "0",
    "course_id": "0",
    "staff_id": "155"
  }
```

### 8.3习题集创建

exercise_set/create

请求

| 参数    | 类型     | 是否必须 | 说明    |
| ----- | ------ | ---- | ----- |
| title | string | Y    | 习题集名称 |

返回

```
36 //返回新增的习题集的ID
```

### 8.4习题集修改

exercise_set/edit

请求

| 参数    | 类型     | 是否必须 | 说明    |
| ----- | ------ | ---- | ----- |
| title | string | Y    | 习题集名称 |
| id    | int    | Y    | 习题集ID |

返回

```
1 //返回更改的习题集数量，如果未做修改返回0
```

### 8.5习题集删除

exercise_set/del

请求

| 参数   | 类型   | 是否必须 | 说明    |
| ---- | ---- | ---- | ----- |
| id   | int  | Y    | 习题集ID |

返回

```
1 //0或则1。1代表成功，0代表失败
```



### 8.6老师端习题集列表和回收站列表

exercise_set/index

请求

| 参数      | 类型     | 是否必须 | 说明                          |
| ------- | ------ | ---- | --------------------------- |
| status  | int    | N    | 默认为1。1返回所有习题集列表，0返回删除的习题集列表 |
| page    | int    | N    |                             |
| keyword | string | N    |                             |

返回

```
{
    "total": 10,
    "count": 10,
    "page_index": 1,
    "page_size": 12,
    "page_count": 1,
    "result": [
      {
        "id": "51",
        "client_id": "113",
        "title": "2020高考模拟题",
        "create_time": "2017-03-16 14:39:14",
        "update_time": null,
        "status": "1",
        "class_id": "0",
        "course_id": "0",
        "staff_id": "113",
        "number": "8",
        "usenumber": "4"
      },
      {
        "id": "130",
        "client_id": "113",
        "title": "骨灰盒或或或或或或或或或或",
        "create_time": "2017-03-24 18:15:41",
        "update_time": "2017-03-24 18:16:27",
        "status": "1",
        "class_id": "0",
        "course_id": "0",
        "staff_id": "113",
        "number": "0",
        "usenumber": "0"
      },
      ……
    ]
  }
```



### 8.7习题集内容  和习题集中习题内容

exercise_set/get_info

请求

| 参数   | 类型   | 是否必须 | 说明                            |
| ---- | ---- | ---- | ----------------------------- |
| id   | int  | Y    | 习题集                           |
| type | int  | N    | 默认为0。0时返回习题集信息和习题信息;1时只返回习题信息 |

返回

```
{
    "id": "51",
    "client_id": "113",
    "title": "2020高考模拟题",
    "create_time": "2017-03-16 14:39:14",
    "update_time": null,
    "status": "1",
    "class_id": "0",
    "course_id": "0",
    "staff_id": "113",
    "exercise_list": [
      {
        "id": "203",
        "client_id": "113",
        "exercise_type_id": "1",
        "content": "<h3>驾驶机动车在高速公路上行驶，遇有雾、雨、雪、沙尘、冰雹等低能见度气象条件时，能见度在50米以下时，以下做法正确的是什么？<br/>A、加速驶离高速公路<br/>B、在应急车道上停车等待<br/>C、可以继续行驶，但车速不得超过每小时40公里<br/>D、以不超过每小时20公里的车速从最近的出口尽快驶离高速公路</h3><p><br/></p>",
        "correct_answer": "2",
        "status": "1",
        "create_time": "2017-03-16 14:39:50",
        "update_time": "2017-03-16 14:39:50",
        "staff_id": "113",
        "exercise_type_name": "选择题",
        "ews_id": "192"
      },
      {
        "id": "312",
        "client_id": "113",
        "exercise_type_id": "2",
        "content": "<p>在△ABC中，角A、B、C所对的边分别为a、b、c，已知a=2，则B=60°．</p>",
        "correct_answer": "2",
        "status": "1",
        "create_time": "2017-03-23 11:21:29",
        "update_time": "2017-03-23 11:21:29",
        "staff_id": "113",
        "exercise_type_name": "判断题",
        "ews_id": "298"
      },
      ……
    ]
  }
```

### 8.8习题创建（一个习题集中最多创建10个习题）

exercise_set/create_exercise

请求

| 参数             | 类型     | 是否必须 | 说明                    |
| -------------- | ------ | ---- | --------------------- |
| set_id         | int    | Y    | 习题集                   |
| type_id        | int    | Y    | 习题类型(现只包含3种：选择，判断和书写) |
| content        | string | Y    | 习题描述                  |
| correct_answer | string | Y    | 参考答案                  |

返回

```
31 //新增习题ID
```

### 8.9修改习题

exercise_set/update_exercise

请求

| 参数             | 类型     | 是否必须 | 说明         |
| -------------- | ------ | ---- | ---------- |
| ews_id         | int    | Y    | 习题集和习题关联ID |
| content        | string | Y    | 习题描述       |
| correct_answer | string | Y    | 习题参考答案     |

返回

```
1 //返回影响的习题数量
```

### 8.10删除习题

exercise_set/del_exercise

请求

| 参数     | 类型   | 是否必须 | 说明         |
| ------ | ---- | ---- | ---------- |
| ews_id | int  | Y    | 习题集和习题关联ID |

返回

```
1 //返回删除的数量
```

### 8.11获取习题内容

exercise_set/get_exercise

请求

| 参数     | 类型   | 是否必须 | 说明         |
| ------ | ---- | ---- | ---------- |
| ews_id | int  | Y    | 习题集和习题关联ID |

返回

```
{
    "id": "308",
    "client_id": "113",
    "exercise_type_id": "1",
    "content": "<p>“直线<sub><img id=\"对象 28\" src=\"http://dev17.ubanjiaoyu.com/uploads/image/20170323/1490238164438853.gif\" height=\"21\" width=\"148\"/></sub>与<sub><img id=\"对象 29\" src=\"http://dev17.ubanjiaoyu.com/uploads/image/20170323/1490238164424260.gif\" height=\"21\" width=\"151\"/></sub>互相垂直”是“<sub><img id=\"对象 30\" src=\"http://dev17.ubanjiaoyu.com/uploads/image/20170323/1490238165948278.gif\" height=\"41\" width=\"42\"/></sub>”的&nbsp; (　　)</p><p><br/></p><p>A.充分不必要条件&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; B.必要不充分条件</p><p>C.充分必要条件&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; D.既不充分也不必要条件</p><p><br/></p>",
    "correct_answer": "2",
    "status": "1",
    "create_time": "2017-03-16 14:39:14",
    "update_time": null,
    "staff_id": "113",
    "set_id": "51",
    "exercise_id": "308",
    "sort": "1490238103",
    "ews_id": "294",
    "title": "2020高考模拟题",
    "class_id": "0",
    "course_id": "0"
  }
```

### 8.12习题升、降序

exercise_set/exercise_sort

请求

| 参数   | 类型   | 是否必须 | 说明               |
| ---- | ---- | ---- | ---------------- |
| id   | int  | Y    | 习题               |
| up   | int  | Y    | 0或者1。0代表下降，1代表上升 |

返回

```
true //true或者false
```



### 8.13习题集回收站清空

exercise_set/clear_recycle

请求

null	

返回

```
true 
```

### 8.14习题集恢复

exercise_set/recovery

请求

| 参数   | 类型     | 是否必须 | 说明              |
| ---- | ------ | ---- | --------------- |
| id   | string | Y    | 多个ID使用英文逗号','拼接 |

返回

```
true
```

## 9习题

### 9.1习题(全部)

exercise/rows

请求

| 参数              | 类型   | 是否必须 | 说明    |
| --------------- | ---- | ---- | ----- |
| exercise_set_id | int  | Y    | 习题集ID |

返回 

```
[
    {
      "ewsid": "24",
      "id": "29",
      "client_id": "1",
      "exercise_type_id": "1",
      "content": "<img src=\"http://dev17.ubanjiaoyu.com/uban/cp/public/uploads/exercise/1-1-1.png\"/>",
      "correct_answer": "3",
      "status": "1",
      "create_time": "2016-10-13 22:54:31",
      "update_time": "2017-03-25 10:57:03",
      "staff_id": "0",
      "type_title": "选择题"
    },
    {
      "ewsid": "28",
      "id": "33",
      "client_id": "1",
      "exercise_type_id": "3",
      "content": "<img src=\"http://dev17.ubanjiaoyu.com/uban/cp/public/uploads/exercise/1-3-1.png\"/>",
      "correct_answer": "10",
      "status": "1",
      "create_time": "2016-10-13 22:55:49",
      "update_time": "2017-03-25 10:57:03",
      "staff_id": "0",
      "type_title": "主观题"
    }
  ]
```

### 9.2习题内容

exercise/row

请求

| 参数   | 类型   | 是否必须 | 说明   |
| ---- | ---- | ---- | ---- |
| id   | int  | Y    | 习题ID |

返回

```
{
    "id": "34",
    "client_id": "1",
    "exercise_type_id": "1",
    "content": "<img src=\"http://dev17.ubanjiaoyu.com/uban/cp/public/uploads/exercise/2-1-1.png\"/>",
    "correct_answer": "4",
    "status": "1",
    "create_time": "2016-10-13 22:56:16",
    "update_time": "2017-03-25 10:57:03",
    "staff_id": "0",
    "estitle": "综合练习",
    "esid": "6",
    "ettitle": "选择题"
  }
```

### 9.3习题列表

exercise/list_rows

请求

| 参数            | 类型     | 是否必须 | 说明   |
| ------------- | ------ | ---- | ---- |
| exercise_type | int    | Y    | 习题   |
| keyword       | string | N    |      |
| page          | int    | N    |      |

返回

```
 {
    "total": 14,
    "count": 14,
    "page_index": 1,
    "page_size": 50,
    "page_count": 1,
    "result": [
      {
        "id": "248",
        "client_id": "113",
        "exercise_type_id": "2",
        "content": "<p>test<br/></p>",
        "correct_answer": "1",
        "status": "1",
        "create_time": "2017-03-21 15:32:35",
        "update_time": "2017-03-22 09:32:49",
        "staff_id": "113",
        "type_title": "判断题"
      },
      {
        "id": "242",
        "client_id": "0",
        "exercise_type_id": "2",
        "content": "1、“大煮干丝”是哪个菜系的代表菜之一( )。 A四川菜系 B山东菜系 C广东菜系 D淮扬菜系",
        "correct_answer": "1",
        "status": "1",
        "create_time": "2017-03-21 14:07:30",
        "update_time": "2017-03-21 14:07:30",
        "staff_id": "113",
        "type_title": "判断题"
      }，
      ……
    ]
  }
```

### 9.4共享习题创建

exercise/create_exercise

请求

| 参数             | 类型     | 是否必须 | 说明     |
| -------------- | ------ | ---- | ------ |
| type_id        | int    | Y    | 习题类型   |
| content        | sting  | Y    | 习题描述   |
| correct_answer | string | Y    | 习题参考答案 |

返回

```
31 //返回新增的习题ID
```

### 9.5共享习题修改

exercise/edit_exercise

请求

| 参数             | 类型     | 是否必须 | 说明     |
| -------------- | ------ | ---- | ------ |
| type_id        | int    | Y    | 习题类型   |
| content        | sting  | Y    | 习题描述   |
| correct_answer | string | Y    | 习题参考答案 |
| id             | int    | Y    | 习题ID   |

返回

```
1 // 返回影响的习题数量  0或者1
```

### 9.6共享习题删除

exercise/del_exercise

请求

| 参数   | 类型   | 是否必须 | 说明   |
| ---- | ---- | ---- | ---- |
| id   | int  | Y    | 习题ID |

返回

```
1 // 返回影响的习题数量  0或者1
```

### 9.7习题类型

exercise/get_type

请求

| 参数   | 类型   | 是否必须 | 说明          |
| ---- | ---- | ---- | ----------- |
| type | int  | N    | 默认为0，返回三种类型 |

返回

```
[
    {
      "id": "1",
      "title": "选择题",
      "option": "{\"1\":\"A\", \"2\":\"B\", \"3\":\"C\", \"4\":\"D\"}"
    },
    {
      "id": "2",
      "title": "判断题",
      "option": "{\"1\":\"对\", \"2\":\"错\"}"
    },
    {
      "id": "3",
      "title": "主观题",
      "option": "{\"10\":\"提交\"}"
    }
  ]
```

### 9.8习题答案修改

exercise/update_answer

请求

| 参数             | 类型     | 是否必须 | 说明                   |
| -------------- | ------ | ---- | -------------------- |
| id             | int    | Y    | 习题ID                 |
| correct_answer | string | Y    | 习题答案,统一使用数组。列如:  [2] |

返回

```
{
  "stat": 0,
  "msg": "",
  "data": 1
}

注：
data为1代表修改成功,为0代表修改失败
```

错误返回

| 错误码   | 说明     |
| ----- | ------ |
| 11002 | 数据不存在  |
| 20003 | 参数格式错误 |
|       |        |





## 10 上传(upload)

### 10.1 获取banner

upload/getbanner

请求

| 参数   | 类型   | 是否必须 | 说明   |
| ---- | ---- | ---- | ---- |
|      |      |      |      |

返回

```json
{
  "stat": 0,
  "msg": "",
  "data": [
    "dev17.ubanjiaoyu.com/uploads/banner/banner1.png",
    "dev17.ubanjiaoyu.com/uploads/banner/banner2.png"
  ]
}
```


## 11 更新apk(update)

### 11.1 检测新版本(version)

请求url: update/version

请求方式：GET

请求参数：

| 参数           | 类型     | 是否必须 | 格式                | 说明    |
| ------------ | ------ | ---- | ----------------- | ----- |
| version_name | string | Y    | version_name/v1.3 | 当前版本号 |

返回json数据：

```json

{
    "stat":0,
    "msg":"当前版本是最新版本！",
    "data":{
        "version_name":"v1.3",
        "description":"sfdfads",
        "file_url":"http://d17.com/organize/api/update/download/version/v1.3",
        "have_new":0
    }
}
```
返回参数说明：

| 参数           | 类型     | 说明      |
| ------------ | ------ | ------- |
| version_name | string | 最新版本号   |
| description  | string | 描述      |
| file_url     | string | 文件下载url |
| have_new     | string | 新版本个数   |



### 11.2 下载指定版本（download）

请求url: update/download

请求方式：GET

请求参数：

| 参数           | 类型     | 是否必须 | 格式                | 说明                  |
| ------------ | ------ | ---- | ----------------- | ------------------- |
| version_name | string | N    | version_name/v1.3 | 指定版本号，没有提交参数则下载最新版本 |

返回json数据：

```json

{
    "stat":20001,
    "msg":"没有该文件",
    "data":null
}
```
返回说明：如果没有错误的话，直接下载apk文件


### 11.3 上传apk页面（uploadTemp）

请求url: update/uploadTemp

上传说明：上传的apk文件命名规则： uban_版本号.apk 例如：uban_1.3.apk





## 12 课堂

### 12.1 开始课堂

testing/start  

```
cwc_id     课程对应班级id 必须，且必须为整数

```

data

```
M {
    I teacher_id:教职工id
    I class_id:班级ID
    S class_title:班级名称
    I testing:课堂ID
    S testing_stat:课堂状态 N|Y
    S testing_start_time:课堂开始时间
    I index:第几轮互动
    I interact:习题互动ID
    S interact_stat:互动状态 "N"|"Y"|""|"S" 注2
    S interact_content:互动题目内容
    I interact_setting_completed:互动是否已设置答案(0:未设置|1:已设置）
    I interact_type:互动题目类型（0：选择题|1：判断题|2：书写题）
    S interact_type_title:互动类型标题
    I interact_time:互动时间(已经开始了多久 单位:秒)
    S interact_custom_exercise_set:自定义习题集内容(json数组形式）
    L interact_correct_answer_set:随堂练习答案
    I doing_exercise_set_id:习题集ID
    S doing_exercise_set_title:习题集标题
    I doing_exercise_id:习题ID
    L doing_exercise_set:习题集
    I doing_exercise_index:习题集游标
    L student_list:学生列表 注1
    I doing_exercise_mode:互动模式, 0纯互动,1习题互动,2随堂练习，3.菁优网组卷答题卡互动,4:箐优网组卷互动模式
    L player:播放器列表
    I showpalyer:是否展示播放器（0：不展示|1：展示）
    I full_screen:是否全屏（0：不全屏|1：全屏）
    ARR jyeoo_subject 当前开课的科目，在菁优网支持的才会出现，具体科目代表值请看：  18箐优网组卷
}

注1:student_list:
 L [
      M {
        S id:学生id
        S gender:性别1为男
        S title:学生姓名 
        S device_uid:设备编号
        S student_id:学号
        I online:是否在线(0不在线|1在线）
        I answered:是否已经回答(0未回答|1已回答）
        I submited:已经提交的答案, 0: 未提交, 1-7: abcd yn commit
      }...
 ]     
注2: interact_stat
'S'|'':旧互动已结束, 新互动未开始(注意此时显示的index应+1), 'N'互动已经开始, 'Y'互动已经结束

注3:
interact_custom_exercise_set:
例子  [ 
    [1, [1]], 
    [2, [2, 3, 4]], 
    [3, [7]] ,
    [2, [1, 5]]
]
注: 按顺序[x][y][z] 下标x=0为第一题, 下标x=1为第二题, y=0为该题题型, y=1为该题答题

interact_correct_answer_set ： 
  [ 
      [1], 
      [2, 3, 4], 
      [7] ,
      [1, 5]
  ]
  注: 按顺序[x][y] 下标x=0为第一题, 下标x=1为第二题
  
  question_list:[
  {
  "title_img_url" :["http://dev17.ubanjiaoyu.com/upload/111.png",
                    "http://dev17.ubanjiaoyu.com/upload/111.png"
  ], // 增加习题拍照的照片，只有纯互动和随堂自定义模式才有，其他模式为空
  "thum_title_img_url":[
  					"http://dev17.ubanjiaoyu.com/upload/111.png",
  					"http://dev17.ubanjiaoyu.com/upload/111.png"
  ],  // 习题拍照缩略图
  }
  ]
```

### 12.2 查看课堂状态

testing/get_status

```

```

data

```
同12.1data
```

### 12.3 开始互动

testing/interact_start

```
exe_type_id:习题类型ID（1选择题|2判断题|3 主观题）,必须
exe_id:习题ID，没有就为0,必须
true_love:正确答案, 如果想以后在interact_set 里设置则传0, 或不传
//**************以上为普通互动参数

//*************以下为箐优网习题互动必须参数
jyeoo_exe_id: 菁优网习题ID
subject: 科目(具体科目请看第18) 例如：HIGH_PHYSICS
```

data

```
同12.1data
```

### 12.4 定义互动答案

testing/interact_set

```
correct_answer : 正确答案 选择题"1"|"2"|"3"|"4"  判断题 "1"|"2"  主观题为"10" ，必须
content : 习题内容,可以为空,可选
testing_id:课堂id，可选
staff_id:教职工id，可选
```

data

```
同12.1data
```

### 12.5 结束互动

testing/interact_finish

```
true_love : 正确答案 合并2.5.3 定义互动 的操作，可不传 选择题"1"|"2"|"3"|"4"  判断题 "5"|"6"  主观题为"7" 
```

data

```
同12.1data
```

### 12.6 标记已经确认统计

testing/interact_statistics_completed

```

```

data

```
同12.1data
```

### 12.7 结束课堂

testing/finish

请求参数：

| 参数            | 类型     | 是否必须 | 格式   | 说明                    |
| ------------- | ------ | ---- | ---- | --------------------- |
| title         | string | N    |      | 不传，则默认开课时间 班级名字       |
| staff_account | String | N    |      | 老师的账号，不提交，则默认当前登录老师账号 |



```
title : 课堂标题
```

data

```
I 课堂id
```

### 12.8 取消课堂

testing/cancel

```

```

data

```
I 课堂id
```

### 12.9 选择习题集

testing/choose_set

```
exercise_set_id：习题集id，必须
```

data

```
同12.1 data
```

### 12.10 跳过当前题目

testing/choose_exe_next

```

```

data

```
同12.1 data
```

### 12.11 切换模式（1.7废弃该接口）

testing/model_change

```
pure：模式类型（0：纯互动|1：习题互动）
```

data

```
同12.1 data
```

### 12.12 取消互动

testing/interact_cancel

```

```

data

```
同12.1data

注：随堂练习的互动不能取消
```



### 12.13 答题分析

report/interact_answers

```
interact:互动id，必须
```

data

```
M {
    I true_love:正确答案/其实就是标记哪一排颜色不一样
    M option：备选项
    I answered:已经回答人数
    I student_count:学生人数
    M chart:各选项回答人数
}

注：
样例
{
    "true_love":"5",
    "option":{"5":"对","6":"错"},
    "answered":0,
    "student_count":2,
    "chart":{"5":0,"6":0}
}
```



### 12.14 答题时间分析

report/interact_answers_times

```
interact:互动id，必须
```

data

```
M {
    I true_love:正确答案/其实就是标记哪一排颜色不一样
    M option：备选项
    I answered:已经回答人数
    I student_count:学生人数
    M chart:各选项回答人数
}

注：
样例
{
    "true_love":0,
    "option":["03:57:58","07:55:56","11:53:54","15:51:52"],
    "answered":0,
    "student_count":2,
    "chart":{"03:57:58":0,"07:55:56":0,"11:53:54":0,"15:51:52":0}
}
```

### 7.15 答题排行榜

report/interact_answers_ranking

```
interact:互动id，必须
```

data

```
L [
    S title:学生名称
    I gender 性别
    S answer 回答答案
    S begin_time 开始回答时间
    S finish_time 结束回答时间
    S correct_answer 正确答案
    S times 答题用时
]...
```

### 12.16 开始随堂练习

testing/practice_start

```
exe_set_id:习题集id
custom_set：自定义习题集内容（json数组）

注：
custom_set:
例子  [ 
    [1], 
    [3], 
    [3] ,
    [4]
]
注: 按顺序[x][y][z] 下标x=0为第一题, 下标x=1为第二题, y=0为该题题型, y=1为该题答题，自定义习题集只有1：单选题|3：书写题|4：多选题

//***********以下为菁优网答题卡模式开始参数
jyeoo_exe_set_id： 题目ID
subject: 科目(具体格式，请看第18)
```

data

```
同12.1data
```

### 12.17 结束随堂练习

testing/practice_finish

```
correct_answer_set: 自定义习题集答案，选择现有习题集的随堂练习可以不传
[ 
    [1], 
    [2, 3, 4], 
    [7] ,
    [1, 5]
]
注: 按顺序[x][y] 下标x=0为第一题, 下标x=1为第二题
```

data

```
同12.1data
```

### 12.18 设定随堂练习答案

testing/practice_answer_set

请求参数

```
correct_answer_set: 自定义习题集答案
[ 
    [1], 
    [2, 3, 4], 
    [7] ,
    [1, 5]
]
注: 按顺序[x][y] 下标x=0为第一题, 下标x=1为第二题
```

data

```
同12.1data
```

### 12.19设置自定义随堂练习多道题单个答案

testing/practice_answer_set_one

请求

| 参数                 | 类型     | 是否必须 | 说明                        |
| ------------------ | ------ | ---- | ------------------------- |
| index              | int    | Y    | 题目序号，从0开始                 |
| correct_answer_set | string | Y    | 正确答案：数组格式，如 [1]，多选题为[2,3] |

返回

```
{
  "stat": 0,
  "msg": "",
  "data": 1
}
注
data为1代表修改成功，为0代表修改失败
```

错误返回

| 错误码  | 说明         |
| ---- | ---------- |
| 1    | 只要是1就是修改失败 |

### 12.20 获取本次课堂的使用的习题

report/exercise_set_questions

请求

|          参数           |   类型   | 是否必须 |  说明  |
| :-------------------: | :----: | :--: | :--: |
| doing_exercise_set_id | String |  y   | 课堂id |

返回

```json
{
    "stat": 0,
    "msg": "获取成功",
    "data": [
        {
            "questions_lists": [
                {
                  	"interact_id":12354 // 互动ID
                    "type": 4, // 题目类型 1单选题，2判断题，3书写题，4多选题
                    "correct_answer_set": [ // 正确答案12345=abcde, 12=对错， 10=提交
                        1,
                        2,
                        3
                    ],
                    "content": "<p>多选题</p>", // 题目
                  	"title_img_url" :["http://dev17.ubanjiaoyu.com/upload/111.png",
                    					  "http://dev17.ubanjiaoyu.com/upload/111.png"
                    ], // 增加习题拍照的照片，只有纯互动和随堂自定义模式才有，其他模式为空
                    "thum_title_img_url":[
                          					"http://dev17.ubanjiaoyu.com/upload/111.png",
                    					    "http://dev17.ubanjiaoyu.com/upload/111.png"
                    ],  // 习题拍照缩略图
                },
                {
                    "type": 3,
                    "correct_answer_set": [
                        10
                    ],
                    "content": "<p>书写题</p>"
                },
                {
                    "type": 2,
                    "correct_answer_set": [
                        1
                    ],
                    "content": "<p>判断题</p>"
                },
                {
                    "type": 1,
                    "correct_answer_set": [
                        2
                    ],
                    "content": "<p>单选题</p>"
                }
            ],
            "interact_type": 2 // 互动类型 0纯互动，1习题互动，2随堂
        },
        {
            "questions_lists": [
                {
                    "type": 1,
                    "content": "<p>单选题</p>",
                    "correct_answer_set": [
                        1
                    ]
                }
            ],
            "interact_type": 1
        }
    ]
}
```
### 12.21 获取本次互动的使用的习题

report/interact_questions

请求

|     参数      |   类型   | 是否必须 |  说明  |
| :---------: | :----: | :--: | :--: |
| interact_id | String |  y   | 互动id |

返回

```json
{
    "stat": 0,
    "msg": "获取成功",
    "data": {
        "questions_lists": [
            {
                "type": 4, // 类型同12.20
                "correct_answer_set": [
                    1,
                    2,
                    3
                ],
                "content": "<p>多选题</p>",
              	"title_img_url" :["http://dev17.ubanjiaoyu.com/upload/111.png",
                    					  "http://dev17.ubanjiaoyu.com/upload/111.png"
                    	], // 增加习题拍照的照片，只有纯互动和随堂自定义模式才有，其他模式为空
                "thum_title_img_url":[
                          					"http://dev17.ubanjiaoyu.com/upload/111.png",
                    					    "http://dev17.ubanjiaoyu.com/upload/111.png"
                    	],  // 习题拍照缩略图
            },
            {
                "type": 3,
                "correct_answer_set": [
                    10
                ],
                "content": "<p>书写题</p>"
            },
            {
                "type": 2,
                "correct_answer_set": [
                    1
                ],
                "content": "<p>判断题</p>"
            },
            {
                "type": 1,
                "correct_answer_set": [
                    2
                ],
                "content": "<p>单选题</p>"
            }
        ],
        "interact_type": 2
    }
}
```
### 12.22 为互动中的题上传照片

report/upload_questions_title_img

请求

|           参数            |   类型   | 是否必须 |                    说明                    |
| :---------------------: | :----: | :--: | :--------------------------------------: |
|       interact_id       | String |  y   |                   互动id                   |
| interact_question_index | string |  y   | 互动中题目的下标，以0开头,多个题目以逗号分隔，全部题目都设置请上传'all'例如：'1,2,3' |
|   question_title_img    |  file  |  y   |                  上传的图片                   |
|           cut           |  Int   |  n   |          是否服务器压缩图片1代表压缩，0为代表不压缩          |

返回

```json
{
    "stat": 0,
    "msg": "更新成功",
    "data": [
        {
            "title_img_url": [
                "http://local.ubanjiaoyu.com/uploads/interact_question_title/20170708/nV98iN0vG2vR2h5pA6Dx4SBenu3ZjEdx.png"
            ],
            "thumb_title_img_url": [
                "http://local.ubanjiaoyu.com/uploads/interact_question_title/20170708/thumb-nV98iN0vG2vR2h5pA6Dx4SBenu3ZjEdx.png"
            ]
        },
        {
            "title_img_url": [],
            "thumb_title_img_url": []
        },
        {
            "title_img_url": [
                "http://local.ubanjiaoyu.com/uploads/interact_question_title/20170708/nV98iN0vG2vR2h5pA6Dx4SBenu3ZjEdx.png"
            ],
            "thumb_title_img_url": [
                "http://local.ubanjiaoyu.com/uploads/interact_question_title/20170708/thumb-nV98iN0vG2vR2h5pA6Dx4SBenu3ZjEdx.png"
            ]
        }
    ]
}
```



### 12.23 删除互动中的题目的图片

report/del_questions_title_img

请求

|      参数       |   类型   | 是否必须 |                    说明                    |
| :-----------: | :----: | :--: | :--------------------------------------: |
|  interact_id  | String |  y   |                   互动id                   |
| question_info | String |  y   | json字符串格式：例如：  {"0":[0], "1": [0]:"2":[0]}代表删除互动中第1，2，3道题的第一张图片 |

返回

```json
{
    "stat": 0,
    "msg": "删除成功",
    "data": [
        {
            "title_img_url": [],
            "thumb_title_img_url": []
        },
        {
            "title_img_url": [],
            "thumb_title_img_url": []
        },
        {
            "title_img_url": [],
            "thumb_title_img_url": []
        }
    ]
}
```

#### 12.24 验证是否能上课（针对老师更换班级所带来的问题，在历史班级中调用）

staff/check_begin

请求：

| 参数       | 类型   | 是否必须 | 说明                  |
| -------- | ---- | ---- | ------------------- |
| class_id | int  | Y    | 班级id                |
| staff_id | int  | N    | 老师id，不传 ，取 当前登录老师id |

返回：

```
{
  "stat": 120001,
  "msg": "不能上课，历史班级",
  "data": []
}
```



```
{
  "stat":0,
  "msg": "successful",
  "data": []
}
```

### 12.25 删除互动中的上课记录

report/del_history

请求:

| 参数         | 类型   | 是否必须 | 说明   |
| ---------- | ---- | ---- | ---- |
| testing_id | int  | Y    | 互动id |

返回:

```
{
  "stat": 0,
  "msg": "成功",
  "data": []
}
```







## 13密码

### 13.1忘记密码、修改密码通用接口（通过短信验证，然后修改密码）

passwd/forget

请求

| 参数     | 类型     | 是否必须 | 说明   |
| ------ | ------ | ---- | ---- |
| mobile | string | Y    | 手机号码 |

正确返回

```
{
  "stat": 0,
  "msg": "短信发送成功",
  "data": {
    "expire": 120
  }
}
```

错误返回

| 返回码   | 说明      |
| ----- | ------- |
| 30001 | 手机号格式错误 |
| 30000 | 没有该用户   |
| 30007 | 请不要重复发送 |
| 30008 | 发送失败    |



### 13.2验证码验证

passwd/check_code

请求

| 参数        | 类型     | 是否必须 | 说明   |
| --------- | ------ | ---- | ---- |
| mobile    | string | Y    | 手机号  |
| code      | string | Y    | 验证码  |
| school_id | string | Y    | 学校ID |

返回

```
{
  "stat": 0,
  "msg": "验证成功",
  "data": {
    "set_pwd_token":"2f5e93b6a33b587b4b6c5c884ba1162b"
  }
}
```

错误返回

| 错误码   | 说明     |
| ----- | ------ |
| 30009 | 无效的验证码 |
| 30003 | 验证码超时  |
| 30008 | 验证码错误  |

### 13.3根据token重置密码

passwd/reset

请求

| 参数            | 类型     | 是否必须 | 说明      |
| ------------- | ------ | ---- | ------- |
| set_pwd_token | string | Y    | 重置密码的凭证 |
| new_passwd    | string | Y    | 新密码     |

返回

```
{
  "stat": 0,
  "msg": "操作成功",
  "data":true
}
```

错误返回

| 错误码   | 说明                 |
| ----- | ------------------ |
| 13004 | 新密码格式错误            |
| 30006 | set_pwd_token错误、无效 |
| 30005 | 超时，请重新找回密码         |
| -1    | 操作失败               |

## 14邮件

### 14.1老师绑定邮箱

email/binding

请求

| 参数    | 类型     | 是否必须 | 说明   |
| ----- | ------ | ---- | ---- |
| email | string | Y    | 邮箱地址 |

返回

```
1 //失败返回0，成功返回1

成功如下
{
  "stat": 0,
  "msg": "",
  "data": 1
}

失败
{
  "stat": 0,
  "msg": "",
  "data": 0
}
```

错误返回

| 错误码   | 说明    |
| ----- | ----- |
| 30012 | 邮箱已存在 |

### 14.2老师邮箱绑定

email/check_binding

请求

| 参数          | 类型     | 是否必须 | 说明     |
| ----------- | ------ | ---- | ------ |
| email_token | string | Y    | 邮箱绑定凭证 |

返回

```
成功：
{
  "stat": 0,
  "msg": "",
  "data": true
}
失败：
{
  "stat": 0,
  "msg": "",
  "data": false
}
```

错误返回

| 错误码   | 说明        |
| ----- | --------- |
| 1     | 非法访问      |
| 30010 | 邮箱链接地址已失效 |

### 14.3邮箱发送通用接口

email/send

请求

| 参数       | 类型                  | 是否必须 | 说明               |
| -------- | ------------------- | ---- | ---------------- |
| address  | string  \|\|  array | Y    | 邮箱地址，多个地址，使用数组存储 |
| title    | string              | Y    | 邮件标题             |
| message  | string              | Y    | 正文内容，可以是html     |
| fromname | string              | N    | 邮件发送方名称。默认为“优伴教育 |

返回

```
true //失败返回false，注意：不反悔stat状态码
```



## 15 书写记录

### 15.1 获取书写内容

writing/get_student_writing_content

请求方式：post

作者：vans

请求参数

| 参数                | 类型     | 是否必须 | 说明     |
| ----------------- | ------ | ---- | ------ |
| answer_writing_id | string | Y    | 书写记录id |

返回

```
{
    "stat": 0,
    "msg": "记录获取成功",
    "data": {
        "content": "{\"cmd\":\"penup\",\"po\":{\"x\":0,\"y\":0},\"time\":1495010752531}\n,{\"cmd\":\"penup\",\"po\":{\"x\":-2247,\"y\":2235},\"time\":1495010753182}" // 书写记录
    }
}
```

## 15 书写记录

### 15.1 获取书写内容

writing/get_student_writing_content

请求方式：post

作者：vans

请求参数

| 参数                | 类型     | 是否必须 | 说明     |
| ----------------- | ------ | ---- | ------ |
| answer_writing_id | string | Y    | 书写记录id |

返回

```
{
    "stat": 0,
    "msg": "记录获取成功",
    "data": {
        "content": "{\"cmd\":\"penup\",\"po\":{\"x\":0,\"y\":0},\"time\":1495010752531}\n,{\"cmd\":\"penup\",\"po\":{\"x\":-2247,\"y\":2235},\"time\":1495010753182}" // 书写记录
    }
}
```

### 15.2 获取书写记录个数按天显示

Writing/get_writing_log_count_by_date

请求方式：post

作者：vans

请求参数

| 参数         | 类型   | 是否必须 | 说明    |
| ---------- | ---- | ---- | ----- |
| end_date   | int  | Y    | 结束时间戳 |
| start_date | int  | Y    | 开始时间戳 |

返回

```
{
    "stat": 0,
    "msg": "获取成功",
    "data": [
        { // 没有LOG的日期不会输出
            "date": "2017-05-11", // 日期
            "log_count": 20 // 书写记录个数
        },
        {
            "date": "2017-05-12",
            "log_count": 5
        }
    ]
}
```

### 15.3 获取书写记录列表

student/writing_log

请求方式：post

作者：vans

请求参数

| 参数              | 类型   | 是否必须 | 说明    |
| --------------- | ---- | ---- | ----- |
| end_timestamp   | int  | Y    | 结束时间戳 |
| start_timestamp | int  | Y    | 开始时间戳 |
| page_index      | int  | Y    | 页数    |

返回

```
{
    "stat": 0,
    "msg": "获取成功",
    "data": [
        {
            "writing_log_id": 969, // 书写记录ID
            "device_uid": "35321dd1",
            "stu_id": 5560,
            "content": "[{\"cmd\":\"pen\",\"po\":{\"x\":-2422,\"y\":6474},\"time\":1494584027568}\n,{\"cmd\":\"pen\",\"po\":{\"x\":-2439,\"y\":6469},\"time\":1494584027593}\n]",
            "create_time": 1494584015, // 开始时间
            "exercise_set_id": 0, // 课堂id 为0代表课外记录
            "end_time": 1494584029 // 结束时间
        }
    ]
}
```

## 16 有优笔

### 16.1 获取笔的使用记录

pen/log

请求方式：post

作者：vans

请求参数：无

返回

```
{
    "stat": 0,
    "msg": "获取成功",
    "data": [
        {
            "id": 558, // 记录id
            "stu_id": 5560, // 学生id
            "device_uid": "35321dd1", // 板id
            "is_bind": 0, //是否正在绑定
            "create_time": "2017-05-12 18:13:18" // 创建时间
        },
        {
            "id": 336,
            "stu_id": 5560,
            "device_uid": "3532b777",
            "is_bind": 0,
            "create_time": "2017-05-05 17:24:36"
        }
    ]
}
```

## 17 微课

### 17.1 结束微课

tiny_test/end

请求方式：post

作者：vans

请求参数：

| 参数         | 类型     | 是否必须 | 说明                                       |      |      |
| ---------- | ------ | ---- | ---------------------------------------- | ---- | ---- |
| tiny_id    | int    | Y    | 微课ID                                     |      |      |
| title      | String | Y    | 标题                                       |      |      |
| total_time | int    | Y    | 视频时间                                     |      |      |
| rub        | Json   | n    | 橡皮檫，例如[ {"po":{"x":-2912,"y":2098},"time":1498183773582}] |      |      |
| pause      | Json   | n    | 暂停时间，例如 [{\"start\":1498183773582,\"end\":1498183773582}] |      |      |
| img_info   | Json   | n    | 照片ID,例如[123456,123457]                   |      |      |
| udio_info  | Json   | n    | 音频ID,例如[123456,123457]                   |      |      |
| is_public  | Int    | y    | 是否公开 1代表公开，0代表私有                         |      |      |
| game_id    | Int    | y    | 是否参加比赛 1代表参加，0代表不参加                      |      |      |
| cover_img  | file   | Y    | 封面图片                                     |      |      |
| end_time   | sting  | y    | 结束时间戳                                    |      |      |
| time_diff  | Int    | n    | 音频与笔记时间戳MS                               |      |      |



返回

```
{
    "stat": 0,
    "msg": "结束成功",
    "data": ""
}
```

### 17.2 开始微课

tiny_test/start

请求方式：post

作者：vans

请求参数：

| 参数         | 类型     | 是否必须 | 说明   |
| ---------- | ------ | ---- | ---- |
| title      | String | Y    | 标题   |
| start_time | String | y    | 开始时间 |



返回

```
{
    "stat": 0,
    "msg": "开始成功",
    "data": {
        "title": "我的微课凤飞飞",
        "total_time": 0, // 总时间
        "rub": [], // 橡皮檫
        "pause": [], // 暂停
        "start_time": 1500373906, // 开始录课时间
        "end_time": 0, // 结束时间为0:暂无
        "status": 2, // 状态 1代表正常，2代表录课中，0代表已删除
        "create_time": 1500373906, // 创建时间
        "staff_id": 262, // 老师ID
        "id": "16" // 微课ID
    }
}
```



### 17.3 上传图片

tiny_test/push_img

请求方式：post

作者：vans

请求参数：

| 参数         | 类型     | 是否必须 | 说明     |
| ---------- | ------ | ---- | ------ |
| img        | File   | Y    | 图片     |
| start_time | String | y    | 开始播放时间 |
| x          | Int    | y    |        |
| y          | Int    | y    |        |
| width      | Int    | y    |        |
| height     | Int    | y    |        |



返回

```
{
    "stat": 0,
    "msg": "上传成功",
    "data": {
        "id": "gjet7kMtG31KyFM74N9TNC3IehPpmXSPcHFbScRiEIitCYptiTzyxAk1xhMvm32z"
    }
}
```



### 17.4 上传音频

tiny_test/push_audio

请求方式：post

作者：vans

请求参数：

| 参数         | 类型     | 是否必须 | 说明     |
| ---------- | ------ | ---- | ------ |
| audio      | File   | Y    | 音频     |
| start_time | String | y    | 开始播放时间 |



返回

```
{
    "stat": 0,
    "msg": "上传成功",
    "data": {
        "id": "gjet7kMtG31KyFM74N9TNC3IehPpmXSPcHFbScRiEIitCYptiTzyxAk1xhMvm32z"
    }
}
```





### 17.5 获取微课信息

tiny_test/get_info

请求方式：post

作者：vans

请求参数：

| 参数   | 类型   | 是否必须 | 说明o  |
| ---- | ---- | ---- | ---- |
| id   | Int  | Y    | 微课ID |



返回

```
{
    "stat": 0,
    "msg": "获取成功",
    "data": {
        "id": 14, // 微课ID
        "title": "VANS微课", //标题
        "start_time": 1500371734, // 开始时间
        "end_time": 1500371999, // 结束时间
        "total_time": "222", // 总播放时间
        			"cover_img_url"："http://local.ubanjiaoyu.com:80/uploads/tiny_testing_img/20170718/79f507cfebbb56a66a65a5e30123e5da.png"
        "rub": [
             {"po":{"x":-2912,"y":2098},"time":1498183773582} // 橡皮檫时间
        ],
        "pause": [
            {"start":1498183773582,"end":1498183773582} // 暂停时间端
        ],
        "status": 1, // 状态 1代表正常，2代表录课中，0代表已删除
        "create_time": 1500371734, //建时间
        "staff_id": 262, // 老师ID
        "staff_name": "伍老师",
        "visiting_card": "我是一名老师，专注于数学系",
        "qr_code_url": "http://local.ubanjiaoyu.com:80/uploads/tiny_testing_img/20170725/2b7d387ce87666a5316999337d9a198d.jpeg",
        "can_thumb": true,
        "is_public": 1, // 公开，1代表公开，0代表私有
        "game_id": 0, // 比赛ID
        "data": [ // 书写笔记
            []
        ],
        "img": [ // 图片资源
            {
                "url": "http://local.ubanjiaoyu.com:80/public/uploads/tiny_testing_img/20170718/79f507cfebbb56a66a65a5e30123e5da.png",
                "start_time": 2746583276, // 开始插入时间
                "draw": {
                    "x": 200, // 坐标x
                    "y": 400, //坐标y
                    "width": 11,  //宽度
                    "height": 44 // 高度
                }
            }
        ],
        "audio": [
            {
                "url": "http://local.ubanjiaoyu.com:80/public/uploads/tiny_testing_audio/20170718/5d3246d6ffd6827f4d33469962dda560.silk",
                "start_time": 2746583276 // 音频开始播放时间
            }
        ]
    }
}
```



### 17.6 小程序微信登录检测

sign/wx_app_bind_check	

请求方式：post

作者：vans

请求参数：

| 参数            | 类型     | 是否必须 | 说明o               |
| ------------- | ------ | ---- | ----------------- |
| code          | String | Y    | 小程序code           |
| encryptedData | String | y    | 小程序加密串，后端解密得到用户信息 |
| iv            | String | y    | 小程序解密             |



返回

```
{
    "stat": 0,
    "msg": "已关联微信用户",
    "data": {
        "token": "xvsu68J4R19SKvwXhSgA4bMnlgTBuiJQ"
    }
}
```



### 17.7 检测能否参赛

tiny_test/check_join_game	

请求方式：post

作者：vans

请求参数：

| 参数   | 类型   | 是否必须 | 说明o       |
| ---- | ---- | ---- | --------- |
| game | Int  | n    | 默认为1，比赛id |



返回

```
{
    "stat": 0,
    "msg": "可以参赛",
    "data": ""
}
```



### 17.8 点赞

tiny_test/thumb_up

请求方式：post

作者：vans

请求参数：

| 参数              | 类型     | 是否必须 | 说明   |
| --------------- | ------ | ---- | ---- |
| tiny_testing_id | String | y    | 微课ID |



返回

```
{
    "stat": 0,
    "msg": "点赞信息获取成功",
    "data": ""
}
```

### 17.9 取消点赞

tiny_test/cancel_thumb_up

请求方式：post

作者：vans

请求参数：

| 参数              | 类型     | 是否必须 | 说明   |
| --------------- | ------ | ---- | ---- |
| tiny_testing_id | String | y    | 微课ID |



返回

```
{
    "stat": -1,
    "msg": "请点赞再取消",
    "data": ""
}
```





### 17.10 投票

tiny_test/vote_game

请求方式：post

作者：vans

请求参数：

| 参数              | 类型     | 是否必须 | 说明                     |
| --------------- | ------ | ---- | ---------------------- |
| tiny_testing_id | String | y    | 微课ID                   |
| game_id         | Int    | n    | 默认为1,有于目前只有一场比赛，所以不填即可 |



返回

```
{
    "stat": 0,
    "msg": "投票成功",
    "data": {
        "vote_count": 1 // 当前投票数
    }
}
```



### 17.11 获取视频广场列表

tiny_test/get_square_list

请求方式：post

作者：vans

请求参数：

| 参数        | 类型     | 是否必须 | 说明                                       |
| --------- | ------ | ---- | ---------------------------------------- |
| type      | String | y    | 列表类型 ：hot代表热门，new代表最新，view_history代表已观看历史 |
| search    | string | n    | 搜索关键字                                    |
| Page      | Int    | n    | 第几页                                      |
| Page_rows | Int    | n    | 每页大小                                     |



返回

```
{
    "total": 1,
    "count": 1,
    "page_index": 1,
    "page_size": 10,
    "page_count": 1,
    "result": [
        {
            "id": 12,
            "title": "我的微课2",
            "start_time": 1500369892,
            "end_time": 0,
            "total_time": "0",
            "create_time": 1500369892, // 视频创建时间
            "staff_id": 0, // 老师ID
            "is_public": 1, // 是否公开
            "game_id": 0, // 比赛ID
            "cover_img_url": "", // 封面URL
            "thumb_count": 0, 已点赞个数
            "vote_count": 0, // 已投票个数
            "can_thumb": true, // 我能否点赞
            "can_vote": false, // 我能否投票
            "view_time": 123467788 // 我曾经观看的时间，注意，只有TYPE为read_history才有改字段
        }
    ]
}
```



### 17.12 获取个人视频列表

tiny_test/my_tiny_test_list

请求方式：post

作者：vans

请求参数：

| 参数        | 类型     | 是否必须 | 说明                                   |
| --------- | ------ | ---- | ------------------------------------ |
| type      | String | y    | 列表类型 ：private代表私有，public代表公有，all代表全部 |
| search    | string | n    | 搜索关键字                                |
| Page      | Int    | n    | 第几页                                  |
| Page_rows | Int    | n    | 每页大小                                 |



返回

```
{
    "stat": 0,
    "msg": "获取成功",
    "data": {
        "total": 4,
        "count": 4,
        "page_index": 1,
        "page_size": 10,
        "page_count": 1,
        "result": [
            {
                "id": 16,
                "title": "我的微课凤飞飞",
                "start_time": 1500373906,
                "end_time": 0,
                "total_time": "0",
                "create_time": 1500373906,
                "staff_id": 262,
                "is_public": 1,
                "game_id": 1,
                "cover_img_url": "",
                "thumb_count": 0,
                "vote_count": 0,
                "can_thumb": true,
                "can_vote": true
            },
            {
                "id": 15,
                "title": "我的微课凤飞飞",
                "start_time": 1500373809,
                "end_time": 0,
                "total_time": "0",
                "create_time": 1500373809,
                "staff_id": 262,
                "is_public": 1,
                "game_id": 0,
                "cover_img_url": "",
                "thumb_count": 0,
                "vote_count": 0,
                "can_thumb": true,
                "can_vote": false
            },
            {
                "id": 14,
                "title": "VANS微课",
                "start_time": 1500371734,
                "end_time": 1500371999,
                "total_time": "222",
                "create_time": 1500371734,
                "staff_id": 262,
                "is_public": 1,
                "game_id": 0,
                "cover_img_url": "",
                "thumb_count": 0,
                "vote_count": 0,
                "can_thumb": true,
                "can_vote": false
            },
            {
                "id": 13,
                "title": "VANS微课",
                "start_time": 1500370247,
                "end_time": 1500370311,
                "total_time": "222",
                "create_time": 1500370247,
                "staff_id": 262,
                "is_public": 1,
                "game_id": 0,
                "cover_img_url": "",
                "thumb_count": 0,
                "vote_count": 0,
                "can_thumb": false,
                "can_vote": false
            }
        ]
    }
}
```



### 17.13 获取个人名片

tiny_test/get_visiting_card

请求方式：post

作者：vans

请求参数：

无



返回

```
{
    "stat": 0,
    "msg": "获取成功",
    "data": {
        "visiting_card": "我是一名老师，专注于数学系",
        "qr_code_url": "http://local.ubanjiaoyu.com:80/uploads/tiny_testing_img/20170725/2b7d387ce87666a5316999337d9a198d.jpeg"
    }
}
```



### 17.14 修改个人名片

tiny_test/update_visiting_card

请求方式：post

作者：vans

请求参数：

| 参数            | 类型     | 是否必须 | 说明       |
| ------------- | ------ | ---- | -------- |
| visiting_card | String | N    | 修改内容     |
| qr_code_img   | FILE   | N    | 二维码图片    |
| wx_img_id     | String | n    | 微信临时文件ID |





返回

```
{
    "stat": 0,
    "msg": "更新成功",
    "data": ""
}
```



### 17.15 获取某人的精彩视频

tiny_test/get_wonder_list

请求方式：post

作者：vans

请求参数：

| 参数        | 类型   | 是否必须 | 说明   |
| --------- | ---- | ---- | ---- |
| staff_id  | Int  | y    | 老师ID |
| page      | int  | N    | 第几页  |
| Page_rows | Int  | n    | 一页大小 |





返回

```
{
    "stat": 0,
    "msg": "获取成功",
    "data": {
        "total": 4,
        "count": 4,
        "page_index": 1,
        "page_size": 10,
        "page_count": 1,
        "result": [
            {
                "id": 16,
                "title": "我的微课凤飞飞",
                "start_time": 1500373906,
                "end_time": 0,
                "total_time": "0",
                "create_time": 1500373906,
                "staff_id": 262,
                "is_public": 1,
                "game_id": 1,
                "cover_img_url": "",
                "thumb_count": 0,
                "vote_count": 0,
                "can_thumb": true,
                "can_vote": true
            },
            {
                "id": 15,
                "title": "我的微课凤飞飞",
                "start_time": 1500373809,
                "end_time": 0,
                "total_time": "0",
                "create_time": 1500373809,
                "staff_id": 262,
                "is_public": 1,
                "game_id": 0,
                "cover_img_url": "",
                "thumb_count": 0,
                "vote_count": 0,
                "can_thumb": true,
                "can_vote": false
            },
            {
                "id": 14,
                "title": "VANS微课",
                "start_time": 1500371734,
                "end_time": 1500371999,
                "total_time": "222",
                "create_time": 1500371734,
                "staff_id": 262,
                "is_public": 1,
                "game_id": 0,
                "cover_img_url": "",
                "thumb_count": 0,
                "vote_count": 0,
                "can_thumb": true,
                "can_vote": false
            },
            {
                "id": 13,
                "title": "VANS微课",
                "start_time": 1500370247,
                "end_time": 1500370311,
                "total_time": "222",
                "create_time": 1500370247,
                "staff_id": 262,
                "is_public": 1,
                "game_id": 0,
                "cover_img_url": "",
                "thumb_count": 0,
                "vote_count": 0,
                "can_thumb": false,
                "can_vote": false
            }
        ]
    }
}
```

### 17.16 获取比赛列表

tiny_test/join_game_list

请求方式：post

作者：vans

请求参数：

| 参数        | 类型     | 是否必须 | 说明    |
| --------- | ------ | ---- | ----- |
| search    | string | y    | 搜索关键字 |
| page      | int    | N    | 第几页   |
| page_rows | Int    | n    | 一页大小  |
| game_id   | Int    | n    | 默认1   |





返回

```
{
    "stat": 0,
    "msg": "获取成功",
    "data": {
        "total": 3,
        "count": 3,
        "page_index": 1,
        "page_size": 10,
        "page_count": 1,
        "result": [
            {
                "id": 16,
                "title": "我的微课凤飞飞",
                "start_time": 1500373906,
                "end_time": 0,
                "total_time": "0",
                "create_time": 1500373906,
                "staff_id": 262,
                "is_public": 1,
                "game_id": 1,
                "cover_img_url": "", // 封面
                "thumb_count": 0,
                "vote_count": 1,
                "can_thumb": true,
                "can_vote": true,
                "staff_name": "伍老师",
                "gender": 0, // 性别
                "headimgurl": // 微信头像 "http://wx.qlogo.cn/mmhead/T7kcKcDia5z7WrwnXFSI9wjic4dZib77W9Ot7ARNRn1jks/0",
                "ranking": 1 // 排名
                "game_join_id":1 // 参赛序号
            },
            {
                "id": 1,
                "title": "3333",
                "start_time": 0,
                "end_time": 0,
                "total_time": "0",
                "create_time": 0,
                "staff_id": 0,
                "is_public": 1,
                "game_id": 1,
                "cover_img_url": "",
                "thumb_count": 0,
                "vote_count": 0,
                "can_thumb": true,
                "can_vote": true,
                "staff_name": "",
                "gender": "",
                "headimgurl": "",
                "ranking": 2
            },
            {
                "id": 2,
                "title": "3333",
                "start_time": 0,
                "end_time": 0,
                "total_time": "0",
                "create_time": 1500356231,
                "staff_id": 0,
                "is_public": 1,
                "game_id": 1,
                "cover_img_url": "",
                "thumb_count": 0,
                "vote_count": 0,
                "can_thumb": true,
                "can_vote": true,
                "staff_name": "",
                "gender": "",
                "headimgurl": "",
                "ranking": 3
            }
        ]
    }
}
```

### 17.17 删除微课

tiny_test/del

请求方式：post

作者：vans

请求参数：

| 参数          | 类型     | 是否必须 | 说明             |
| ----------- | ------ | ---- | -------------- |
| tiny_id_csv | string | y    | 微课ID,格式123，123 |





返回

```
{
    "stat": 2000015,
    "msg": "请选择正常状态的微课",
    "data": ""
}
```



### 17.18 修改微课

tiny_test/update_tiny_testing

请求方式：post

作者：vans

请求参数：

| 参数              | 类型     | 是否必须 | 说明                                       |
| --------------- | ------ | ---- | ---------------------------------------- |
| tiny_testing_id | int    | y    | 微课ID                                     |
| type            | string | n    | 类型：join_game代表参加比赛，private代表私有，public代表公有 |
| title           | String | n    | 标题                                       |
| game_id         | int    | N    | 比赛ID 默认1                                 |





返回

```
{
    "stat": 0,
    "msg": "更新成功",
    "data": ""
}
```



### 17.19 添加微课阅读历史

tiny_test/add_view_list

请求方式：post

作者：vans

请求参数：

| 参数              | 类型   | 是否必须 | 说明   |
| --------------- | ---- | ---- | ---- |
| tiny_testing_id | int  | y    | 微课ID |





返回

```
{
    "stat": 0,
    "msg": "添加成功",
    "data": ""
}
```







## 18 菁优网组卷

subject科目值：

```
'PRIMARY_MATH'
'MIDDLE_MATH'
'MIDDLE_PHYSICS'
'MIDDLE_CHEMISTRY'
'MIDDLE_GEOGRAPHY'
'MIDDLE_ENGLISH'
'MIDDLE_CHINESE'
'MIDDLE_POLITICS'
'MIDDLE_HISTORY' 
'HIGH_MATH'
'HIGH_PHYSICS'
'HIGH_CHEMISTRY'
'HIGH_BIO'
'HIGH_GEOGRAPHY'
'HIGH_ENGLISH'
'HIGH_CHINESE'
'HIGH_POLITICS'
'HIGH_HISTORY'
```



### 18.1 获取已经组卷列表

jyeoo_exercise_set/lists

请求方式：post

作者：vans

请求参数：

| 参数        | 类型     | 是否必须 | 说明   |      |
| --------- | ------ | ---- | ---- | ---- |
| subject   | int    | Y    | 科目   |      |
| search    | String | n    | 搜索   |      |
| page      | int    | n    | 当前页码 |      |
| page_size | int    | n    | 每页大小 |      |



返回

```
{
    "stat": 0,
    "msg": "成功",
    "data": {
        "Count": 3,
        "PageIndex": 1,
        "PageSize": 10,
        "Data": [
            {
                "exe_set_id": "d7e0ceb3-2684-4582-aa03-e66870d81490",
                "create_time": 1503043126,
                "use_count": 0, // 使用次数
                "title": "高中物理组卷0007题",
                "ques_count": 7, //总题数
                "multi_select": 1, // 多选题个数
                "choose_number": 1, // 单选
                "write_number": 5,  // 书写
                "if_number": 0 // 判断
            },
            {
                "exe_set_id": "30378ec9-5bd8-4c44-8fe3-0e73892d3bc3",
                "create_time": 1503040019,
                "use_count": 0,
                "title": "高中物理组卷0004题",
                "ques_count": 4,
                "multi_select": 2,
                "choose_number": 1,
                "write_number": 1,
                "if_number": 0
            },
            {
                "exe_set_id": "5df7aaf7-924a-4918-96b1-b8779dcd2f07",
                "create_time": 1503038589,
                "use_count": 0,
                "title": "高中物理组卷0007题",
                "ques_count": 7,
                "multi_select": 1,
                "choose_number": 1,
                "write_number": 5,
                "if_number": 0
            }
        ]
    }
}
```



### 18.2 获取组卷内容

jyeoo_exercise_set/get_info

请求方式：*post*

作者：vans

请求参数：

| 参数        | 类型     | 是否必须 | 说明   |      |
| --------- | :----- | ---- | ---- | ---- |
| subject   | int    | Y    | ke   |      |
| search    | String | n    | 搜索   |      |
| paper_id  | int    | n    | 组卷ID |      |
| page_size | int    | n    | 每页大小 |      |



返回

```
{
    "title": "高中物理组卷0007题",
    "create_time": 1503043126,
    "question": [
      {
          "exercise_type_id": 1, //类型 1代表单选
          "type_title": "选择题",
          "id": "5ef4aa6e-a2b8-404b-abdc-76a84b22d648", // 习题ID
          "options": [ //选项
              "路程和位移",
              "速度和加速度",
              "力和功",
              "电场强度和电势"
          ],
          "Answers": [
              "1" // 菁优网正确答案选项
          ],
          "content": "下列各组物理量中均为矢量的是（　　）", // 题目
          "create_time": 1492077343, // 菁优网题目创建时间
          "correct_answer": [
              1 // 正确答案选项,翻译成有优板识别标识
          ]
      },
      {
          "exercise_type_id": 3,
          "type_title": "填空题",
          "options": [],
          "Answers": [
              "可以",
              "不可以"
          ],
          "content": "研究地球的公转时，<!--BA--><div class='quizPutTag' contenteditable='true'>&nbsp;</div><!--EA-->（填“可以”或“不可以”）把地球看做质点；研究地球的自转时，<!--BA--><div class='quizPutTag' contenteditable='true'>&nbsp;</div><!--EA-->（填“可以”或“不可以”）把地球看做质点．",
          "create_time": 1382527689,
          "correct_answer": [
              10
          ]
      },
      {
          "exercise_type_id": 4,
          "type_title": "多选题",
          "options": [
              "学校上午8点开始上第一节课，到8点45分下课",
              "小学每节课只有40分钟",
              "我走不动了，休息一下吧",
              "邢慧娜获得雅典奥运会女子1000m冠军，成绩是30分24秒36"
          ],
          "Answers": [
              "1",
              "2",
              "3"
          ],
          "content": "下列说法中，关于时间的是（　　）",
          "create_time": 1494918843,
          "correct_answer": [
              1,
              2,
              3
          ]
      },
      {
          "exercise_type_id": 3,
          "type_title": "作图题",
          "options": [],
          "Answers": [],
          "content": "如图所示，物体沿着曲线由A运动到B，在图上作出物体从A到B的位移矢量．<br /><img alt=\"菁优网\" src=\"http://img.jyeoo.net/quiz/images/201611/215/59bf7685.png\" style=\"vertical-align:middle\" />",
          "create_time": 1480070366,
          "correct_answer": [
              10
          ]
      },
      {
          "exercise_type_id": 3,
          "type_title": "实验题",
          "options": [],
          "Answers": [
              "AD",
              "天平",
              "刻度尺"
          ],
          "content": "<img alt=\"菁优网\" src=\"http://img.jyeoo.net/quiz/images/201607/113/9d6d1778.png\" style=\"vertical-align:middle;FLOAT:right;\" />某实验小组用如图所示的实验装置和实验器材做“探究功与速度变化的关系”实验，在实验中，该小组同学把砂和砂桶的总重力当作小车受到的合外力．<br />（1）为了保证实验结果的误差尽量小，在实验操作中，下面做法必要的是<!--BA--><div class='quizPutTag' contenteditable='true'>&nbsp;</div><!--EA-->．<br />A．实验前要对装置进行平衡摩擦力的操作<br />B．实验操作时要先放小车，后接通电源<br />C．在利用纸带进行数据处理时，所选的两个研究点离得越近越好<br />D．在实验过程中要保证砂和砂桶的总质量远小于小车的质量<br />（2）除实验装置中的仪器外，还需要的测量仪器有<!--BA--><div class='quizPutTag' contenteditable='true'>&nbsp;</div><!--EA-->，<!--BA--><div class='quizPutTag' contenteditable='true'>&nbsp;</div><!--EA-->．",
          "create_time": 1468482577,
          "correct_answer": [
              10
          ]
      },
      {
          "exercise_type_id": 3,
          "type_title": "计算题",
          "options": [],
          "Answers": [],
          "content": "一个物体先向正东以3m/s的速度前进了4s，又以3m/s的速度回头走了4s，试求：<br />（1）该物体在8s内的平均速度大小和方向；<br />（2）如果后4s不是回头，而是向正北方向，这8s时间内的平均速度大小和方向．",
          "create_time": 1491377995,
          "correct_answer": [
              10
          ]
      },
      {
          "exercise_type_id": 3,
          "type_title": "解答题",
          "options": [],
          "Answers": [
              "6V以下",
              "220V",
              "0.02",
              "匀加速直线",
              "0.80",
              "0.32"
          ],
          "content": "电磁打点计时器通常的工作电压为<!--BA--><div class='quizPutTag' contenteditable='true'>&nbsp;</div><!--EA-->，电火花打点计时器通常的工作电压为<!--BA--><div class='quizPutTag' contenteditable='true'>&nbsp;</div><!--EA-->．我们日常生活所用的交流电频率是50Hz，如果用它做电源，打点计时器每隔<!--BA--><div class='quizPutTag' contenteditable='true'>&nbsp;</div><!--EA-->s打一个点．<br />某同学在做“探究小车速度随时间变化的规律”实验时，得到了如图所示的一条较为理想的纸带，A、B、C、D、E、F为在纸带上依次所选的记数点，相邻记数点间有四个点未画出，测得AB=1.20cm，AC=3.20cm，AD=6.00cm，AE=9.60cm，AF=14.00cm，则以上由数据可知小车做<!--BA--><div class='quizPutTag' contenteditable='true'>&nbsp;</div><!--EA-->运动，运动的加速度a=<!--BA--><div class='quizPutTag' contenteditable='true'>&nbsp;</div><!--EA-->m/s<SUP>2</SUP>，打点计时器打下D点时小车的瞬时速度为<!--BA--><div class='quizPutTag' contenteditable='true'>&nbsp;</div><!--EA-->&nbsp;m/s．<br /><img alt=\"菁优网\" src=\"http://img.jyeoo.net/quiz/images/201112/85/56e30b3a.png\" style=\"vertical-align:middle\" />",
          "create_time": 1325070513,
          "correct_answer": [
              10
          ]
      }
  ]
}
```



### 18.3 组卷平台登录url

jyeoo_exercise_set/login_url

请求方式：post

作者：vans

请求参数：

| 参数   |      |      |      |      |
| ---- | ---- | ---- | ---- | ---- |
|      |      |      |      |      |



返回

```
{
    "stat": 0,
    "msg": "成功",
    "data": {
        "jyeoo_login_url": "http://uban.jyeoo.com/service/sign?id=34465686-1fd4-45dc-a5ef-04c95e1cd2db&token=FD8414401018AB81741D8C7C847301CC2D917F8B3FD272F7BA70DCC6E0247CBFE493CB8AA23E590D"
    }
}
```

### 18.4 获取组卷地区

jyeoo_exercise_set/get_regions

请求方式：post

作者：meeter

请求参数：无

返回：

```
{
  "stat": 0,
  "msg": "成功",
  "data": [
    {
      "ID": "1000000",
      "Name": "全国",
      "SName": "全国",
      "Type": -1,
      "PID": ""
    },
    {
      "ID": "1010000",
      "Name": "北京市",
      "SName": "北京",
      "Type": -1,
      "PID": ""
    },
    {
      "ID": "1010001",
      "Name": "东城区",
      "SName": "东城区",
      "Type": 3,
      "PID": "1010000"
    },
    {
      "ID": "1010002",
      "Name": "西城区",
      "SName": "西城区",
      "Type": 3,
      "PID": "1010000"
    },
    {
      "ID": "1010003",
      "Name": "崇文区",
      "SName": "崇文区",
      "Type": 3,
      "PID": "1010000"
    },
    {
      "ID": "1010004",
      "Name": "宣武区",
      "SName": "宣武区",
      "Type": 3,
      .......
```

### 18.5 获取组卷学科题型

jyeoo_exercise_set/get_books

请求方式：post

作者：meeter

请求参数:

| 参数      | 是否必须 | 类型     | 说明                |
| ------- | ---- | ------ | ----------------- |
| subject | Yes  | string | 科目请遵循（subject科目值） |

返回：

错误

```
{
  "stat": 600002,
  "msg": "该老师没有这个学科",
  "data": ""
}
```

正确：

```
{
  "stat": 0,
  "msg": "成功",
  "data": [
    {
      "Key": 1,
      "Value": "选择题"
    },
    {
      "Key": 2,
      "Value": "填空题"
    },
    {
      "Key": 3,
      "Value": "多选题"
    },
    {
      "Key": 4,
      "Value": "现代文阅读"
    },
    {
      "Key": 5,
      "Value": "文言文阅读"
    },
    {
      "Key": 6,
      "Value": "诗歌阅读"
    },
    {
      "Key": 7,
      "Value": "默写"
    },
    {
      "Key": 8,
      "Value": "语言表达"
    },
    {
      "Key": 9,
      "Value": "解答题"
    },
    {
      "Key": 10,
      "Value": "名著导读"
    },
    {
      "Key": 11,
      "Value": "作文"
    },
    {
      "Key": 12,
      "Value": "语言文字应用"
    }
  ]
}
```

### 18.6获取组卷学科（jyeoo支持的所有学科）

jyeoo_exercise_set/get_subjects

请求方式：post

作者：meeter

请求参数:无

返回：正确

```
{
  "stat": 0,
  "msg": "成功",
  "data": [
    [
      "10",
      "math3",
      "小学数学",
      "40"
    ],
    [
      "20",
      "math",
      "初中数学",
      "40"
    ],
    [
      "21",
      "physics",
      "初中物理",
      "40"
    ],
    [
      "22",
      "chemistry",
      "初中化学",
      "40"
    ],
    [
      "23",
      "bio",
      "初中生物",
      "40"
    ],
    [
      "25",
      "geography",
      "初中地理",
      "40"
    ],
    [
      "27",
      "english",
      "初中英语",
      "50"
    ],
    [
      "26",
      "chinese",
      "初中语文",
      "40"
    ],
    [
      "28",
      "politics",
      "初中政治",
      "40"
    ],
    [
      "29",
      "history",
      "初中历史",
      "40"
    ],
    [
      "30",
      "math2",
      "高中数学",
      "40"
    ],
    [
      "31",
      "physics2",
      "高中物理",
      "40"
    ],
    [
      "32",
      "chemistry2",
      "高中化学",
      "40"
    ],
    [
      "33",
      "bio2",
      "高中生物",
      "40"
    ],
    [
      "35",
      "geography2",
      "高中地理",
      "40"
    ],
    [
      "37",
      "english2",
      "高中英语",
      "50"
    ],
    [
      "36",
      "chinese2",
      "高中语文",
      "60"
    ],
    [
      "38",
      "politics2",
      "高中政治",
      "40"
    ],
    [
      "39",
      "history2",
      "高中历史",
      "40"
    ],
    [
      "42",
      "tech1",
      "高中信息",
      "40"
    ],
    [
      "43",
      "tech2",
      "高中通用",
      "40"
    ]
  ]
}
```

错误：

```
{
  "stat": 600006,
  "msg":'科目获取失败',
  "data": ""
}
```

### 18.7获取组卷学科来源

jyeoo_exercise_set/get_sources

请求方式：post

作者：meeter

请求参数:

参考18.5

返回：正确

```
{
  "stat": 0,
  "msg": "成功",
  "data": [
    {
      "Key": 1,
      "Value": "高考真题"
    },
    {
      "Key": 2,
      "Value": "学业考试"
    },
    {
      "Key": 3,
      "Value": "自主招生"
    },
    {
      "Key": 4,
      "Value": "高考模拟"
    },
    {
      "Key": 5,
      "Value": "高考复习"
    },
    {
      "Key": 6,
      "Value": "期末试题"
    },
    {
      "Key": 7,
      "Value": "期中试题"
    },
    {
      "Key": 8,
      "Value": "月考试题"
    },
    {
      "Key": 9,
      "Value": "单元测验"
    },
    {
      "Key": 10,
      "Value": "同步练习"
    },
    {
      "Key": 11,
      "Value": "竞赛试题"
    },
    {
      "Key": 12,
      "Value": "假期作业"
    },
    {
      "Key": 13,
      "Value": "模块试题"
    }
  ]
}
```

错误：

```
{
  "stat": 600007,
  "msg":'来源获取失败',
  "data": ""
}
```

### 18.8获取组卷学科考点

jyeoo_exercise_set/get_points

请求方式：post

作者：meeter

请求参数:

参考18.5

返回：正确

```
{
  "stat": 0,
  "msg": "成功",
  "data": [
    {
      "No": "1",
      "Name": "基础知识",
      "Desc": "",
      "Children": [
        {
          "No": "1",
          "Name": "词汇",
          "Desc": "",
          "Children": [
            {
              "No": "11",
              "Name": "字音",
              "Desc": "高考易错字音：<br />A：<br />挨紧 āi&nbsp;&nbsp; 挨饿受冻 ái&nbsp;&nbsp; 白皑皑 ái&nbsp;&nbsp; 狭隘 ài&nbsp;&nbsp; 方兴未艾 ài（自怨自艾 yì）&nbsp;&nbsp; 不谙水性 ān&nbsp;&nbsp;&nbsp;熬菜 āo&nbsp;&nbsp; <br />煎熬 áo&nbsp;&nbsp; 鏖战 áo&nbsp;&nbsp;&nbsp;拗断 ǎo&nbsp;&nbsp; 拗口 ào 违拗 ào．<br />B<br />纵横捭阖 bǎi hé&nbsp;&nbsp;&nbsp;稗官野史 bài&nbsp;&nbsp;&nbsp;扳平 bān&nbsp;&nbsp;&nbsp;同胞 bāo&nbsp;&nbsp;&nbsp;炮羊肉 bāo&nbsp;&nbsp;&nbsp;剥皮 bāo&nbsp;&nbsp;&nbsp;薄纸 báo&nbsp;&nbsp; <br />并行不悖 bèi&nbsp;&nbsp; 蓓蕾 bèi lěi&nbsp;&nbsp;&nbsp;奔丧 bēn&nbsp;&nbsp;&nbsp;投奔 bèn&nbsp;&nbsp;&nbsp;迸发 bèng&nbsp;&nbsp;&nbsp;包庇 bì&nbsp;&nbsp;&nbsp;麻痹 bì&nbsp;&nbsp; 奴颜婢膝 bì xī &nbsp;&nbsp;<br />刚愎自用 bì&nbsp;&nbsp;&nbsp;复辟 bì&nbsp;&nbsp;&nbsp;筚路蓝缕 bì&nbsp;&nbsp; 濒临 bīn&nbsp;&nbsp;&nbsp;针砭 biān&nbsp;&nbsp;&nbsp;屏营 bīng&nbsp; 屏气 bǐng&nbsp; 屏弃bǐng&nbsp; <br />摒弃 bìng&nbsp; 剥削 bō xuē&nbsp;&nbsp;&nbsp;停泊 bó&nbsp;&nbsp;&nbsp;淡薄 bó&nbsp;&nbsp;&nbsp;哺育 bǔ<br />C<br />粗糙 cāo&nbsp;&nbsp;&nbsp;嘈杂 cáo&nbsp;&nbsp;&nbsp;参差 cēn cī&nbsp;&nbsp;&nbsp;差错 chā&nbsp;&nbsp;&nbsp;偏差 chā&nbsp;&nbsp;&nbsp;差距 chā&nbsp;&nbsp;&nbsp;搽粉 chá&nbsp;&nbsp;&nbsp;猹 chá&nbsp;&nbsp; <br />刹那 chà&nbsp;&nbsp;&nbsp;侘傺chà chì&nbsp;&nbsp; 姹紫嫣红chà&nbsp;&nbsp; 差遣 chāi&nbsp;&nbsp; 诌媚 chǎn&nbsp;&nbsp;&nbsp;忏悔 chàn&nbsp;&nbsp;&nbsp;羼水 chàn&nbsp;&nbsp;&nbsp;场院 cháng &nbsp;&nbsp;<br />一场雨 cháng&nbsp;&nbsp;&nbsp; 一场大战 cháng&nbsp;&nbsp; 一场球赛 chǎng&nbsp;&nbsp; 赔偿 cháng&nbsp;&nbsp;&nbsp;徜徉 cháng&nbsp;&nbsp;&nbsp;绰起 chāo&nbsp;&nbsp;&nbsp;风驰电掣 chè &nbsp;&nbsp;<br />称心chèn&nbsp;&nbsp; 瞠目结舌 chēng&nbsp;&nbsp;乘机 chéng&nbsp;&nbsp; 惩前毖后 chéng&nbsp;&nbsp;&nbsp;惩创 chéng chuàng&nbsp;&nbsp;&nbsp;驰骋 chěng&nbsp;&nbsp;&nbsp;鞭笞 chī&nbsp; &nbsp;<br />踟蹰（踟躇） chí chú&nbsp; 踯躅 zhí zhú&nbsp;&nbsp;奢侈 chǐ&nbsp;&nbsp;&nbsp;豆豉 chǐ&nbsp;&nbsp; 整饬 chì&nbsp;&nbsp;&nbsp;炽热 chì&nbsp;&nbsp;&nbsp;不啻 chì &nbsp;&nbsp;<br />叱咤风云 chì zhà&nbsp;&nbsp; 忧心忡忡 chōng&nbsp;&nbsp;&nbsp;踌躇 chóu chú&nbsp; 铁杵 chǔ&nbsp;&nbsp; 处分 chǔ&nbsp;&nbsp; 处所 chù&nbsp;&nbsp; 相形见绌 chù &nbsp;&nbsp;&nbsp;<br />黜免 chù&nbsp;&nbsp;&nbsp;揣摩 chuǎi&nbsp;&nbsp;&nbsp;挣揣 chuài&nbsp;&nbsp; 椽子 chuán&nbsp;&nbsp;&nbsp;创伤 chuāng&nbsp;&nbsp;&nbsp;创建 chuàng&nbsp;&nbsp; 凄怆 chuàng&nbsp;&nbsp; <br />啜泣 chuò&nbsp;&nbsp;&nbsp;辍学 chuò&nbsp;&nbsp;&nbsp;宽绰 chuò&nbsp;&nbsp;&nbsp;瑕疵 cī&nbsp;&nbsp;&nbsp;伺候 cì&nbsp;&nbsp;&nbsp;烟囱 cōng&nbsp;&nbsp;&nbsp;淙淙流水 cóng&nbsp;&nbsp;&nbsp;一蹴而就 cù &nbsp;&nbsp;<br />璀璨 cuǐ&nbsp;&nbsp;&nbsp;忖度 cǔn duó&nbsp;&nbsp;&nbsp;蹉跎 cuō tuó&nbsp;&nbsp;&nbsp;挫折cuò<br />D<br />答应 dā&nbsp;&nbsp;&nbsp;逮老鼠 dǎi&nbsp;&nbsp;&nbsp;逮捕dài&nbsp; 春风骀荡dài&nbsp;&nbsp; 殚思极虑dān&nbsp;&nbsp;&nbsp;虎视眈眈 dān&nbsp;&nbsp; 肆无忌惮 dàn&nbsp;&nbsp; <br />档案 dàng&nbsp;&nbsp;&nbsp;当（本）年 dàng&nbsp;&nbsp;&nbsp;当年（过去）dāng&nbsp;&nbsp; 追悼 dào&nbsp;&nbsp;&nbsp;提防 dī&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;瓜熟蒂落 dì&nbsp;&nbsp; 缔造 dì &nbsp;&nbsp;<br />掂量 diān&nbsp;&nbsp; 踮脚 diǎn&nbsp; 惦记 diàn&nbsp;&nbsp; 玷污 diàn&nbsp;&nbsp;&nbsp;装订 dìng&nbsp;&nbsp;&nbsp;&nbsp;订正 dìng&nbsp;&nbsp;&nbsp;恫吓 dòng hè&nbsp;&nbsp;&nbsp;句读 dòu &nbsp;&nbsp;<br />兑换 duì&nbsp;&nbsp;&nbsp;踱步 duó<br />E<br />阿谀 ē yú&nbsp; 婀娜 ē nuó&nbsp;&nbsp;&nbsp;扼要 è<br />F<br />菲薄 fěi&nbsp;&nbsp;&nbsp;氛围 fēn&nbsp;&nbsp;&nbsp;肤浅 fū&nbsp;&nbsp;&nbsp;敷衍塞责 fū yǎn sè&nbsp;&nbsp;&nbsp;仿佛 fú&nbsp;&nbsp;&nbsp;凫水 fú&nbsp;&nbsp;&nbsp;篇幅 fú&nbsp;&nbsp;&nbsp;辐射 fú&nbsp;&nbsp; <br />果脯 fǔ&nbsp;&nbsp;&nbsp;胸脯 pú&nbsp;&nbsp; 随声附和 fù hè<br />G<br />准噶尔 gá&nbsp;&nbsp;&nbsp;大动干戈 gē&nbsp;&nbsp;&nbsp;力能扛鼎gāng&nbsp;&nbsp; 诸葛亮 gé&nbsp;&nbsp;&nbsp;脖颈 gěng&nbsp;&nbsp;&nbsp;提供 gōng&nbsp;&nbsp;&nbsp;供销 gōng&nbsp;&nbsp;&nbsp;供给 gōng jǐ&nbsp;&nbsp; <br />供不应求 gōng yìng&nbsp;&nbsp;&nbsp;供认 gòng&nbsp;&nbsp;&nbsp;口供 gòng&nbsp;&nbsp;&nbsp;佝偻 gōu lóu&nbsp;&nbsp;&nbsp;勾当 gòu&nbsp;&nbsp;&nbsp;骨朵 gū&nbsp;&nbsp;&nbsp;骨气 gǔ&nbsp;&nbsp;&nbsp;蛊惑 gǔ &nbsp;&nbsp;<br />商贾 gǔ&nbsp;&nbsp;&nbsp;桎梏 gù&nbsp;&nbsp;&nbsp;皇冠 guān&nbsp;&nbsp; 冠心病 guān&nbsp;&nbsp; 粗犷 guǎng&nbsp;&nbsp;&nbsp;皈依 guī&nbsp;&nbsp;&nbsp;瑰丽 guī&nbsp;&nbsp;&nbsp;刽子手 guì&nbsp;&nbsp; 聒噪 guō<br />H<br />哈达 hǎ&nbsp;&nbsp;&nbsp;尸骸 hái&nbsp;&nbsp;&nbsp;希罕 hǎn&nbsp;&nbsp;&nbsp;引吭高歌 háng&nbsp;&nbsp;&nbsp;沆瀣一气 hàng xiè&nbsp;&nbsp;&nbsp;巷道hàng（巷子xiàng）&nbsp; 干涸 hé &nbsp;&nbsp;<br />一丘之貉 hé&nbsp;&nbsp; 上颌 hé&nbsp;&nbsp;&nbsp;喝采 hè&nbsp;&nbsp;&nbsp;负荷 hè&nbsp;&nbsp;&nbsp;蛮横 hèng&nbsp;&nbsp;&nbsp;飞来横祸 hèng&nbsp;&nbsp;&nbsp;发横财 hèng&nbsp;&nbsp;哄抢 hōng&nbsp;&nbsp;&nbsp; <br />哄人 hǒng&nbsp;&nbsp; 一哄而散 hòng&nbsp;&nbsp;&nbsp;糊口 hú&nbsp;&nbsp;&nbsp;囫囵吞枣 hú lún&nbsp;&nbsp;&nbsp;华山 huà&nbsp;&nbsp;&nbsp;怙恶不悛hù quān&nbsp;&nbsp;&nbsp;豢养 huàn &nbsp;&nbsp;<br />病入膏肓 huāng&nbsp;&nbsp; 讳疾忌医 huì jí&nbsp;&nbsp;&nbsp;诲人不倦 huì&nbsp;&nbsp;&nbsp;阴晦 huì&nbsp;&nbsp;&nbsp;污秽 huì&nbsp;&nbsp;&nbsp;不容置喙 huì&nbsp;&nbsp; 混水摸鱼 hún &nbsp;&nbsp;<br />混淆 hùn xiáo&nbsp;&nbsp; 和泥 huó&nbsp;&nbsp;&nbsp;搅和 huò&nbsp;&nbsp;&nbsp;豁达 huò&nbsp;&nbsp;&nbsp;霍乱 huò<br />J<br />茶几 jī&nbsp;&nbsp;&nbsp;畸形 jī&nbsp;&nbsp;&nbsp;羁绊 jī&nbsp;&nbsp;&nbsp;羁旅 jī&nbsp;&nbsp;&nbsp;放荡不羁 jī&nbsp;&nbsp;&nbsp;无稽之谈 jī&nbsp;&nbsp;&nbsp;跻身 jī&nbsp;&nbsp;&nbsp;通缉令 jī&nbsp;&nbsp;&nbsp;汲取 jí&nbsp; <br />即使 jí&nbsp;&nbsp;&nbsp;开学在即 jí&nbsp;&nbsp;&nbsp;疾恶如仇 jí&nbsp;&nbsp;&nbsp;嫉妒 jí&nbsp;&nbsp;&nbsp;棘手 jí&nbsp;&nbsp;&nbsp;贫瘠 jí&nbsp; 狼藉 jí&nbsp;&nbsp;&nbsp;一触即发 jí&nbsp;&nbsp;&nbsp;<br />佶jí屈聱áo牙&nbsp;&nbsp;&nbsp; 脊梁 jǐ&nbsp;&nbsp;&nbsp;人才济济 jǐ&nbsp;&nbsp;&nbsp;给予 jǐ yǔ&nbsp;&nbsp;&nbsp;凯觎 jì yú&nbsp;&nbsp;&nbsp;成绩 jì&nbsp;&nbsp;&nbsp;雪茄 jiā&nbsp;&nbsp;&nbsp;信笺 jiān&nbsp; <br />歼灭 jiān&nbsp;&nbsp;&nbsp;草菅人命 jiān&nbsp;&nbsp;&nbsp;缄默 jiān 渐染 jiān&nbsp;&nbsp; 眼睑 jiǎn&nbsp;&nbsp;&nbsp;间断 jiàn&nbsp;&nbsp;&nbsp;监生 jiàn&nbsp;&nbsp; 细嚼慢咽 jiáo<br />咬文嚼字 jiáo&nbsp; 味同嚼蜡 jiáo&nbsp; 贪多嚼不烂jiáo&nbsp;&nbsp; 咀嚼jué&nbsp; 过屠门而大嚼jué&nbsp; 矫枉过正 jiǎo&nbsp; 缴纳 jiǎo &nbsp;&nbsp;<br />校对 jiào&nbsp;&nbsp;&nbsp;开花结果 jiē&nbsp;&nbsp;&nbsp;事情结果 jié&nbsp;&nbsp;&nbsp;结冰 jié&nbsp;&nbsp;&nbsp;反诘 jié&nbsp;&nbsp;&nbsp;拮据 jié jū&nbsp; 攻讦 jié&nbsp;&nbsp;&nbsp;桔梗 jié &nbsp;&nbsp;<br />押解 jiè&nbsp; 浑身解数xiè&nbsp;&nbsp;&nbsp;情不自禁 jīn&nbsp;&nbsp;&nbsp;根茎叶 jīng&nbsp;&nbsp;&nbsp;菁菁校园 jīng&nbsp;&nbsp; 粳米 jīng&nbsp;&nbsp; 长颈鹿 jǐng &nbsp;&nbsp;<br />杀一儆百 jǐng&nbsp;&nbsp; 强劲 jìng&nbsp;&nbsp;&nbsp;劲敌 jìng&nbsp;&nbsp;&nbsp;劲旅 jìng&nbsp;&nbsp;&nbsp;痉挛 jìng&nbsp;&nbsp;&nbsp;抓阄 jiū&nbsp;&nbsp;&nbsp;针灸 jiǔ&nbsp;&nbsp;&nbsp;韭菜 jiǔ &nbsp;&nbsp;&nbsp;<br />内疚 jiù&nbsp;&nbsp; 既往不咎 jiù&nbsp;&nbsp;&nbsp;不落窠 kē臼jiù&nbsp;&nbsp; 狙击jū&nbsp;&nbsp;&nbsp;咀嚼 jǔ jué&nbsp;&nbsp;&nbsp;循规蹈矩 jǔ&nbsp;&nbsp;&nbsp;矩形 jǔ&nbsp;&nbsp;&nbsp;龃龉 jǔ yǔ <br />踽踽独行jǔ&nbsp; 前倨后恭 jù&nbsp;&nbsp;&nbsp;镌刻 juān&nbsp;&nbsp;&nbsp;隽永 juàn&nbsp; 隽秀 jùn&nbsp;&nbsp;&nbsp;角色 jué&nbsp;&nbsp;&nbsp;口角 jué&nbsp;&nbsp;&nbsp;角斗 jué&nbsp;&nbsp;&nbsp;角逐 jué&nbsp; <br />倔强 jué jiàng&nbsp;&nbsp;&nbsp;猖獗 jué&nbsp;&nbsp;&nbsp;诡谲 jué&nbsp;&nbsp;&nbsp;矍铄 jué&nbsp;&nbsp;&nbsp;攫取 jué&nbsp; 细菌 jūn&nbsp; 蕈菌 xùn jùn&nbsp;&nbsp;&nbsp;龟袭 jūn．<br />K<br />同仇敌忾 kài&nbsp;&nbsp;&nbsp;门槛 kǎn槛车 jiàn&nbsp; 兽槛 jiàn&nbsp; 直栏横槛jiàn&nbsp; 坎坷 kě&nbsp;&nbsp;&nbsp;可汗 kè hán&nbsp;&nbsp;&nbsp;恪守 kè&nbsp;&nbsp;&nbsp;<br />倥偬 kǒng zǒng&nbsp;&nbsp;&nbsp;会计 kuài&nbsp;&nbsp; 窥探 kuī&nbsp;&nbsp;&nbsp;傀儡 kuǐ&nbsp;&nbsp; 岿然不动kuī&nbsp;&nbsp; 振聋发聩kuì<br />L<br />邋暹 lā ta&nbsp;&nbsp;&nbsp;拉家常 lā&nbsp;&nbsp;&nbsp;丢三落四 là&nbsp;&nbsp;&nbsp;书声琅琅 láng&nbsp;&nbsp;&nbsp;唠叨 láo dāo&nbsp; 落枕 lào&nbsp;&nbsp;&nbsp;勒索 lè&nbsp; 勒紧 lēi &nbsp;&nbsp;<br />擂鼓 léi&nbsp;&nbsp;&nbsp;赢弱 léi&nbsp;&nbsp;&nbsp;果实累累 léi&nbsp;&nbsp;&nbsp;罪行累累 lěi&nbsp;&nbsp;&nbsp;连累lěi&nbsp; 擂台 lèi&nbsp;&nbsp;&nbsp;罹难 lí&nbsp;&nbsp;&nbsp;潋滟 liàn&nbsp; <br />打量 liáng&nbsp;&nbsp;&nbsp;量入为出 liàng&nbsp;&nbsp;&nbsp;撩水 liāo&nbsp;&nbsp;&nbsp;撩拨 liáo&nbsp;&nbsp;&nbsp;寂寥 liáo&nbsp;&nbsp;&nbsp;瞭望 liào&nbsp;&nbsp;&nbsp;趔趄 liè qie&nbsp; <br />恶劣 liè&nbsp;&nbsp;&nbsp;雕镂 lòu&nbsp;&nbsp;&nbsp;贿赂 lù&nbsp;&nbsp;&nbsp;棕榈 lǘ&nbsp; 掠夺 lüè<br />M<br />抹桌子 mā&nbsp;&nbsp;&nbsp;阴霾 mái&nbsp;&nbsp;&nbsp;埋头苦干 mái&nbsp;&nbsp; 埋怨 mán&nbsp;&nbsp;&nbsp;欺谩 mán&nbsp;&nbsp; 轻谩 màn&nbsp; 蔓延màn&nbsp; 不蔓不枝 màn&nbsp;&nbsp; 藤蔓wàn<br />瓜蔓wàn&nbsp; 顺蔓摸瓜wàn&nbsp; 耄耋 mào dié&nbsp;&nbsp;&nbsp;联袂 mèi&nbsp;&nbsp;&nbsp;闷热 mēn&nbsp; 扪心自问 mén&nbsp; 愤懑 mèn&nbsp;&nbsp;&nbsp;蒙头转向 mēng &nbsp;&nbsp;<br />蒙头盖脸 méng&nbsp; 羁縻 mí&nbsp; 靡/糜费 mí&nbsp;&nbsp;&nbsp;萎靡不振 mǐ&nbsp;&nbsp;&nbsp;静谧 mì&nbsp; 分娩 miǎn&nbsp;&nbsp;&nbsp;酩酊 mǐng dǐng&nbsp;&nbsp;&nbsp;荒谬 mìù &nbsp;&nbsp;<br />纰缪 pī miù&nbsp; 绸缪 móu&nbsp;&nbsp;&nbsp;脉脉 mò&nbsp;&nbsp;&nbsp;抹墙 mò&nbsp;&nbsp;&nbsp;蓦然回首 mò&nbsp; 磨坊mò fáng&nbsp; 磨面mò&nbsp;&nbsp; 牟取 móu&nbsp;&nbsp;&nbsp;模样 mú<br />N<br />羞赧 nǎn&nbsp;&nbsp; 呶呶不休 náo&nbsp;&nbsp;&nbsp;泥淖 nào&nbsp;&nbsp;&nbsp;口讷 nè&nbsp;&nbsp;&nbsp;气馁 něi&nbsp; 端倪ní&nbsp; 忸怩ní&nbsp; 拟人 nǐ&nbsp;&nbsp;&nbsp;隐匿 nì&nbsp;&nbsp;&nbsp;拘泥 nì &nbsp;&nbsp;<br />亲昵 nì&nbsp;&nbsp;&nbsp;拈花惹草 niān&nbsp;&nbsp; 宁静níng&nbsp; 宁馨儿níng&nbsp; 宁可nìng&nbsp; 宁肯nìng&nbsp; 宁缺毋滥nìng&nbsp; 宁死不屈 nìng &nbsp;<br />宁愿nìng&nbsp; 泥泞 nìng&nbsp;&nbsp; 奸佞nìng&nbsp; 佞臣nìng&nbsp; 佞笑nìng&nbsp; 忸怩 niǔ ní&nbsp;&nbsp;&nbsp;执拗 niù&nbsp; 拗不过niù&nbsp; 驽马 nú&nbsp;&nbsp;&nbsp;<br />虐待 nüè<br />O<br />讴歌ōu&nbsp;&nbsp;&nbsp;沤肥òu．<br />P<br />扒手 pá&nbsp;&nbsp;&nbsp;迫击炮 pǎi&nbsp;&nbsp;&nbsp;心宽体胖 pán&nbsp;&nbsp;&nbsp;蹒跚 pán&nbsp;&nbsp;&nbsp;滂沱 pāng tuó&nbsp;&nbsp;&nbsp;彷徨 páng&nbsp;&nbsp;&nbsp;炮制 páo&nbsp;&nbsp; <br />咆哮 páo xiào&nbsp;&nbsp;炮烙 páo luò&nbsp;&nbsp;&nbsp;胚胎 pēi&nbsp;&nbsp;&nbsp;香喷喷 pēn&nbsp;&nbsp;&nbsp;抨击 pēng&nbsp;&nbsp;&nbsp;澎湃 péng pài&nbsp;&nbsp;&nbsp;纰漏 pī&nbsp;&nbsp;&nbsp;劈面 pī&nbsp;&nbsp; <br />劈叉 pǐ&nbsp;&nbsp; 毗邻 pí&nbsp; 裨将pí&nbsp; 偏裨pí&nbsp; 癖好 pǐ&nbsp;&nbsp;&nbsp;否极泰来 pǐ&nbsp;&nbsp;&nbsp;媲美 pì&nbsp; 睥睨pì nì&nbsp; 扁舟 piān&nbsp;&nbsp;&nbsp;<br />大腹便便 pián&nbsp;&nbsp;&nbsp;剽窃 piāo&nbsp;&nbsp; 饿殍 piǎo&nbsp;&nbsp;&nbsp;&nbsp;乒乓 pīng pāng&nbsp;&nbsp;&nbsp;暴虎冯河 píng&nbsp;&nbsp; 湖泊 pō&nbsp;&nbsp;&nbsp;居心叵测 pǒ &nbsp;&nbsp;<br />糟粕 pò&nbsp;&nbsp;&nbsp;解剖 pōu&nbsp;&nbsp;&nbsp;前仆后继 pū&nbsp;&nbsp;&nbsp;奴仆 pú&nbsp;&nbsp;&nbsp;风尘仆仆 pú&nbsp;&nbsp;&nbsp;玉璞 pú&nbsp;&nbsp;&nbsp;匍匐 pú fú&nbsp;&nbsp;&nbsp;瀑布 pù&nbsp;&nbsp;&nbsp;<br />一曝/暴十寒．<br />Q<br />休戚与共 qī&nbsp;&nbsp;&nbsp;蹊跷 qī qiāo（另辟蹊xī径）&nbsp;&nbsp;&nbsp;祈祷 qí&nbsp;&nbsp;&nbsp;颀长 qí&nbsp;&nbsp;&nbsp;岐途 qí&nbsp;&nbsp;&nbsp;绮丽 qǐ&nbsp;&nbsp;&nbsp;修葺 qì&nbsp;&nbsp;&nbsp;休憩 qì &nbsp;&nbsp;<br />关卡 qiǎ&nbsp; 悭吝 qiān&nbsp;&nbsp;&nbsp;掮客 qián&nbsp;&nbsp;&nbsp;潜移默化qián&nbsp;&nbsp;&nbsp;虔诚 qián&nbsp;&nbsp;&nbsp;缱绻 qiǎn quǎn&nbsp; 天堑 qiàn&nbsp;&nbsp;&nbsp;戕害 qiāng &nbsp;&nbsp;<br />呼天抢地 qiāng&nbsp;&nbsp; 强迫 qiǎng&nbsp;&nbsp;&nbsp;勉强 qiǎng&nbsp;&nbsp;&nbsp;强求 qiǎng&nbsp;&nbsp;&nbsp;牵强附会qiǎng&nbsp;&nbsp;&nbsp;襁褓 qiǎng&nbsp;&nbsp;&nbsp;翘首远望 qiáo &nbsp;&nbsp;<br />讥诮 qiào&nbsp; 怯懦 qiè&nbsp;&nbsp;&nbsp;提纲挈领 qiè&nbsp;&nbsp;&nbsp;锲而不舍 qiè&nbsp;&nbsp;&nbsp;惬意 qiè&nbsp;&nbsp;&nbsp;衾枕 qīn&nbsp;&nbsp;&nbsp;引擎 qíng&nbsp;&nbsp;&nbsp;亲家 qìng &nbsp;&nbsp;<br />曲折 qū&nbsp;&nbsp;&nbsp;祛除 qū&nbsp;&nbsp;&nbsp;黢黑 qū&nbsp;&nbsp;&nbsp;清癯 qú&nbsp;&nbsp;&nbsp;瞿塘峡 qú&nbsp;&nbsp;&nbsp;通衢大道 qú&nbsp;&nbsp;&nbsp;龋齿 qǔ&nbsp;&nbsp; 面面相觑 qù&nbsp;&nbsp;&nbsp;<br />怙hù恶不悛 quān&nbsp;&nbsp; 债券 quàn&nbsp;&nbsp;&nbsp;商榷 què&nbsp;&nbsp;&nbsp;逡巡 qūn&nbsp;&nbsp;&nbsp;麇集 qún．<br />R<br />荏苒 rěn rǎn&nbsp;&nbsp;&nbsp;稔知 rěn&nbsp;&nbsp;&nbsp;妊娠 rèn shēn&nbsp;&nbsp;&nbsp;仍然 réng&nbsp;&nbsp;&nbsp;冗长 rǒng&nbsp;&nbsp; 方枘圆凿 ruì&nbsp;&nbsp; 繁文缛节 rù<br />S<br />缫丝 sāo&nbsp;&nbsp;&nbsp;稼穑 jià sè&nbsp;&nbsp;&nbsp;堵塞 sè&nbsp;&nbsp;&nbsp;刹车 shā&nbsp; 煞/杀风景shā&nbsp; 煞笔 shā&nbsp;&nbsp; 凶神恶煞 shà&nbsp;&nbsp; 歃血为盟 shà&nbsp;&nbsp; <br />芟除 shān&nbsp;&nbsp;&nbsp;潸然泪下 shān&nbsp;&nbsp;禅让 shàn&nbsp;&nbsp;&nbsp;讪笑 shàn&nbsp;&nbsp;&nbsp;赡养 shàn&nbsp;&nbsp;&nbsp;折本 shé&nbsp;&nbsp;&nbsp;慑服 shè&nbsp;&nbsp;&nbsp;退避三舍 shè &nbsp;&nbsp;<br />莘莘学子shēn shēn&nbsp;&nbsp; 海市蜃楼 shèn&nbsp;&nbsp;&nbsp;狼奔豕突 shǐ&nbsp; 舐犊之情 shì&nbsp;&nbsp; 教室 shì&nbsp;&nbsp; 有恃无恐 shì&nbsp;&nbsp;&nbsp;狩猎 shòu &nbsp;&nbsp;<br />倏忽 shū&nbsp;&nbsp;&nbsp;束缚 shù fù&nbsp;&nbsp;&nbsp;刷白 shuà&nbsp;&nbsp;&nbsp;游说 shuì&nbsp;&nbsp; 数见不鲜 shuò&nbsp; 吸吮 shǔn&nbsp;&nbsp;&nbsp;&nbsp;瞬息万变 shùn &nbsp;&nbsp;<br />窥伺 sì（伺cì候）&nbsp;&nbsp; 塞责 sè&nbsp;&nbsp; 怂恿 sǒng yǒng&nbsp;&nbsp;&nbsp;塑料 sù&nbsp;&nbsp;&nbsp;簌簌 sù&nbsp;&nbsp;&nbsp;半身不遂suí&nbsp;&nbsp; 未遂suì&nbsp; <br />鬼鬼祟祟suì&nbsp;&nbsp;&nbsp;婆娑 suō<br />T<br />趿拉 tā&nbsp;&nbsp;&nbsp;鞭挞 tà&nbsp;&nbsp;&nbsp;纷至沓来tà&nbsp;&nbsp; 拓本tà（落拓tuò不羁）&nbsp;&nbsp; 叨扰tāo&nbsp; 叨陪鲤对tāo&nbsp; 丝绦tāo&nbsp; 熏陶 táo <br />挑剔 tī&nbsp; 体已 tī&nbsp;&nbsp;&nbsp;孝悌 tì&nbsp;&nbsp;&nbsp;倜傥 tì tǎng&nbsp;&nbsp;恬不知耻 tián&nbsp;&nbsp;&nbsp;暴殄天物 tiǎn&nbsp;&nbsp;&nbsp;轻佻 tiāo&nbsp;&nbsp;&nbsp;妥贴（帖） tiē&nbsp; <br />请帖 tiě&nbsp;&nbsp;&nbsp;字帖 tiè&nbsp;&nbsp;&nbsp;恸哭 tòng&nbsp;&nbsp;&nbsp;如火如荼 tú&nbsp;&nbsp;&nbsp;湍急 tuān&nbsp;&nbsp;&nbsp;蜕化 tuì 朝暾 tūn&nbsp; 囤积 tún&nbsp;&nbsp; （粮囤 dùn）<br />臀部tún．<br />W<br />崴脚 wǎi&nbsp;&nbsp; 瓜蔓 wàn&nbsp; 藤蔓wàn&nbsp; 顺蔓摸瓜wàn&nbsp;&nbsp; 逶迤 wēi yí&nbsp;&nbsp;&nbsp;违反 wéi&nbsp;&nbsp;&nbsp;崔嵬 wéi&nbsp; 圩田 wéi&nbsp; <br />冒天下之大不韪wěi&nbsp;&nbsp;推诿wěi&nbsp; 猥亵wěi&nbsp; 为虎作伥 wèi chāng&nbsp;&nbsp; 紊乱 wěn&nbsp; 倭寇 wō&nbsp;&nbsp;&nbsp; 龌龊 wò chuò&nbsp;&nbsp; <br />斡旋 wò&nbsp;&nbsp;&nbsp;深恶痛疾 wù<br />X<br />膝盖 xī&nbsp;&nbsp;&nbsp;檄文 xí&nbsp;&nbsp;&nbsp;狡黠 xiá&nbsp;&nbsp;&nbsp;厦门 xià&nbsp;&nbsp;&nbsp;纤维 xiān wéi&nbsp;&nbsp;&nbsp;翩跹 xiān&nbsp;&nbsp;&nbsp;纤纤玉手xiān&nbsp;&nbsp; 屡见不鲜 xiān &nbsp;&nbsp;<br />垂涎三尺 xián&nbsp;&nbsp;&nbsp;勾股弦 xián&nbsp;&nbsp;&nbsp;鲜见 xiǎn&nbsp;&nbsp;&nbsp;肖像 xiào&nbsp;&nbsp;&nbsp;生肖 xiào&nbsp; 采撷 xié&nbsp;&nbsp;&nbsp;叶韵xié&nbsp;&nbsp;&nbsp;纸屑 xiè &nbsp;&nbsp;<br />机械 xiè&nbsp;&nbsp; 省亲 xǐng&nbsp;&nbsp;&nbsp;不朽 xiǔ&nbsp;&nbsp;&nbsp;铜臭 xiù&nbsp; 乳臭未干xiù&nbsp; 无色无臭xiù&nbsp; 星宿 xiù&nbsp;&nbsp; 长吁短叹 xū &nbsp;&nbsp;<br />自诩 xǔ&nbsp;&nbsp;&nbsp;抚恤金 xù&nbsp;&nbsp;&nbsp;酗酒 xù&nbsp;&nbsp;&nbsp;煦暖 xù&nbsp; 煊赫 xuān&nbsp; 烜赫 xuǎn&nbsp;&nbsp;眩晕 xuàn yùn&nbsp;&nbsp;&nbsp;炫耀 xuàn&nbsp; 拱券xuàn&nbsp; <br />洞穴 xué&nbsp;&nbsp; 噱头xué&nbsp; 可发一噱jué&nbsp;&nbsp; 戏谑 xuè&nbsp;&nbsp;呕心沥血xuè&nbsp; 血xiě淋淋&nbsp;&nbsp; 驯服 xùn&nbsp; 蕈菌 xùn jùn&nbsp; <br />徇私舞弊xùn．<br />Y<br />睚眦必报yá zì&nbsp; 倾轧 yà&nbsp;&nbsp;&nbsp;揠苗助长 yà&nbsp;&nbsp;&nbsp;殷红 yān&nbsp;&nbsp;&nbsp;湮没 yān&nbsp;&nbsp;&nbsp;筵席 yán&nbsp;&nbsp;&nbsp;百花争妍 yán 河沿 yán&nbsp; <br />偃旗息鼓 yǎn&nbsp;&nbsp;&nbsp;奄奄一息 yǎn&nbsp;&nbsp;&nbsp;梦魇yǎn&nbsp;&nbsp; 赝品 yàn&nbsp;&nbsp;&nbsp;佯装 yáng&nbsp;&nbsp;&nbsp;怏怏不乐 yàng&nbsp;&nbsp;&nbsp;安然无恙 yàng&nbsp; <br />杳无音信 yǎo&nbsp;&nbsp;&nbsp;窈窈 yǎo tiǎo&nbsp;&nbsp;&nbsp;发虐子 yào&nbsp;&nbsp;&nbsp;耀武扬威 yào&nbsp;&nbsp;&nbsp;因噎废食 yē&nbsp;&nbsp; 揶揄 yé yú&nbsp;&nbsp; 陶冶 yě &nbsp;&nbsp;<br />呜咽 yè&nbsp;&nbsp;&nbsp;摇曳 yè&nbsp;&nbsp;&nbsp;拜谒 yè&nbsp;&nbsp;&nbsp;笑靥 yè&nbsp;&nbsp;&nbsp;开门揖盗 yī&nbsp;&nbsp; 虚与委蛇yí&nbsp;&nbsp; 甘之如饴 yí&nbsp;&nbsp;&nbsp;颐和园 yí&nbsp;&nbsp;&nbsp;<br />迤逦 yǐ lǐ&nbsp; 旖旎 yǐ nǐ&nbsp;&nbsp;&nbsp;倚仗yǐ&nbsp;&nbsp;&nbsp;游弋 yì&nbsp;&nbsp;&nbsp;后裔 yì&nbsp;&nbsp;&nbsp;奇闻轶事 yì&nbsp;&nbsp;&nbsp;络绎不绝 yì&nbsp;&nbsp;&nbsp;造诣 yì&nbsp;&nbsp; 友谊 yì &nbsp;&nbsp;<br />肄业 yì 熠熠闪光 yì&nbsp;&nbsp;&nbsp;自缢 yì&nbsp;&nbsp; 黄金百镒yì&nbsp; 自怨自艾yì&nbsp;&nbsp; 惩艾yì&nbsp; 一望无垠 yín&nbsp;&nbsp;&nbsp;荫凉 yìn&nbsp;&nbsp;&nbsp;应届 yīng &nbsp;&nbsp;<br />应承 yìng&nbsp;&nbsp; 黑黝黝 yǒu&nbsp;&nbsp;&nbsp;良莠不齐 yǒu&nbsp;&nbsp;&nbsp;迂回 yū&nbsp;&nbsp; 向隅而泣 yú&nbsp; 喁喁私语 yú&nbsp; 始终不渝 yú&nbsp;&nbsp;年逾古稀 yú &nbsp;&nbsp;<br />伛偻 yú lǚ&nbsp; 舆论 yú&nbsp;&nbsp;&nbsp;尔虞我诈 yú&nbsp;&nbsp;&nbsp;囹<B>圄</B> yǔ&nbsp;&nbsp;&nbsp;参与 yù&nbsp;&nbsp;&nbsp;驾驭 yù&nbsp;&nbsp;&nbsp;熨帖 yù&nbsp; 鹜蚌相争 yù&nbsp;&nbsp;&nbsp;卖儿鬻女 yù &nbsp;&nbsp;<br />断壁残垣 yuán&nbsp;&nbsp;&nbsp;苑囿 yuàn yòu&nbsp;&nbsp;&nbsp;掾吏 yuàn&nbsp; 锁钥 yuè&nbsp; 头晕 yūn&nbsp; 晕厥yūn jué 晕场yùn&nbsp;&nbsp;&nbsp;晕船 yùn &nbsp;&nbsp;<br />晕车 yùn&nbsp; 晕血 yùn&nbsp; 晕针 yùn&nbsp; 日晕 yùn&nbsp; 月晕 yùn．<br />Z<br />扎小辫 zā&nbsp;&nbsp;&nbsp;柳荫匝地 zā&nbsp;&nbsp;&nbsp;登载 zǎi 刊载 zǎi&nbsp; 转载 zǎi&nbsp; 载重 zài&nbsp;&nbsp;&nbsp;载歌载舞 zài&nbsp;&nbsp;&nbsp;怨声载道 zài&nbsp; <br />拒载 zài&nbsp; 下载资料zài&nbsp; 暂时 zàn&nbsp;&nbsp;&nbsp;臧否 zāng pǐ&nbsp;&nbsp;&nbsp;宝藏 zàng&nbsp;&nbsp;&nbsp;确凿 záo&nbsp;&nbsp;&nbsp;名噪一时zào&nbsp;&nbsp; 啧啧称赞 zé &nbsp;&nbsp;<br />咋舌 zé&nbsp;&nbsp; 谮言 zèn&nbsp;&nbsp;&nbsp;憎恶 zēng&nbsp;&nbsp;&nbsp;赠送 zèng&nbsp; 驻扎 zhā&nbsp;&nbsp;&nbsp;咋呼 zhā&nbsp;&nbsp;&nbsp;挣扎 zhá 札记 zhá 炸酱 zhá<br />炸油条zhá&nbsp;&nbsp; 炸弹zhà&nbsp;&nbsp;&nbsp;择菜 zhái&nbsp;&nbsp;&nbsp;占卜 zhān&nbsp; 客栈 zhàn&nbsp;&nbsp;&nbsp;破绽 zhàn&nbsp;&nbsp;&nbsp;精湛 zhàn&nbsp;&nbsp;&nbsp;颤/战栗 zhàn &nbsp;&nbsp;<br />高涨 zhǎng&nbsp;&nbsp;&nbsp;涨价 zhǎng&nbsp; 头昏脑涨zhàng&nbsp; 着数 zhāo&nbsp;&nbsp;着慌 zháo&nbsp; 着火zháo&nbsp; 着急zháo&nbsp; 着凉zháo<br />着忙zháo&nbsp; 着迷zháo&nbsp; 着魔zháo&nbsp; 着三不着两zháo&nbsp; 沼泽 zhǎo 召开 zhào&nbsp;&nbsp;&nbsp;肇事 zhào&nbsp;&nbsp;&nbsp;折腾 zhē&nbsp;&nbsp;&nbsp;<br />动<B>辄</B>得<B>咎</B> zhé jiù&nbsp;&nbsp;&nbsp;蛰伏 zhé&nbsp;&nbsp;&nbsp;贬谪 zhé 潭柘寺zhè&nbsp; 铁砧 zhēn&nbsp;&nbsp;&nbsp;日臻完善 zhēn&nbsp;&nbsp;&nbsp;甄别 zhēn&nbsp; 箴言 zhēn &nbsp;&nbsp;<br />缜密 zhěn&nbsp;&nbsp;&nbsp;赈灾 zhèn&nbsp; 症结 zhēng&nbsp;&nbsp;&nbsp;拯救 zhěng&nbsp;&nbsp;&nbsp;症候 zhèng&nbsp;&nbsp;&nbsp;诤友 zhèng&nbsp;&nbsp;&nbsp;挣脱 zhèng&nbsp;&nbsp;&nbsp;脂肪 zhī&nbsp;&nbsp; <br />踯躅 zhí zhú&nbsp; 近在咫尺 zhǐ&nbsp;&nbsp;&nbsp;博闻强识 zhì&nbsp;&nbsp;&nbsp;标识 zhì（标识shí）款识 zhì&nbsp; 质量 zhì&nbsp;&nbsp;&nbsp;脍炙人口 zhì&nbsp;&nbsp; <br />鳞次栉比 zhì&nbsp;&nbsp;&nbsp;对峙 zhì&nbsp;&nbsp;&nbsp;卷帙浩繁 zhì&nbsp;&nbsp; 栉风沐雨 zhì&nbsp; 中看 zhōng&nbsp;&nbsp;中听 zhōng&nbsp;&nbsp;&nbsp;中肯 zhòng&nbsp;&nbsp;&nbsp;<br />刀耕火种 zhòng&nbsp;&nbsp;&nbsp;胡诌 zhōu&nbsp; 啁啾 zhōu&nbsp;&nbsp;&nbsp;压轴 zhòu&nbsp; 大轴子zhòu&nbsp; 贮藏 zhù&nbsp; 莺啼鸟啭 zhuàn&nbsp;&nbsp;&nbsp;撰稿 zhuàn &nbsp;&nbsp;<br />谆谆 zhūn&nbsp; 弄巧成拙 zhuō&nbsp;&nbsp;&nbsp;灼热 zhuó&nbsp;&nbsp;&nbsp;卓越 zhuó&nbsp;&nbsp;&nbsp;啄木鸟 zhuó&nbsp;&nbsp;&nbsp;着陆 zhuó&nbsp;&nbsp; 着落zhuó&nbsp; 着墨zhuó<br />着色zhuó&nbsp; 着想zhuó&nbsp; 着手成春zhuó&nbsp; 着装zhuó&nbsp; 穿着打扮 zhuó&nbsp; 恣意 zì&nbsp;&nbsp;&nbsp;浸渍 zì&nbsp;&nbsp;&nbsp;作坊 zuō",
              "Children": []
            },
            {
              "No": "12",
              "Name": "字形",
              "Desc": "高考易错字形：<br />A<br /><B>唉</B>声叹气&nbsp; 白雪<B>皑皑&nbsp; </B>和<B>蔼&nbsp; </B><B>蔼</B>然可亲&nbsp; 暮<B>霭&nbsp; </B><B>安</B>装&nbsp; <B>按图</B>索骥&nbsp; 昏<B>暗</B>&nbsp;<B>&nbsp;&nbsp;</B><B>暗</B>淡&nbsp; <B>黯</B>淡&nbsp;&nbsp; <B>黯</B>然销魂&nbsp; <B>黯</B>然失色&nbsp; <br /><B>佶</B><B>/</B><B>诘屈聱</B>牙&nbsp;&nbsp;&nbsp;&nbsp;<B>桀骜</B>不驯&nbsp;&nbsp;&nbsp; <B>翱</B>翔&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<B>遨</B>游&nbsp;&nbsp;&nbsp;&nbsp; 道貌<B>岸</B>然．<br />B<br /><B>般</B><B>/</B><B>班配</B>&nbsp; 选<B>拔</B>赛&nbsp; 木<B>版</B>画&nbsp;&nbsp;<B>绊</B>脚石&nbsp; 磕磕<B>绊绊&nbsp; </B>搅<B>拌</B>机&nbsp; 自<B>暴</B>自弃&nbsp; <B>暴</B>发户&nbsp; 山洪<B>暴</B>发&nbsp; <B>爆</B>发力&nbsp; <B>爆</B>冷门&nbsp; 火山<B>爆</B>发&nbsp; <br /><B>爆</B>发革命&nbsp; <B>辨</B>正/<B>辩</B>正&nbsp; <B>辨</B>证/<B>辩</B>证&nbsp;&nbsp; 辨<B>证</B><B>/</B><B>症</B>论治&nbsp; <B>辩</B>解&nbsp; <B>辩</B>论&nbsp; <B>辩</B>驳&nbsp; 狡<B>辩</B>&nbsp; 声<B>辩&nbsp; </B><B>辩</B>证法&nbsp; 分<B>辩</B>（辩白）<br />分<B>辨</B>（辨别）&nbsp; <B>辨</B>认&nbsp; <B>辨</B>别&nbsp; 明<B>辨</B>是非&nbsp; 硬<B>邦邦</B>&nbsp; 双<B>胞</B>胎&nbsp; 永<B>葆</B>青春&nbsp;&nbsp; 飞扬<B>跋</B>扈&nbsp;&nbsp; 依山<B>傍</B>水&nbsp; <B>傍</B>人门户&nbsp; 自<B>暴</B>自弃&nbsp; <br /><B>彪</B>炳千古&nbsp; 落英<B>缤</B>纷&nbsp; 远大<B>抱负</B>&nbsp; 打击<B>报复</B>&nbsp; 以德<B>报</B>怨&nbsp; 甘<B>拜</B>下风&nbsp; <B>比比</B>皆是&nbsp; 纵横<B>捭</B>阖&nbsp; <B>稗</B>官野史 <B>俾</B>众周知<br /><B>俾</B>有所悟&nbsp; 大有<B>裨</B>益&nbsp; 奴颜<B>婢</B>膝&nbsp; 可见一<B>斑</B>&nbsp;&nbsp;<B>班</B>门弄斧&nbsp; <B>班</B>师回朝&nbsp; 安<B>邦</B>定国&nbsp; <B>坂</B><B>/</B><B>阪</B>上走丸&nbsp; 图穷<B>匕见</B>&nbsp; <B>背</B>水一战&nbsp; <br /><B>背</B>道而<B>驰</B>&nbsp; 并行不<B>悖</B>&nbsp; 中西合<B>璧</B><B>&nbsp;</B>&nbsp;金<B>碧</B>辉煌&nbsp; 作<B>壁</B>上观&nbsp; 刚<B>愎</B>自用&nbsp; <B>秉</B>公执法&nbsp; <B>毕</B>竟&nbsp; <B>必/毕</B>恭<B>必/毕</B>敬<br />原形<B>毕</B>露&nbsp; 锋芒<B>毕</B>露&nbsp; 针<B>砭</B>时弊&nbsp; 褒<B>贬</B>不一&nbsp; 关怀<B>备</B>至&nbsp; 推崇<B>备</B>至&nbsp; 德才兼<B>备</B>&nbsp; 艰苦<B>备</B>尝 <B>倍</B>道兼程&nbsp; 事半功倍&nbsp; <br />信心<B>倍</B>增&nbsp; 英雄<B>辈</B>出&nbsp; 作<B>弊</B>&nbsp; <B>弊</B>端&nbsp; 兴利除<B>弊</B>&nbsp; 营私舞<B>弊</B>&nbsp; 民生凋<B>敝</B>&nbsp; <B>敝</B>帚自珍&nbsp; 舌<B>敝</B>唇焦&nbsp; 遮天<B>蔽</B>日&nbsp; 一言以<B>蔽</B>之<br /><B>筚/荜</B>路蓝缕&nbsp; <B>蓬荜</B>生辉&nbsp; 作法自<B>毙</B>&nbsp; 惩前<B>毖</B>后&nbsp; 枪<B>毙&nbsp; </B>不修<B>边</B>幅&nbsp; 脉<B>搏</B>&nbsp; 拼<B>搏&nbsp; </B>起<B>搏</B>器&nbsp; 赤膊&nbsp; 胳<B>膊</B>&nbsp; 赌<B>博</B>&nbsp; <br /><B>博</B>弈 <B>舶</B>来品&nbsp; 对<B>簿</B>公堂&nbsp; <B>部署</B><B>&nbsp;</B>&nbsp;<B>布</B>置&nbsp; 按<B>部</B>就班&nbsp; 战略<B>部</B>署．<br />C<br /><B>仓皇</B><B>/</B><B>仓惶</B><B>/</B><B>苍黄</B><B>/</B><B>仓黄</B><B>&nbsp; </B><B>沧</B>桑 <B>苍</B>白 <B>沧</B>海&nbsp; 满目<B>疮</B>痍&nbsp;&nbsp;<B>苍</B>松翠柏&nbsp; <B>搀</B><B>/</B><B>掺</B>和&nbsp; <B>蹉</B>跎&nbsp;&nbsp; <B>磋</B>商&nbsp;&nbsp; <B>嗟</B>叹&nbsp; 找<B>茬</B>&nbsp;&nbsp; 拼<B>凑</B>&nbsp; <br /><B>奏</B>效&nbsp; 一<B>蹴</B>而就&nbsp; 为虎作<B>伥</B>&nbsp; <B>撤</B>销处分&nbsp; 响<B>彻</B>云霄&nbsp; 天崩地<B>坼</B>&nbsp; 拔得头<B>筹</B><B>&nbsp;</B>&nbsp;<B>陈</B>规陋习&nbsp; <B>策</B>源地&nbsp; 擦边球&nbsp; 白<B>炽</B>灯&nbsp; <br />返回<B>舱</B>&nbsp; <B>岔</B>路口&nbsp; 黄<B>澄澄</B>（dēng） 德<B>才</B>兼备&nbsp; <B>才</B>华横溢&nbsp;&nbsp; 人尽其<B>才</B><B>&nbsp;</B>&nbsp;<B>才</B>疏学浅&nbsp; <B>才</B>高八斗&nbsp; 就地取<B>材</B>&nbsp; 因<B>材</B>施教 <br />大<B>材</B>小用&nbsp; <B>人才</B><B>/</B><B>材&nbsp; </B>别出心<B>裁&nbsp; </B><B>惨</B>无人道&nbsp; <B>残</B>酷无情&nbsp; 自<B>惭</B>形秽&nbsp; 酒中<B>掺</B>水&nbsp; 风<B>采</B><B>&nbsp; </B>神<B>采</B><B>/</B><B>彩</B>&nbsp; 文<B>采</B>&nbsp; 无精打<B>采</B><B> </B><br />兴高<B>采</B>烈&nbsp;&nbsp; 光<B>彩</B><B>/</B><B>采</B>&nbsp;&nbsp; 精<B>彩</B><B>/</B><B>采</B><B>&nbsp;&nbsp; </B>喝<B>彩</B><B>/</B><B>采&nbsp; </B>喝倒<B>彩</B><B>/</B><B>采</B>&nbsp;&nbsp; 挂<B>彩</B>&nbsp;&nbsp; 博<B>彩</B>&nbsp;&nbsp; 剪<B>彩</B>&nbsp;&nbsp; 多姿多<B>彩</B>&nbsp;&nbsp; 五<B>彩</B><B>/</B><B>采</B>缤纷 <br />张灯结<B>彩&nbsp; </B><B>粲</B>然一笑&nbsp; <B>恻</B>隐之心&nbsp; 缠绵悱<B>恻</B>&nbsp; 如愿以<B>偿</B><B>&nbsp;</B>&nbsp;得不<B>偿</B>失&nbsp; 如期<B>偿</B>还&nbsp; <B>偿</B>还债务&nbsp; <B>尝</B>鼎一脔&nbsp; 艰苦备<B>尝</B>&nbsp; <br />扬<B>长</B>而去&nbsp; <B>长</B>年累月&nbsp; <B>长</B>治久安&nbsp; <B>长</B>此以往&nbsp; 万古<B>长</B>青&nbsp; 细水<B>长</B>流&nbsp; 别无<B>长</B>物 <B>长</B>风破浪 小人<B>长</B>戚戚&nbsp; <br /><B>长</B>使英雄泪满襟&nbsp;&nbsp; <B>常</B>备不懈&nbsp; 冬夏<B>常</B>青&nbsp; 习以为<B>常</B>&nbsp;&nbsp; <B>常</B>胜将军 老生<B>常</B>谈 万里悲秋<B>常</B>作客&nbsp; 良<B>辰</B>美景&nbsp; 寥若<B>晨</B>星<br /><B>瞠</B>目结舌&nbsp; 一<B>成</B>不变&nbsp;&nbsp; 相辅相<B>成</B>&nbsp;&nbsp; 一脉相<B>承&nbsp; </B>众志成<B>城&nbsp; </B>胸无<B>城</B>府&nbsp; 开<B>诚</B>布公&nbsp; <B>诚</B>惶<B>诚</B>恐&nbsp; 计日<B>程</B>功&nbsp; 张<B>皇</B>失<B>措</B><br />驰<B>骋&nbsp; </B><B>嗤</B>之以鼻&nbsp; 风<B>驰</B>电掣&nbsp; <B>驰</B>名中外&nbsp; <B>驰</B>目远眺&nbsp; <B>驰</B>誉全国&nbsp; 松<B>弛</B>&nbsp; 一张一<B>弛</B><B>&nbsp;</B>&nbsp;<B>弛</B>缓&nbsp; 不可<B>弛</B>懈&nbsp;&nbsp; 整<B>饬</B><B> </B>&nbsp;<br />故作矜<B>持</B><B>&nbsp;</B>&nbsp;<B>叱</B>咤风云&nbsp; <B>赤</B>地千里&nbsp;&nbsp; 同性相<B>斥</B>&nbsp; <B>斥</B>退左右&nbsp; <B>斥</B>资百万&nbsp; <B>充</B>耳不闻&nbsp; 首当其<B>冲</B><B>&nbsp;</B>&nbsp;忧心<B>忡忡</B>&nbsp; 人影<B>幢幢</B>&nbsp; <B><br />崇</B>山峻岭&nbsp; 鬼鬼<B>祟祟</B>&nbsp; 邮<B>戳</B>&nbsp; <B>戳</B>穿阴谋&nbsp;&nbsp;相形见<B>绌</B>&nbsp; <B>川</B>流不息&nbsp; 如<B>椽</B>大笔&nbsp; <B>创</B>伤&nbsp; 千<B>疮</B>百孔&nbsp; <B>椎</B>心泣血&nbsp; 觥<B>筹</B>交错<br />一<B>筹</B>莫展&nbsp; 驻守边<B>陲</B>&nbsp;&nbsp; <B>绰绰</B>有余&nbsp;&nbsp; 吹毛求<B>疵&nbsp; </B><B>催</B>人泪下&nbsp; <B>摧</B>枯拉朽&nbsp; 义不容<B>辞</B><B>&nbsp;</B>&nbsp;<B>辞</B><B>/</B><B>词藻</B><B>&nbsp; </B><B>辞</B><B>/</B><B>词令</B><B>&nbsp; </B>振振有<B>词</B><B>/</B><B>辞</B>&nbsp;&nbsp; <br />喜忧<B>参</B>半&nbsp; <B>除</B>恶务尽&nbsp; <B>锄</B>强扶弱&nbsp; <B>促</B>膝谈心&nbsp; 抱头鼠<B>窜&nbsp; </B><B>篡</B>位夺权&nbsp; <B>璀璨</B>夺目&nbsp; 鞠躬尽<B>瘁</B>&nbsp;&nbsp; 心力交<B>瘁</B>&nbsp;&nbsp; 出类拔<B>萃</B>&nbsp; <br />荟<B>萃</B>&nbsp; 精<B>粹</B>&nbsp; 纯<B>粹</B>&nbsp; 国<B>粹</B>&nbsp; 憔悴<br />D<br />老<B>搭档</B><B>&nbsp;</B>&nbsp;<B>耽</B>搁&nbsp;&nbsp;虎视<B>眈眈</B><B>&nbsp;</B>&nbsp;&nbsp;&nbsp;以逸<B>待</B>劳&nbsp; <B>玷</B>污&nbsp; 取<B>缔</B>&nbsp; <B>缔</B>结&nbsp; <B>缔</B>造&nbsp; 真<B>谛</B>&nbsp; <B>谛</B>听&nbsp; 瓜熟<B>蒂</B>落&nbsp; 携<B>带</B>&nbsp; 穿<B>戴</B><B>&nbsp;</B>&nbsp;<B>戴</B>高帽&nbsp; <br />不共<B>戴</B>天&nbsp;<B>&nbsp;</B>披星<B>戴（带）</B>月&nbsp; <B>戴</B>罪立功&nbsp; 责无旁<B>贷&nbsp; </B>严惩不<B>贷</B>&nbsp; <B>弹</B>丸之地&nbsp;&nbsp;&nbsp;<B>殚</B>精竭虑&nbsp; 肆无忌<B>惮&nbsp; </B>中流<B>砥</B>柱&nbsp; <B>砥砺</B><B>&nbsp; </B><br />眼高手<B>低</B>&nbsp; 刨根问<B>底</B>&nbsp;&nbsp; <B>诋</B>毁&nbsp; 民生凋<B>敝</B>&nbsp; 狗尾续<B>貂</B>&nbsp; <B>掉</B>书袋&nbsp; <B>吊</B>胃口&nbsp; <B>掉</B>以轻心&nbsp; 尾大不<B>掉</B><B>&nbsp;</B>&nbsp;提心<B>吊</B>胆&nbsp; 沽名<B>钓</B>誉<br />重<B>叠</B>&nbsp; 层峦<B>叠</B>嶂&nbsp; 层见<B>叠</B>出&nbsp; <B>叠</B>床架屋&nbsp; 更<B>迭</B> 花样<B>迭</B>出&nbsp; <B>迭</B>挫强敌&nbsp; 高潮<B>迭</B>起&nbsp; 后悔不<B>迭</B> <B>喋喋</B>不休&nbsp; <B>喋</B>血&nbsp; 通<B>牒</B>&nbsp;&nbsp;&nbsp; <br />间<B>谍</B>&nbsp; <B>谍</B>报&nbsp; 影<B>碟</B>&nbsp; <B>碟</B>子&nbsp; <B>顶</B>礼膜拜&nbsp; <B>鼎</B>力相助&nbsp; 三足<B>鼎</B>立&nbsp; 大名<B>鼎鼎</B><B>&nbsp; </B>革故<B>鼎</B>新&nbsp; 纱<B>锭</B>&nbsp; 签<B>订</B>合同&nbsp; 修<B>订</B>书刊&nbsp;&nbsp; <br />装<B>订</B>&nbsp; <B>订</B>书机&nbsp; 碰<B>钉</B>子&nbsp; <B>钉</B>扣子 亵<B>渎</B>&nbsp; <B>渎</B>职&nbsp; 牛犊&nbsp; 买<B>椟</B>还珠&nbsp; 穷兵<B>黩</B>武&nbsp; <B>赎</B>罪&nbsp; 尺<B>牍&nbsp; </B>连篇累<B>牍</B>&nbsp; <B>渡</B>过难关&nbsp; <br /><B>渡</B>江 <B>度</B>假村 暗<B>度</B>陈仓 <B>端</B>详&nbsp; <B>断章取义 </B><B>兑</B>现&nbsp; <B>堕</B>落腐化&nbsp; <B>坠</B>入深渊&nbsp; 点<B>缀</B>&nbsp; <B>缀</B>句成文&nbsp; 拾<B>掇</B><B>&nbsp;</B>&nbsp;<B>辍</B>学&nbsp; <B>咄咄</B>逼人 <br />揆情<B>度</B>理 安步<B>当</B>车&nbsp; <B>螳臂当</B><B>/</B><B>挡车</B>&nbsp; 独<B>当</B>一面&nbsp; 兵来将挡 <br />E<br />出<B>尔</B>反<B>尔&nbsp; </B>闻名遐<B>迩</B>&nbsp; 响<B>遏</B>行云&nbsp; 浑浑<B>噩噩&nbsp; </B><B>噩</B>梦&nbsp; <B>噩</B>耗&nbsp; <B>噩</B>运&nbsp; <B>噩</B>兆&nbsp; <B>厄</B>运&nbsp; 白垩<br />F&nbsp;<br /><B>妨</B>碍&nbsp; <B>防</B>范&nbsp; <B>防</B>备 <B>副</B>作用&nbsp; <B>富</B>营养&nbsp; <B>负</B>增长&nbsp; <B>辐</B>射&nbsp; 振<B>幅</B>&nbsp; 不修边<B>幅</B>&nbsp; <B>幅</B>员辽阔&nbsp; 一<B>幅</B>画&nbsp; 一<B>副</B>对联&nbsp; <B>孵</B>化器&nbsp; <br />冷不<B>防</B><B>&nbsp;</B>&nbsp;烟<B>霏</B>&nbsp; <B>流言蜚</B><B>/</B><B>飞语</B><B>&nbsp; </B><B>蜚</B><B>/</B><B>飞短流长</B>&nbsp;&nbsp; <B>蜚</B>声文坛&nbsp; 成绩<B>斐</B>然&nbsp; 受益<B>匪</B>浅&nbsp; <B>匪</B>夷所思&nbsp; 妄自<B>菲</B>薄&nbsp; 三<B>番</B>五次<br /><B>翻/幡</B>然悔悟&nbsp; <B>翻</B>两<B>番&nbsp; </B><B>翻</B>云覆雨&nbsp; 天<B>翻</B>地<B>覆</B>&nbsp; <B>覆</B>盖面广&nbsp; <B>覆</B>水难收&nbsp; 重蹈<B>覆</B>辙&nbsp; 无以<B>复</B>加&nbsp; 死灰<B>复</B>燃&nbsp; <B>繁</B>文<B>缛</B>节&nbsp; <br />删<B>繁</B>就简&nbsp; <B>繁</B>重&nbsp; 不厌其<B>烦&nbsp; </B>要言不<B>烦</B>&nbsp; <B>繁</B><B>/</B><B>烦</B>难<B>&nbsp;&nbsp; </B><B>繁</B><B>/</B><B>烦</B>言&nbsp;&nbsp; <B>繁</B><B>/</B><B>烦</B>杂<B>&nbsp;&nbsp; </B><B>繁</B><B>/</B><B>烦</B>乱&nbsp;<B>&nbsp;</B><B>繁</B><B>/</B><B>烦</B>冗<B>&nbsp; </B><B>繁</B><B>/</B><B>烦</B>琐&nbsp; <B>反</B>复&nbsp; <br />适得其<B>反&nbsp; </B>物极必<B>反</B>&nbsp; 易如<B>反</B>掌&nbsp; 辗转<B>反</B>侧&nbsp; 举一<B>反</B>三&nbsp; 义无<B>反</B><B>/</B><B>返</B>顾&nbsp; <B>返</B>航&nbsp; <B>返</B>工&nbsp; <B>返</B>青&nbsp; <B>返</B>聘&nbsp; <B>返</B>修率&nbsp; 一去不<B>返</B>&nbsp; <br />回光<B>返</B>照&nbsp; 积重难<B>返</B>&nbsp; 流连忘<B>返</B>&nbsp; <B>返</B>老还童&nbsp; <B>返</B>璞归真&nbsp; <B>发愤</B><B>/</B><B>奋</B>&nbsp; <B>奋</B>发图强&nbsp;&nbsp; 发<B>愤</B>图强&nbsp; <B>奋</B>不顾身&nbsp; <B>奋</B>笔疾书&nbsp; <br /><B>愤</B>世嫉俗&nbsp;&nbsp; 浪<B>费&nbsp; </B>耗<B>费</B> 煞<B>费</B>心机 <B>费</B>力劳心 不<B>费</B>吹灰之力&nbsp; <B>废</B>物 <B>废</B>弛 半途而<B>废</B><B> </B>百<B>废</B>待兴 <B>废</B>寝忘食 安<B>分</B><B>&nbsp; </B>本<B>分</B> <br />福<B>分</B> <B>分</B>内&nbsp; <B>分</B>外&nbsp; 恰如其<B>分</B>&nbsp; 非<B>分</B>之想&nbsp; 积极<B>分</B>子 处<B>分</B>&nbsp; 股<B>份</B>&nbsp; 凑<B>份</B>子 <B>身份</B><B>/</B><B>分</B><B> </B><B>肤</B>浅&nbsp; <B>浮</B>浅&nbsp; 入不<B>敷</B>出&nbsp; 破<B>釜</B>沉舟&nbsp; <br /><B>釜</B>底抽薪&nbsp; 班门弄<B>斧</B>&nbsp; 沁人肺<B>腑</B>&nbsp; 牵强<B>附</B>会 趋炎<B>附</B>势 <B>付</B>之一笑&nbsp; 物<B>阜</B>民丰．<br />G<br />气<B>概</B>&nbsp;&nbsp;&nbsp;以偏<B>概</B>全&nbsp; 言简意<B>赅&nbsp; </B>八<B>竿</B><B>/</B><B>杆</B>子打不着<B>&nbsp; </B>竹<B>竿</B>&nbsp; 钓鱼<B>竿</B>&nbsp; 立<B>竿</B>见影&nbsp; 揭<B>竿</B>而起&nbsp; 百尺<B>竿</B>头&nbsp; 笔<B>杆</B>（gǎn） <br />秤<B>杆</B>（gǎn）&nbsp; 旗<B>杆</B>（gān） 电线<B>杆</B>（gān） 高粱<B>秆</B>&nbsp; 秸<B>秆&nbsp; </B><B>个</B>别处理&nbsp; <B>各</B>别对待&nbsp; 如<B>鲠</B>在喉&nbsp; 卑<B>躬</B>屈膝&nbsp; 打<B>躬</B>作揖<br />前倨后<B>恭</B><B>&nbsp;&nbsp; </B><B>功</B>败垂成&nbsp;&nbsp; <B>功</B>亏一篑&nbsp;&nbsp; 好大喜<B>功</B>&nbsp;&nbsp; <B>攻</B>其不备&nbsp; 鬼斧神<B>工&nbsp; </B>异曲同<B>工</B>&nbsp;&nbsp; 耽误<B>工</B>夫&nbsp; 中国<B>功</B>夫&nbsp; <br />下<B>功</B>夫&nbsp; 一笔<B>勾</B>销&nbsp; <B>勾</B>通内奸&nbsp; <B>沟</B>通交流&nbsp; 余勇可<B>贾</B>&nbsp;&nbsp; 一<B>鼓</B>作气<br />悬梁刺<B>股</B>&nbsp; <br />寒风刺<B>骨</B><br />步入正<B>轨</B><B> </B>&nbsp;<br />行踪<B>诡</B>秘&nbsp; <br />阴谋<B>诡</B>计&nbsp; <br />心怀<B>鬼</B>胎&nbsp; <br />食不<B>果</B>腹&nbsp; <br />马革<B>裹</B>尸&nbsp; <br />巾<B>帼</B>英雄&nbsp; <B>&nbsp;</B><br /><B>固</B><B>/</B><B>故步自封</B><B>&nbsp; </B>&nbsp;<br /><B>辜</B>负&nbsp; <br /><B>姑</B>息养奸&nbsp; <br /><B>沽</B>名钓誉<br />待价而<B>沽</B><br /><B>蛊</B>惑人心<br /><B>顾</B>名思义 <br />明知<B>故</B>犯&nbsp; <br /><B>固</B>守成规 <br />灌唱片&nbsp; <br /><B>灌</B>输心血&nbsp; <br />醍醐<B>灌</B>顶&nbsp; <br />全神<B>贯</B>注&nbsp; <br />学<B>贯</B>古今&nbsp; <br />如雷<B>贯</B>耳&nbsp; <br />学<B>贯</B>古今&nbsp; <br />恶<B>贯</B>满盈&nbsp; <br />万<B>贯</B>家财&nbsp; <br />鱼<B>贯</B>而出&nbsp; <br />气<B>贯</B>长虹&nbsp; <br />娇生<B>惯</B>养<br />粗<B>犷</B>",
              "Children": []
            },
            {
              "No": "13",
              "Name": "字义、词义辨析",
              "Desc": "v．",
              "Children": []
            },
            {
              "No": "14",
              "Name": "词性",
              "Desc": "",
              "Children": []
            },
            {
              "No": "15",
              "Name": "感情色彩",
              "Desc": "",
              "Children": []
            },
            {
              "No": "16",
              "Name": "构词方式",
              "Desc": "v．",
              "Children": []
            },
            {
              "No": "17",
              "Name": "短语的结构",
              "Desc": "",
              "Children": []
            },
            {
              "No": "18",
              "Name": "成语",
              "Desc": "v．",
              "Children": []
            },
            {
              "No": "19",
              "Name": "词语（熟语）使用",
              "Desc": "v．",
              "Children": []
            },
            {
              "No": "1A",
              "Name": "歇后语、谚语",
              "Desc": "",
              "Children": []
            },
            {
              "No": "1B",
              "Name": "关联词语",
              "Desc": "v．",
              "Children": []
            }
          ]
        },
        ......
```

错误：

```
{
  "stat": 600007,
  "msg":'考点获取失败',
  "data": ""
}
```

### 18.9搜索获取试题列表

jyeoo_exercise_set/search_ques

请求方式：get

作者：meeter

请求参数:

| 参数      | 类型     | 是否必须 | 说明    |
| ------- | ------ | ---- | ----- |
| key     | string | Y    | 关键词   |
| pi      | int    | Y    | 页码    |
| ps      | int    | Y    | 每页记录数 |
| subject | string | Y    | 科目    |
| t       | int    | Y    | 分类    |

注:t:1:选择;2:填空;9解答;0:全部

```
{
  "stat": 0,
  "msg": "成功",
  "data": {
    "Count": 60,
    "PageIndex": 1,
    "PageSize": 2,
    "Message": null,
    "Data": [
      {
        "Points": [
          {
            "Key": "19",
            "Value": "词语（熟语）使用"
          }
        ],
        "Topics": [],
        "Analyse": "",
        "Method": "",
        "Discuss": "",
        "Teacher": "qzr老师",
        "IsTrue": 2,
        "ID": "a4428569-a6c7-4464-8224-11b81f6e410c",
        "Subject": 36,
        "Cate": 1,
        "CateName": "选择题",
        "Label": "（2018•江苏模拟）",
        "LabelReportID": null,
        "Content": "在下面一段文字空缺处依次填入词语，最恰当的一组是（　　）<br />&nbsp;&nbsp;&nbsp; <del>有着学界楷模</del>，<del>一代宗师之美誉</del>的<del>国学大师南怀瑾</del>，是<del>一个传奇式</del>的<del>人物</del>。他精研儒释，道，将中国文化各种思想<bdo class=\"mathjye-underline\">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</bdo>。它不仅是<del>国学大师</del>，诗人，还是中国文化的传播者。他一生行迹奇特，<bdo class=\"mathjye-underline\">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</bdo>，令人犹不尽识其详者。在为人处理方面，他更是将老子，庄子，孔子的智慧集于一身，<bdo class=\"mathjye-underline\">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</bdo><del>一个</del>布道者，在人事纷繁，充满矛盾的社会生活中，为我们详解其“道”。",
        "Options": [
          "融会贯通&nbsp;&nbsp;&nbsp;&nbsp;常情莫测&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;俨然",
          "融贯中西&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;捉摸不透&nbsp;&nbsp;&nbsp;&nbsp;俨然",
          "融会贯通&nbsp;&nbsp;&nbsp;&nbsp;捉摸不透&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;焉然",
          "融贯中西&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;常情莫测&nbsp;&nbsp;&nbsp;&nbsp;焉然"
        ],
        "Answers": [
          "0"
        ],
        "UserAnswers": [],
        "QuesFiles": [],
        "QuesChilds": [],
        "VIP": 0,
        "Point": 0,
        "Degree": 0.6,
        "RC": 1,
        "FavTime": 5,
        "FavCount": 5,
        "ViewCount": 115,
        "DownCount": 2,
        "PaperCount": 20,
        "PaperTime": 20,
        "RealCount": 1
      },
      {
        "Points": [
          {
            "Key": "43",
            "Value": "传记阅读"
          }
        ],
        "Topics": [],
        "Analyse": "",
        "Method": "",
        "Discuss": "",
        "Teacher": "一片树叶老师",
        "IsTrue": 2,
        "ID": "6474ca1e-c49a-430b-8a1e-d4e5a1fd8c71",
        "Subject": 36,
        "Cate": 4,
        "CateName": "现代文阅读",
        "Label": "（2013•漳州一模）",
        "LabelReportID": "380e6e16-e685-4ce6-8b78-7eaee3854833",
        "Content": "阅读下面的文字，完成下列各题。<br /><BDO class=mathjye-aligncenter><del>南怀瑾</del>：手无金印，权倾天下</BDO>&nbsp;&nbsp;&nbsp;&nbsp;9月29日，<del>南怀瑾</del>在位于江苏庙港的太湖大学堂去世。终年95岁。他一生花费很多时间向外人讲述佛经、易经等超脱世俗的学问，但他并非<del>一个</del>全然的出世者，他对世俗权力世界的经略和游移也是其人生的主题<del>之</del>一。<br />&nbsp;&nbsp;&nbsp;&nbsp;采访过<del>南怀瑾</del>的记者注意到<del>一个</del>事实：无论这位被称为“<del>国学大师</del>”的人在哪里落脚，他的门外总有很多人等着，其中有商人、政客和学者。<br />&nbsp;&nbsp;&nbsp;&nbsp;这位老者影响了几代人。<br />&nbsp;&nbsp;&nbsp;&nbsp;<del>南怀瑾</del>的家曾被称为“人民公社”，几乎每晚都高朋满座，许多台湾政要都拜于他门下。有一天，他给这些弟子上课，突然发现这些人大都是肩膀上“戴星星”的军事要员。他数了一下，加起来一共有28颗。<br />近10年来，南方科技大学校长朱清时每年都去拜访<del>南怀瑾</del>4到5次，最近一次登门拜访是9月22日。这次拜访不同于以往，前一天朱清时收到短信，南老师因感冒引发肺炎，病情转重，朱清时接到“病危”短信后，便立即出发赶往庙港。9月29日下午6时，朱清时再次收到短信，得知“南老师圆寂”的消息。在朱清时看来，这位老者影响了几代人，包括他自己。“我想我们失去了一位老师，一位在当今社会为我们引路的提灯人。”他对记者说。<br />&nbsp;&nbsp;&nbsp;&nbsp;人们称<del>南怀瑾</del>为<del>国学大师</del>、中国传统文化的积极传播者、实业家，但这些标签用于他身上，似乎都不算准确。他幼承庭训，少习诸子百家，精通<del>国学</del>、易经和佛学等，却没有一心成为<del>学界</del>权威；他在政界声名鹊起，享誉两岸三地，却对政治有一种先天的敏感，始终保持合理距离；他长袖善舞，虽时而在经贸领域度化，但从不沾染铜臭味。<br />&nbsp;&nbsp;&nbsp;&nbsp;<del>南怀瑾</del>推崇儒家思想，对朋友讲究“仁义”二字，因此朋友众多，遍布政商<del>学界</del>。<br />&nbsp;&nbsp;&nbsp;&nbsp;无论是在他过世前还是<del>之</del>后，怀念和追忆的只言片语在商界要人中不断出现。亚太交流与合作基金会执行副主席肖武男回忆起自己和<del>南怀瑾</del>的第一次见面时最深刻的印象是南对朋友热情相迎。当年，肖向<del>南怀瑾</del>的秘书提出见面请求，秘书要求肖武男提供生辰八字、出生年月，并告知他，南先生繁忙，未必能见。但半小时后，秘书又跟肖武男联系，告知南老师要他马上从上海赶去苏州见他。<br />&nbsp;&nbsp;&nbsp;&nbsp;<del>南怀瑾</del>一生从未谋得半官一职，但海峡两岸的政要均视他为关键<del>人物</del>。在大陆和台湾关系微妙的80年代，<del>南怀瑾</del>是促成两岸“汪辜会谈”的重要触媒。<br />&nbsp;&nbsp;&nbsp;&nbsp;1988年，<del>南怀瑾</del>移居香港，当年在成都军官学校时的老同事、民革中央副主席贾亦斌登门拜访。贾亦斌此行来香港正是希望借助南的影响力，在两岸间搭建<del>一个</del>新的密使平台。当年4月的一天，<del>南怀瑾</del>从香港打电话给他的学生苏志诚：“志诚，你告诉叔叔（指李登辉），那边有贾亦斌带朋友来，你懂不懂朋友啊？”苏志诚答：“听懂了。”南说：“你告诉他，快派人过来！”1990年12月31日，在<del>南怀瑾</del>的引荐下，时任中共中央对台办主任杨斯德与李登辉办公室主任苏志诚在香港<del>南怀瑾</del>的家中见面。此后几年里，双方往来频繁。<br />&nbsp;&nbsp;&nbsp;&nbsp;与南有深交的阳光国际传媒董事长陈平曾在接受采访时回忆道：“南老一直希望在两岸和平发展中起作用，实际上也起了部分作用。”<br />&nbsp;&nbsp;&nbsp;&nbsp;<del>南怀瑾</del>曾经亲笔起草《和平共计协商统一建议书》，交密使分别送达两岸最高当局，提出三条基本原则：一是和平共计、祥化宿怨；二是同心合作、发展经济；三是协商国家民族统一大业。但建议书发出后却没有得到有关方面的回复，最后流产。这成为他一生未了的心愿。<br /><BDO class=mathjye-alignright>（选自《博客天下》杂志105期，有删改）</BDO>（1）下列对作品的概括和分析，不正确的两项是<!--BA--><div class='quizPutTag' contenteditable='true'>&nbsp;</div><!--EA--><br />A．<del>南怀瑾</del>一生花很多时间向外人讲述佛经、易经等出世的学问，但实际上他是以<del>一个</del>入世者的角度来讲出世的学问的。<br />B．<del>南怀瑾</del>的家曾被称为“人民公社”，几乎每晚高朋满座，众多“戴星星”的军事要员来听他讲课，体现了人们对其依赖心理。<br />C．朱清时对记者说的“我们失去了一位老师，一位在当今社会为我们引路的提灯人”，从侧面体现了<del>南怀瑾</del>的价值与影响。<br />D．“他长袖善舞，虽时而在经贸领域度化，但从不沾染铜臭味”这句话可作为他“并非<del>一个</del>全然的出世者”的<del>一个</del>形象阐释。<br />E．本文记叙、描写与议论相结合，从文化的积极传播者、实业家的层面全方位地展示了<del>国学大师南怀瑾</del>超脱世俗的形象。<br />（2）文章写到南方科技大学校长朱清时两次收到短信，有何作用？<br />（3）<del>南怀瑾</del>作为<del>一个</del>学者可谓“手无金印”，而作者为什么说他“权倾天下”？请结合文本探析其原因。",
        "Options": [],
        "Answers": [
          "BE"
        ],
        "UserAnswers": [],
        "QuesFiles": [],
        "QuesChilds": [],
        "VIP": 0,
        "Point": 0,
        "Degree": 0.4,
        "RC": 1,
        "FavTime": 0,
        "FavCount": 0,
        "ViewCount": 9,
        "DownCount": 0,
        "PaperCount": 1,
        "PaperTime": 1,
        "RealCount": 1
      }
    ],
    "EXData": null,
    "Keys": [
      "有着",
      "学界",
      "楷模",
      "一代宗师",
      "之",
      "美誉",
      "国学",
      "大师",
      "南怀瑾",
      "一个",
      "传奇式",
      "人物"
    ],
    "TotalPage": 30
  }
}
```



### 18.10按章节获取试题列表

jyeoo_exercise_set/search_ques_by_chapter

请求方式：get

作者：meeter

请求参数:

|        参数 | 类型      | 是否必须 | 说明                              |
| --------: | :------ | ---- | ------------------------------- |
|   subject | string  | Y    | 学科                              |
|    bookID | string  | Y    | 教材标识（参考获取教材API 18.12）           |
| chapterID | string  | Y    | 章节标识（参考获取教材API 18.12）           |
|   pointNo | string  | Y    | 章节考点                            |
|      cate | int     | Y    | 题型（参考获取试题题型API 18.5，“0”不限题型）    |
|    degree | int     | Y    | 难度（0：不限；1：基础题；2：中档题；3：难题）       |
|        sc | boolean | Y    | 真题集（True：真题；False：不限）           |
|        gc | boolean | Y    | 好题集（同上）                         |
|        rc | boolean | Y    | 常考题（同上）                         |
|        yc | boolean | Y    | 压轴题（同上）                         |
|        ec | boolean | Y    | 易错题（同上）                         |
|        er | boolean | Y    | 用户题型（同上）                        |
|    region | string  | Y    | 地区（参考获取地区API，“空”不限地区）           |
|    source | int     | Y    | 来源（参考获取试题来源API，“0”不限来源）         |
|        po | int     | Y    | 排序（0：综合排序；1：组卷次数；2：真题次数；3：试题难度） |
|        pd | int     | Y    | 升序（0：升序；1：降序）                   |
|        pi | int     | Y    | 页码                              |
|        ps | int     | Y    | 每页记录数                           |

注：没有必要传的参数，请传空字符串（chapterID，pointNo）

返回：正确



### 18.11获取教材版本

jyeoo_exercise_set/get_editions

请求方式：post

作者：meeter

请求参数:参考18.5

返回：成功

```
{
  "stat": 0,
  "msg": "成功",
  "data": [
    {
      "Key": 17,
      "Value": "人教新版"
    },
    {
      "Key": 2,
      "Value": "北师大版"
    },
    {
      "Key": 18,
      "Value": "沪科新版"
    },
    {
      "Key": 19,
      "Value": "教科新版"
    },
    {
      "Key": 20,
      "Value": "鲁教五四新版"
    },
    {
      "Key": 21,
      "Value": "北京课改新版"
    },
    {
      "Key": 12,
      "Value": "沪粤版"
    },
    {
      "Key": 13,
      "Value": "教科版"
    },
    {
      "Key": 14,
      "Value": "沪教版"
    },
    {
      "Key": 15,
      "Value": "苏教版"
    },
    {
      "Key": 1,
      "Value": "人教版"
    },
    {
      "Key": 16,
      "Value": "粤教版"
    },
    {
      "Key": 10,
      "Value": "鲁教五四版"
    },
    {
      "Key": 9,
      "Value": "北京课改版"
    },
    {
      "Key": 8,
      "Value": "沪科版"
    },
    {
      "Key": 6,
      "Value": "苏科版"
    },
    {
      "Key": 4,
      "Value": "浙教版"
    },
    {
      "Key": 3,
      "Value": "华师大版"
    }
  ]
}
```

### 18.12获取学科教材信息

jyeoo_exercise_set/get_book

请求方式：post

作者：meeter

请求参数:参考18.5

返回：成功

```
{
  "stat": 0,
  "msg": "成功",
  "data": [
    {
      "ID": "b6174fb4-1c52-4186-a1d0-a79ec6087045",
      "GradeID": 10,
      "TermID": 1,
      "EditionID": 1,
      "Name": "必修1",
      "Desc": "",
      "Cover": "",
      "Type": 1,
      "Status": 2,
      "Subject": 30,
      "Term": "上学期",
      "Grade": "高一",
      "Edition": "人教A版",
      "TypeName": "必修1",
      "Children": [
        {
          "ID": "acfe19cd-7319-4d79-8409-54462213d84d",
          "PID": null,
          "BookID": "b6174fb4-1c52-4186-a1d0-a79ec6087045",
          "Name": "第1章 集合与函数概念",
          "Seq": 1,
          "Desc": "<IMG alt=\"\" src=\"http://img.jyeoo.net/quiz/images/201104/17/ba9198a3.png\">",
          "DotName": "第1章 集合与函数概念",
          "PointNo": null,
          "PK": "b6174fb4-1c52-4186-a1d0-a79ec6087045~acfe19cd-7319-4d79-8409-54462213d84d~",
          "Subject": 30,
          "Children": [
            {
              "ID": "38a4964f-d901-423b-a5ab-62adecde8b0c",
              "PID": "acfe19cd-7319-4d79-8409-54462213d84d",
              "BookID": "b6174fb4-1c52-4186-a1d0-a79ec6087045",
              "Name": "1.1 集合",
              "Seq": 1,
              "Desc": "",
              "DotName": "1.1 集合",
              "PointNo": null,
              "PK": "b6174fb4-1c52-4186-a1d0-a79ec6087045~38a4964f-d901-423b-a5ab-62adecde8b0c~",
              "Subject": 30,
              "Children": [
                {
                  "ID": "8335e0a5-608c-43a7-8b63-5da8ee8d9a10",
                  "PID": "38a4964f-d901-423b-a5ab-62adecde8b0c",
                  "BookID": "b6174fb4-1c52-4186-a1d0-a79ec6087045",
                  "Name": "1.1.1 集合的含义与表示",
                  "Seq": 1,
                  "Desc": "<IMG alt=\"\" src=\"http://img.jyeoo.net/quiz/images/201104/17/2426a3d6.png\">",
                  "DotName": "1.1.1 集合的含义与表示",
                  "PointNo": null,
                  "PK": "b6174fb4-1c52-4186-a1d0-a79ec6087045~8335e0a5-608c-43a7-8b63-5da8ee8d9a10~",
                  "Subject": 30,
                  "Children": [],
                  "Points": [
                    {
                      "Key": "11",
                      "Value": "集合的含义"
                    },
                    {
                      "Key": "12",
                      "Value": "元素与集合关系的判断"
                    },
                    {
                      "Key": "13",
                      "Value": "集合的确定性、互异性、无序性"
                    },
                    {
                      "Key": "14",
                      "Value": "集合的分类"
                    },
                    {
                      "Key": "15",
                      "Value": "集合的表示法"
                    }
                  ]
                },
                {
                  "ID": "ebe1c444-7a14-45e8-af2d-bff95449e9e6",
                  "PID": "38a4964f-d901-423b-a5ab-62adecde8b0c",
                  "BookID": "b6174fb4-1c52-4186-a1d0-a79ec6087045",
                  "Name": "1.1.2 集合间的基本关系",
                  "Seq": 2,
                  "Desc": "",
                  "DotName": "1.1.2 集合间的基本关系",
                  "PointNo": null,
                  "PK": "b6174fb4-1c52-4186-a1d0-a79ec6087045~ebe1c444-7a14-45e8-af2d-bff95449e9e6~",
                  "Subject": 30,
                  "Children": [],
                  "Points": [
                    {
                      "Key": "16",
                      "Value": "子集与真子集"
                    },
                    {
                      "Key": "18",
                      "Value": "集合的包含关系判断及应用"
                    },
                    {
                      "Key": "19",
                      "Value": "集合的相等"
                    },
                    {
                      "Key": "1A",
                      "Value": "集合中元素个数的最值"
                    },
                    {
                      "Key": "1B",
                      "Value": "空集的定义、性质及运算"
                    },
                    {
                      "Key": "1C",
                      "Value": "集合关系中的参数取值问题"
                    }
                  ]
                },
                {
                  "ID": "d953232b-f2cc-4928-901f-11cf956c1031",
                  "PID": "38a4964f-d901-423b-a5ab-62adecde8b0c",
                  "BookID": "b6174fb4-1c52-4186-a1d0-a79ec6087045",
                  "Name": "1.1.3 集合的基本运算",
                  "Seq": 3,
                  "Desc": "",
                  "DotName": "1.1.3 集合的基本运算",
                  "PointNo": null,
                  "PK": "b6174fb4-1c52-4186-a1d0-a79ec6087045~d953232b-f2cc-4928-901f-11cf956c1031~",
                  "Subject": 30,
                  "Children": [],
                  "Points": [
                    {
                      "Key": "1D",
                      "Value": "并集及其运算"
                    },
                    {
                      "Key": "1E",
                      "Value": "交集及其运算"
                    },
                    {
                      "Key": "1F",
                      "Value": "补集及其运算"
                    },
                    {
                      "Key": "1G",
                      "Value": "全集及其运算"
                    },
                    {
                      "Key": "1H",
                      "Value": "交、并、补集的混合运算"
                    },
                    {
                      "Key": "1J",
                      "Value": "Venn图表达集合的关系及运算"
                    }
                  ]
                }
              ],
            .......
```

### 18.13按考点获取试题列表

jyeoo_exercise_set/search_point

请求方式：post

作者：meeter

请求参数: 

| 参数      | 类型      | 是否必须 | 说明                              |
| ------- | ------- | ---- | ------------------------------- |
| subject | string  | Y    | 科目                              |
| level1  | string  | Y    | 一级考点（参考获取考点API18.8）             |
| level2  | string  |      | 二级考点                            |
| pointNo | string  |      | 考点                              |
| cate    | string  |      | 题型（参考获取试题题型API18.5，“0”不限题型）     |
| degree  | int     |      | 难度（0：不限；1：基础题；2：中档题；3：难题）       |
| sc      | boolean |      | 真题集（True：真题；False：不限）           |
| gc      | boolean |      | 好题集（True：好题；False：不限）           |
| rc      | boolean |      | 常考题（True：常考题；False：不限）          |
| yc      | boolean |      | 压轴题（True：压轴题；False：不限）          |
| ec      | boolean |      | 易错题（True：易错题；False：不限）          |
| er      | boolean |      | 用户错题（True：错题；False：不限）          |
| region  | string  |      | 地区（参考获取地区API18.4，“空”不限地区）       |
| source  | string  |      | 来源（参考获取试题来源API，“0”不限来源）         |
| po      | int     | Y    | 排序（0：综合排序；1：组卷次数；2：真题次数；3：试题难度） |
| pd      | int     | Y    | 升降（0：升序；1：降序）                   |
| pi      | int     | Y    | 页码                              |
| ps      | int     | Y    | 每页记录数                           |

返回：成功

### 18.14获取试题解析

jyeoo_exercise_set/get_ques

请求方式：post

作者：meeter

请求参数:

| 参数      | 类型     | 是否必须 | 说明   |
| ------- | ------ | ---- | ---- |
| subject | string | Y    | 科目   |
| id      | int    | Y    | 试题标识 |

返回：成功

```
{
  "stat": 0,
  "msg": "成功",
  "data": {
    "ID": "f68fa96e-8e80-4d39-b619-a0b17bd46abb",
    "Content": "如果a的倒数是-1，则a<SUP>2015</SUP>的值是（　　）",
    "Method": "解：由a的倒数是-1，得a=-1．<br />a<SUP>2015</SUP>=（-1）<SUP>2015</SUP>=-1，<br />故选：B．",
    "Analyse": "根据乘积为1的两个数互为倒数，可得a的值，再根据负数的奇数次幂是负数，可得答案．",
    "Discuss": "本题考查了倒数，先利用倒数求出a的值，再利用负数的奇数次幂是负数．",
    "Degree": 0.8,
    "Cate": 1,
    "Label": "（2017•莒县模拟）",
    "LabelYear": 2017,
    "VIP": 0,
    "Remark": "",
    "Answer": "<sl><Answer><c><string>1</string></c><o><string>1</string><string>-1</string><string>2015</string><string>-2015</string></o></Answer></sl>",
    "Origin": 1,
    "Source": 4,
    "Source2": 1,
    "RC": 5,
    "EC": 0,
    "GC": 0,
    "YZ": false,
    "CPID": null,
    "U0ID": null,
    "U0RQ": null,
    "U1ID": null,
    "U1RQ": null,
    "U2ID": null,
    "U2RQ": null,
    "U3ID": null,
    "U3RQ": null,
    "U4ID": null,
    "U4RQ": null,
    "U5ID": null,
    "U5RQ": null,
    "U6ID": null,
    "U6RQ": null,
    "U7ID": null,
    "U7RQ": null,
    "Error": 0,
    "Flags": "110   ",
    "ATMP": 12,
    "Date": "0001-01-01T00:00:00",
    "Status": 0,
    "Demand": "",
    "UseTime": 0,
    "Batch": 0,
    "UTSA": null,
    "SyncDate": "0001-01-01T00:00:00",
    "QuesPoints": [],
    "QuesPoint2s": [],
    "QuesTopics": [],
    "QuesGroups": [],
    "QuesFiles": [],
    "QuesChilds": [],
    "Subject": 20,
    "Seq": 0,
    "Score": 0,
    "Other": 0,
    "CateName": "选择题",
    "Teacher": "",
    "UserAnswers": [],
    "UserScores": [],
    "IsTrue": 2,
    "Points": [
      {
        "Key": "17",
        "Value": "倒数"
      },
      {
        "Key": "1E",
        "Value": "有理数的乘方"
      }
    ],
    "Topics": [],
    "Options": [
      "1",
      "-1",
      "2015",
      "-2015"
    ],
    "Answers": [
      "1"
    ],
    "Star": "★☆☆☆☆",
    "Point": 0,
    "FavTime": 77,
    "ViewCount": 6647,
    "DownCount": 98,
    "RealCount": 5,
    "PaperCount": 1447,
    "ParentID": null,
    "ParentContent": null,
    "LabelReportID": "1b2d7fe7-a6e2-4edd-87a8-c272b6c1bbbd"
  }
}
```

失败：

```
{
  "stat": 600012,
  "msg": "解析获取失败",
  "data": ''
}
```

### 18.15提交组卷

jyeoo_exercise_set/post_paper

请求方式：post

作者：meeter

请求参数:

| 参数      | 类型     | 是否必须 | 说明$id |
| ------- | ------ | ---- | ----- |
| ID      | string | 是    | 组卷标识  |
| title   | string | 是    | 组卷名称  |
| des     | string | 是    | 组卷描述  |
| score   | int    | 是    | 分值    |
| times   | int    | 是    | 时间    |
| arr     | json   | 是    | 组卷信息  |
| subject | string | 是    | 科目    |

注:arr数据格式:

```
$arr=[
            1=>[
                0=>[
                    'id'=>"edf936b7-27ee-456e-ad54-555a065c8653",
                    'se'=>5,
                    'seq'=>1
                    ],
                1=>[
                    'id'=>"99f079ce-925b-42ee-8dd3-bd559b764bad",
                    'se'=>5,
                    'seq'=>2
                    ]
                ],
            2=>[
                0=>[
                    'id'=>"2be4d2da-7810-45fb-a2b1-9b88c4189d01",
                    'se'=>8,
                    'seq'=>3
                ]
            ]
        ];
```

id:试题标识;se:分值;seq:题号;1:代表选择;2:代表填空;3:解答

再转化为json对象;

返回:

```
{
  "stat": 0,
  "msg": "成功",
  "data": {
    "ID": "e2505a19-bbef-4b74-9bc9-622799402010"
  }
}
```





### 18.16获取班级

jyeoo_exercise_set/get_grades

请求方式：post

作者：meeter

请求参数:无

返回：

```
{
  "stat": 0,
  "msg": "成功",
  "data": [
    {
      "Key": 1,
      "Value": "一年级"
    },
    {
      "Key": 2,
      "Value": "二年级"
    },
    {
      "Key": 3,
      "Value": "三年级"
    },
    {
      "Key": 4,
      "Value": "四年级"
    },
    {
      "Key": 5,
      "Value": "五年级"
    },
    {
      "Key": 6,
      "Value": "六年级"
    },
    {
      "Key": 7,
      "Value": "七年级"
    },
    {
      "Key": 8,
      "Value": "八年级"
    },
    {
      "Key": 9,
      "Value": "九年级"
    },
    {
      "Key": 10,
      "Value": "高一"
    },
    {
      "Key": 11,
      "Value": "高二"
    },
    {
      "Key": 12,
      "Value": "高三"
    }
  ]
}
```

### 18.17获取试题题干

jyeoo_exercise_set/get_batch_queses

请求方式：post

作者：meeter

请求参数:

| 参数      | 是否必须 | 类型     | 说明   |
| ------- | ---- | ------ | ---- |
| subject | Y    | string | 科目   |
| id      | Y    | string | 试题标识 |

返回：

### 18.18下载组卷

jyeoo_exercise_set/down_paper

请求方式：post

作者：meeter

请求参数:

| 参数      | 是否必须 | 类型     | 说明               |
| ------- | ---- | ------ | ---------------- |
| subject | 是    | string | 科目               |
| id      | 是    | string | 组卷标识             |
| name    | 是    | string | 完整路径             |
| method  | 否    | int    | 组卷解析(默认等于31(全部)) |

注:分析 值1,解答2,点评4,考点8,专题16,如:下载分析+解答+点评 ,method就是1+2+4=7.组卷下载次数有限,个位数.

```
{
  "stat": 0,
  "msg": "成功",
  "data": {
    'ok'
  }
}
```

### 18.19搜索试卷

jyeoo_exercise_set/search_paper

请求方式：post

作者：meeter

请求参数:

| 参数      | 是否必须 | 类型     | 说明    |
| ------- | ---- | ------ | ----- |
| subject | 是    | string | 科目    |
| key     | 是    | string | 关键词   |
| pi      | 是    | int    | 页码    |
| ps      | 是    | int    | 每页记录数 |

返回

```
{
  "stat": 0,
  "msg": "成功",
  "data": {
    "Count": 60,
    "PageIndex": 1,
    "PageSize": 20,
    "Message": null,
    "Data": [
      {
        "ID": "7fb6d9c6-9f24-4643-9ddf-7d6b68d7a3ec",
        "Title": "新人教版八年级（上）《第<del>1</del>章 机械运动》2017年单元测试卷（<del>1</del>）",
        "ViewCount": 2116,
        "DownCount": 327,
        "FavoriteCount": 43,
        "WFID": "0d70fdb0-a2f4-4563-8089-db66929e442e",
        "Region": "       ",
        "UserID": "e79e1952-66f5-40ae-9d3a-2e26b28028bb",
        "Date": "2016-10-04T04:14:07.063",
        "Status": 8,
        "Fee": 0,
        "Point": 3,
        "VIP": 0,
        "Count": 0,
        "Subject": 21,
        "Groups": [],
        "RegionName": null,
        "Year": 0,
        "SourceName": null,
        "GradeName": null,
        "TermName": null,
        "EditionName": null,
        "SchoolName": null,
        "Score": 0,
        "Degree": 0,
        "Times": 0
      },
      {
        "ID": "06310b5f-698a-4411-9745-30625bf1f0a6",
        "Title": "苏科版八年级（上）《第<del>1</del>章 声现象》2016年单元测试卷（<del>1</del>）",
        "ViewCount": 63,
        "DownCount": 9,
        "FavoriteCount": 2,
        "WFID": "d9128b25-5086-4e6f-8e71-43fdcfeeefa9",
        "Region": "       ",
        "UserID": "e79e1952-66f5-40ae-9d3a-2e26b28028bb",
        "Date": "2016-10-05T04:13:58.627",
        "Status": 8,
        "Fee": 0,
        "Point": 3,
        "VIP": 0,
        "Count": 0,
        "Subject": 21,
        "Groups": [],
        "RegionName": null,
        "Year": 0,
        "SourceName": null,
        "GradeName": null,
        "TermName": null,
        "EditionName": null,
        "SchoolName": null,
        "Score": 0,
        "Degree": 0,
        "Times": 0
      },
      {
        "ID": "0b504d91-f2c7-49d6-9450-7435b0a8f7a4",
        "Title": "2016-2017学年新人教版八年级（上）质检物理试卷（<del>1</del>-2章）（<del>1</del>）",
        "ViewCount": 24,
        "DownCount": 5,
        "FavoriteCount": 1,
        "WFID": "2c4781a7-785b-4da0-ac1d-63cdc0070372",
        "Region": "       ",
        "UserID": "e79e1952-66f5-40ae-9d3a-2e26b28028bb",
        "Date": "2017-08-08T03:13:04.963",
        "Status": 8,
        "Fee": 0,
        "Point": 4,
        "VIP": 0,
        "Count": 0,
        "Subject": 21,
        "Groups": [],
        "RegionName": null,
        "Year": 0,
        "SourceName": null,
        "GradeName": null,
        "TermName": null,
        "EditionName": null,
        "SchoolName": null,
        "Score": 0,
        "Degree": 0,
        "Times": 0
      },
      {
        "ID": "1a76cb9b-feab-4f69-b73c-c0fab76fb1c7",
        "Title": "新人教版八年级（上）《第<del>1</del>章 机械运动》2015年单元测试卷（湖南省长沙市岳麓区学士街道学士中学）（<del>1</del>）",
        "ViewCount": 514,
        "DownCount": 55,
        "FavoriteCount": 14,
        "WFID": "25eadd86-64a9-43ea-b414-ace0ff9f95ee",
        "Region": "1140103",
        "UserID": "e79e1952-66f5-40ae-9d3a-2e26b28028bb",
        "Date": "2015-11-21T04:09:05.783",
        "Status": 8,
        "Fee": 0,
        "Point": 3,
        "VIP": 0,
        "Count": 0,
        "Subject": 21,
        "Groups": [],
        "RegionName": null,
        "Year": 0,
        "SourceName": null,
        "GradeName": null,
        "TermName": null,
        "EditionName": null,
        "SchoolName": null,
        "Score": 0,
        "Degree": 0,
        "Times": 0
      },
      {
        "ID": "a9c6f58e-42ea-4152-9b69-07d00fb585a0",
        "Title": "苏科版八年级（上）《第<del>1</del>章 声现象》2015年单元测试卷（<del>1</del>）",
        "ViewCount": 169,
        "DownCount": 31,
        "FavoriteCount": 3,
        "WFID": "2858eb26-0a5e-48c9-998e-0e53519719ac",
        "Region": "       ",
        "UserID": "e79e1952-66f5-40ae-9d3a-2e26b28028bb",
        "Date": "2015-11-04T04:12:02.22",
        "Status": 8,
        "Fee": 0,
        "Point": 3,
        "VIP": 0,
        "Count": 0,
        "Subject": 21,
        "Groups": [],
        "RegionName": null,
        "Year": 0,
        "SourceName": null,
        "GradeName": null,
        "TermName": null,
        "EditionName": null,
        "SchoolName": null,
        "Score": 0,
        "Degree": 0,
        "Times": 0
      },
      {
        "ID": "4bbffffe-bd7d-4b40-ba49-23401c3c15d3",
        "Title": "新人教版八年级（上）《第<del>1</del>章 机械运动》2015年单元测试卷（<del>1</del>）",
        "ViewCount": 53,
        "DownCount": 3,
        "FavoriteCount": 0,
        "WFID": "c95f9eb9-1096-4987-aa37-e73f3ac01a69",
        "Region": "       ",
        "UserID": "e79e1952-66f5-40ae-9d3a-2e26b28028bb",
        "Date": "2016-10-25T04:11:59.227",
        "Status": 8,
        "Fee": 0,
        "Point": 3,
        "VIP": 0,
        "Count": 0,
        "Subject": 21,
        "Groups": [],
        "RegionName": null,
        "Year": 0,
        "SourceName": null,
        "GradeName": null,
        "TermName": null,
        "EditionName": null,
        "SchoolName": null,
        "Score": 0,
        "Degree": 0,
        "Times": 0
      },
      {
        "ID": "18b08fa7-b902-4358-b48b-99a4dbcdfec1",
        "Title": "人教版八年级（上）《第<del>1</del>章 机械运动》2014年单元测试卷（<del>1</del>）",
        "ViewCount": 2411,
        "DownCount": 117,
        "FavoriteCount": 35,
        "WFID": "eea62127-e7f2-4112-88e6-8d63583be294",
        "Region": "       ",
        "UserID": "e79e1952-66f5-40ae-9d3a-2e26b28028bb",
        "Date": "2014-08-12T04:08:45.82",
        "Status": 8,
        "Fee": 0,
        "Point": 3,
        "VIP": 0,
        "Count": 0,
        "Subject": 21,
        "Groups": [],
        "RegionName": null,
        "Year": 0,
        "SourceName": null,
        "GradeName": null,
        "TermName": null,
        "EditionName": null,
        "SchoolName": null,
        "Score": 0,
        "Degree": 0,
        "Times": 0
      },
      {
        "ID": "3eca286e-8b09-4e97-9175-03513530db56",
        "Title": "苏科版八年级（上）《第<del>1</del>章 声现象》2014年单元测试卷（<del>1</del>）",
        "ViewCount": 797,
        "DownCount": 35,
        "FavoriteCount": 7,
        "WFID": "e05f9564-0913-4c40-844d-e1de7533be37",
        "Region": "       ",
        "UserID": "0bb2e563-44b3-4471-b49f-46f72964c0ea",
        "Date": "2014-06-17T04:09:39.12",
        "Status": 8,
        "Fee": 0,
        "Point": 3,
        "VIP": 0,
        "Count": 0,
        "Subject": 21,
        "Groups": [],
        "RegionName": null,
        "Year": 0,
        "SourceName": null,
        "GradeName": null,
        "TermName": null,
        "EditionName": null,
        "SchoolName": null,
        "Score": 0,
        "Degree": 0,
        "Times": 0
      },
      {
        "ID": "1aeb9122-d1be-4496-ac46-a4bb5b30f410",
        "Title": "人教版八年级（上）《第<del>1</del>章 机械运动》2014年单元测试卷（湖北省襄阳市南漳县肖堰镇肖堰初级中学）（<del>1</del>）",
        "ViewCount": 410,
        "DownCount": 34,
        "FavoriteCount": 9,
        "WFID": "c2ac5d47-6929-439c-ac0b-ba95cf6cb445",
        "Region": "1130504",
        "UserID": "e79e1952-66f5-40ae-9d3a-2e26b28028bb",
        "Date": "2015-01-30T04:05:21.347",
        "Status": 8,
        "Fee": 0,
        "Point": 3,
        "VIP": 0,
        "Count": 0,
        "Subject": 21,
        "Groups": [],
        "RegionName": null,
        "Year": 0,
        "SourceName": null,
        "GradeName": null,
        "TermName": null,
        "EditionName": null,
        "SchoolName": null,
        "Score": 0,
        "Degree": 0,
        "Times": 0
      },
      {
        "ID": "dffebdcc-9bac-495e-8009-cb73e7a3d1e3",
        "Title": "2014-2015学年河北省承德市承德县二中八年级（上）段考物理试卷（<del>1</del>-2章）（<del>1</del>）",
        "ViewCount": 215,
        "DownCount": 10,
        "FavoriteCount": 2,
        "WFID": "c9f00cdf-18a8-4980-afee-fc5d8e02c010",
        "Region": "1110804",
        "UserID": "e79e1952-66f5-40ae-9d3a-2e26b28028bb",
        "Date": "2014-11-22T04:05:30.88",
        "Status": 8,
        "Fee": 0,
        "Point": 4,
        "VIP": 0,
        "Count": 0,
        "Subject": 21,
        "Groups": [],
        "RegionName": null,
        "Year": 0,
        "SourceName": null,
        "GradeName": null,
        "TermName": null,
        "EditionName": null,
        "SchoolName": null,
        "Score": 0,
        "Degree": 0,
        "Times": 0
      },
      {
        "ID": "4892e5a8-4c1d-42de-b44c-8a575b779717",
        "Title": "新人教版八年级（上）《第<del>1</del>章 机械运动》2013年同步练习卷F（<del>1</del>）",
        "ViewCount": 1448,
        "DownCount": 31,
        "FavoriteCount": 15,
        "WFID": "a2496f7f-21f9-4f55-903d-6a6a00411253",
        "Region": "       ",
        "UserID": "e79e1952-66f5-40ae-9d3a-2e26b28028bb",
        "Date": "2013-09-08T04:05:47.147",
        "Status": 8,
        "Fee": 0,
        "Point": 3,
        "VIP": 0,
        "Count": 0,
        "Subject": 21,
        "Groups": [],
        "RegionName": null,
        "Year": 0,
        "SourceName": null,
        "GradeName": null,
        "TermName": null,
        "EditionName": null,
        "SchoolName": null,
        "Score": 0,
        "Degree": 0,
        "Times": 0
      }
        ]
```

### 18.20获取试卷

jyeoo_exercise_set/get_paper

请求方式：post

作者：meeter

请求参数:

| 参数      | 是否必须 | 类型     | 说明   |
| ------- | ---- | ------ | ---- |
| subject | 是    | string | 科目   |
| ID      | 是    | string | 试卷标识 |

返回

```
{
  "stat": 0,
  "msg": "成功",
  "data": {
    "ID": "7fb6d9c6-9f24-4643-9ddf-7d6b68d7a3ec",
    "Title": "2012年辽宁省铁岭市中考物理试卷",
    "ViewCount": 8555,
    "DownCount": 65,
    "FavoriteCount": 18,
    "WFID": "00000000-0000-0000-0000-000000000000",
    "Region": "1071200",
    "UserID": "00000000-0000-0000-0000-000000000000",
    "Date": "0001-01-01T00:00:00",
    "Status": 0,
    "Fee": 0,
    "Point": 20,
    "VIP": 0,
    "Count": 8555,
    "Subject": 21,
    "Groups": [
      {
        "Key": "一、选择题（本题共12小题，共28分．1～8题为单选题，每小题2分；9～12小题为多选题，每小题2分，漏选得2分，错选得0分）",
        "Value": [
          {
            "ID": "515310af-15a1-4156-b558-c7258c7a4181",
            "Content": "下列各现象中属光折射现象的是（　　）",
            "Method": "",
            "Analyse": "",
            "Discuss": "",
            "Degree": 0.6,
            "Cate": 1,
            "Label": "（2012•铁岭）",
            "LabelYear": 2012,
            "VIP": 0,
            "Remark": "",
            "Answer": "<sl><Answer><c><string>1</string></c><o><string>&lt;img alt=\"菁优网\" src=\"http://img.jyeoo.net/quiz/images/201210/26/6de9564b.png\" style=\"vertical-align:middle\" /&gt;&lt;br /&gt;猫的影子</string><string>&lt;img alt=\"菁优网\" src=\"http://img.jyeoo.net/quiz/images/201210/26/3909ef27.png\" style=\"vertical-align:middle\" /&gt;&lt;br /&gt;“折断”的牙刷</string><string>&lt;img alt=\"菁优网\" src=\"http://img.jyeoo.net/quiz/images/201210/26/cc6dbe04.png\" style=\"vertical-align:middle\" /&gt;&lt;br /&gt;日环食</string><string>&lt;img alt=\"菁优网\" src=\"http://img.jyeoo.net/quiz/images/201210/26/30f372c6.png\" style=\"vertical-align:middle\" /&gt;&lt;br /&gt;塔在水中的倒影</string></o></Answer></sl>",
            "Origin": 1,
            "Source": 1,
            "Source2": 0,
            "RC": 1,
            "EC": 0,
            "GC": 0,
            "YZ": false,
            "CPID": null,
            "U0ID": null,
            "U0RQ": null,
            "U1ID": null,
            "U1RQ": null,
            "U2ID": null,
            "U2RQ": null,
            "U3ID": null,
            "U3RQ": null,
            "U4ID": null,
            "U4RQ": null,
            "U5ID": null,
            "U5RQ": null,
            "U6ID": null,
            "U6RQ": null,
            "U7ID": null,
            "U7RQ": null,
            "Error": 0,
            "Flags": "010   ",
            "ATMP": 0,
            "Date": "0001-01-01T00:00:00",
            "Status": 0,
            "Demand": "",
            "UseTime": 1080,
            "Batch": 0,
            "UTSA": null,
            "SyncDate": "0001-01-01T00:00:00",
            "QuesPoints": [],
            "QuesPoint2s": [],
            "QuesTopics": [],
            "QuesGroups": [],
            "QuesFiles": [],
            "QuesChilds": [],
            "Subject": 21,
            "Seq": 1,
            "Score": 2,
            "Other": 222,
            "CateName": "选择题",
            "Teacher": "",
            "UserAnswers": [],
            "UserScores": [],
            "IsTrue": 2,
            "Points": [
              {
                "Key": "AM",
                "Value": "光的折射现象及其应用"
              }
            ],
            "Topics": [
              {
                "Key": "12 ",
                "Value": "应用题"
              },
              {
                "Key": "513",
                "Value": "光的折射、光的色散"
              }
            ],
            "Options": [
              "<img alt=\"菁优网\" src=\"http://img.jyeoo.net/quiz/images/201210/26/6de9564b.png\" style=\"vertical-align:middle\" /><br />猫的影子",
              "<img alt=\"菁优网\" src=\"http://img.jyeoo.net/quiz/images/201210/26/3909ef27.png\" style=\"vertical-align:middle\" /><br />“折断”的牙刷",
              "<img alt=\"菁优网\" src=\"http://img.jyeoo.net/quiz/images/201210/26/cc6dbe04.png\" style=\"vertical-align:middle\" /><br />日环食",
              "<img alt=\"菁优网\" src=\"http://img.jyeoo.net/quiz/images/201210/26/30f372c6.png\" style=\"vertical-align:middle\" /><br />塔在水中的倒影"
            ],
            "Answers": [
              "1"
            ],
            "Star": "&nbsp;",
            "Point": 0,
            "FavTime": 0,
            "ViewCount": 248,
            "DownCount": 10,
            "RealCount": 1,
            "PaperCount": 5,
            "ParentID": null,
            "ParentContent": null,
            "LabelReportID": "e7809f7b-ec2a-4ed1-bc8d-6cafc8cd1b0f"
          },
          {
            "ID": "9456ff10-c515-15e8-9547-30095825f809",
            "Content": "<img alt=\"菁优网\" src=\"http://img.jyeoo.net/quiz/images/201210/26/96550685.png\" style=\"vertical-align:middle;FLOAT:right\" />如图是甲、两种物质在加热程中温随间变化关的图象，由图象可以断（　　）",
            "Method": "",
            "Analyse": "",
            "Discuss": "",
            "Degree": 0.6,
            "Cate": 1,
            "Label": "（2012•铁岭）",
            "LabelYear": 2012,
            "VIP": 0,
            "Remark": "",
            "Answer": "<sl><Answer><c><string>2</string></c><o><string>甲物质是晶体</string><string>甲物质在第5min时开始熔化</string><string>乙物质的熔点是80℃</string><string>第12min时乙物质是固态</string></o></Answer></sl>",
            "Origin": 1,
            "Source": 1,
            "Source2": 0,
            "RC": 7,
            "EC": 0,
            "GC": 5,
            "YZ": false,
            "CPID": null,
            "U0ID": null,
            "U0RQ": null,
            "U1ID": null,
            "U1RQ": null,
            "U2ID": null,
            "U2RQ": null,
            "U3ID": null,
            "U3RQ": null,
            "U4ID": null,
            "U4RQ": null,
            "U5ID": null,
            "U5RQ": null,
            "U6ID": null,
            "U6RQ": null,
            "U7ID": null,
            "U7RQ": null,
            "Error": 0,
            "Flags": "110   ",
            "ATMP": 0,
            "Date": "0001-01-01T00:00:00",
            "Status": 0,
            "Demand": "",
            "UseTime": 1439,
            "Batch": 0,
            "UTSA": null,
            "SyncDate": "0001-01-01T00:00:00",
            "QuesPoints": [],
            "QuesPoint2s": [],
            "QuesTopics": [],
            "QuesGroups": [],
            "QuesFiles": [],
            "QuesChilds": [],
            "Subject": 21,
            "Seq": 2,
            "Score": 2,
            "Other": 222,
            "CateName": "选择题",
            "Teacher": "",
            "UserAnswers": [],
            "UserScores": [],
            "IsTrue": 2,
            "Points": [
              {
                "Key": "1C",
                "Value": "熔化和凝固的温度—时间图象"
              }
            ],
            "Topics": [
              {
                "Key": "521",
                "Value": "温度计、熔化和凝固"
              }
            ],
            "Options": [
              "甲物质是晶体",
              "甲物质在第5min时开始熔化",
              "乙物质的熔点是80℃",
              "第12min时乙物质是固态"
            ],
            "Answers": [
              "2"
            ],
            "Star": "★★☆☆☆",
            "Point": 0,
            "FavTime": 22,
            "ViewCount": 1141,
            "DownCount": 13,
            "RealCount": 7,
            "PaperCount": 130,
            "ParentID": null,
            "ParentContent": null,
            "LabelReportID": "e7809f7b-ec2a-4ed1-bc8d-6cafc8cd1b0f"
          },
          {
            "ID": "88b10fa5-ed15-4153-5c1e-ae87d325924a",
            "Content": "下列态变化中凝华的是（　　）",
            "Method": "",
            "Analyse": "",
            "Discuss": "",
            "Degree": 0.8,
            "Cate": 1,
            "Label": "（2012•铁岭）",
            "LabelYear": 2012,
            "VIP": 0,
            "Remark": "",
            "Answer": "<sl><Answer><c><string>3</string></c><o><string>春天，河面上冰雪消融</string><string>夏天，树叶上露珠晶莹</string><string>秋天，山谷中雾绕群峰</string><string>冬天，柳树上霜满枝头</string></o></Answer></sl>",
            "Origin": 1,
            "Source": 1,
            .......
```

### 18.21获取试卷解析

jyeoo_exercise_set/get_paper

请求方式：post

作者：meeter

请求参数:

| 参数      | 是否必须 | 类型     | 说明         |
| ------- | ---- | ------ | ---------- |
| subject | Y    | string | 科目         |
| ID      | Y    | string | 试卷标识       |
| method  | N    | int    | 试卷解析(默认31) |

注:分析 值1,解答2,点评4,考点8,专题16,如:下载分析+解答+点评 ,method就是1+2+4=7.

返回:

```
{
  "stat": 600017,
  "msg": "获取试卷Doc数据失败",
  "data": ""
}
```

## 19 播放器

### 19.1 批注插入接口

writing/insert_postil

请求方式：post

作者：meeter

请求参数：

| 参数                | 类型     | 是否必须 | 说明   |
| ----------------- | ------ | ---- | ---- |
| doing_exercise_id | int    | Y    | 互动id |
| student_id        | int    | Y    | 学生id |
| postil            | string | Y    | 批注内容 |
| page              | int    | Y    | 页数   |

返回：

1：成功

```
{
  "stat": 0,
  "msg": "successful",
  "data": ""
}
```

2：失败

```
{
  "stat": -1,
  "msg": "fail",
  "data": ""
}
```


###  19.2批注查询接口

writing/query_postil

请求方式：post

作者：meeter

请求参数：

| 参数                | 类型   | 是否必须 | 说明   |
| ----------------- | ---- | ---- | ---- |
| doing_exercise_id | int  | Y    | 互动id |
| student_id        | int  | Y    | 学生id |
| page              | int  | N    | 页码   |

注：有页码这个参数就返回当前页对应的数据，不传页码，就返回所有的数据。

返回：

1：成功

```
{
  "stat": 0,
  "msg": "successful",
  "data": [
    {
      "postil": "[{\"cmd\":\"penup\",\"po\":{\"x\":1968,\"y\":5789},\"time\":1488678755565}]",
      "page": "1"
    },
    {
      "postil": "[{\"cmd\":\"penup\",\"po\":{\"x\":1968,\"y\":5789},\"time\":1488678755565},							 {\"cmd\":\"penup\",\"po\":{\"x\":1968,\"y\":5789},\"time\":1408678738565}]",
      "page": "2"
    }
  ]
}
```

2：失败

```
{
  "stat": 900001,
  "msg": "批注为空",
  "data": []
}
```