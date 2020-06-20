# 人工智能实训平台前端自动化测试记录

## 环境搭建

本项目选用 `cypress` 进行前端自动化测试。

- [官方网站 —— cypress](https://www.cypress.io/)

安装步骤：

```bash
# 安装 cypress
npm install cypress --save-dev
# 打开 cypress
./node_modules/.bin/cypress open
```

此外可以设置一个 `npm` 脚本：

```json
// package.json
{  
	"scripts": {
		"cypress": "cypress open"
	},
}
```



>控制台提示： **cy is not defined while running cypress test**
>
>1. `npm install eslint-plugin-cypress --save-dev`
>2. 在 `package.json` 或 `.eslintrc.json`中的 `"eslintConfig"` 添加配置项
>
>```json
>{
>  "extends": [
>    "plugin:cypress/recommended"
>  ]
>}
>```
>
>参考文档：
>
>- [Reference Error :cy is not defined while running cypress test](https://stackoverflow.com/questions/58552921/reference-error-cy-is-not-defined-while-running-cypress-test)



### 在 Cypress 中使用 localStorage 

#### Installation

```bash
cnpm i --save-dev cypress-localstorage-commands
```

#### Usage

Add this line to your project's `cypress/support/commands.js`:

```javascript
import "cypress-localstorage-commands"
```

### Commands

Save current localStorage values into an internal "snapshot":

保存当前 localStorage 进入快照：

```javascript
cy.saveLocalStorage();
```



Restore localStorage to previously "snapshot" saved values:

还原 localStorage 至之前的快照：

```javascript
cy.restoreLocalStorage();
```



Clear localStorage "snapshot" values:

清空快照中的 localStorage：

```javascript
cy.clearLocalStorageSnapshot();
```



Get localStorage item. Equivalent to `localStorage.getItem` in browser:

获取 localStorage 的值，等价于 `localStorage.getItem`：

```javascript
cy.getLocalStorage("item");
```



Set localStorage item. Equivalent to `localStorage.setItem` in browser:

设置 localStorage 的值，等价于 `localStorage.setItem`：

```javascript
cy.setLocalStorage("item", "value");
```



Remove localStorage item. Equivalent to `localStorage.removeItem` in browser:

移除 localStorage 的值，等价于 `localStorage.removeItem`：

```javascript
cy.removeLocalStorage("item");
```



#### Reference： 

- https://github.com/javierbrea/cypress-localstorage-commands

## 分解模块

由此前的单元测试转为 `E2E` 测试，文件编排的思路也需要改变，如此前更多是以单文件为一个单位去测试各项函数是否输入如同预期，现在的思路就应该转为：

- **将项目按照功能模块进行拆分，以模块为单位**

### 登录模块

#### 测试步骤：

1. 进入项目首页 （管理员登录部分）
2. 在用户名&密码栏中输入对应字符
3. `click` 点击登录按钮
4. 检测是否跳转到到指定路径
5. 是否弹出登录成功提示

---

6. 注销 （学生用户登录部分）
7. 在用户名&密码栏中输入对应字符
8. `click` 点击登录按钮
9. 检测是否跳转到到指定路径
10. 是否弹出登录成功提示



### 忘记密码模块

#### 测试步骤：

1. 进入项目首页
2. `click` 忘记密码
3. 填入用户名
4. `click` 发送验证码
5. 检测是否弹出已发送邮件提示
6. 暂停
7. 填入邮件中的验证码
8. 点击重置密码
9. 检测是否弹出重置密码成功提示



### 注册模块

#### 测试步骤：

1. 复用 —— 登录模块（管理员）
2. 记录目前待审批人数
3. 注销
4. 点击注册按钮
5. 填入相关信息（可否mock）
6.  `click`发送验证码 & 检测是否弹出验证码发送成功提示
7. 手动填入验证码
8. `click` 注册按钮
9.  检测是否弹出已收到申请提示
10. `click` 登录按钮
11. 复用 —— 登录模块
12. 记录目前待审批人数，与此前人数比较，是否增加1



### 管理员后台组件显示模块

#### 测试步骤：

1. 复用 —— 登录模块（管理员）
2. 测试标题是否正常显示
3. 测试头像是否正常显示
4. 测试左侧菜单栏是否正常显示



### 用户操作模块

#### 测试步骤：

1. 复用 —— 登录模块（管理员）

---

2.  记录第二条学生用户名 & 用户总数（删除用户部分）
   1.  `String.indexOf()` 找到需要截取的位置
   2.  `String.slice(start, end)` 截取需要用的字符串
   3.  `parseInt(<String>)` 字符串转Number
3. `click` 第一条学生信息的 `CheckBox`
4. `click()`删除用户按钮
5. `click()` 确定按钮
6.  比较现在第一条学生用户名是否与之前保存的第二条学生用户名相等
7.  记录现在用户总数 & 与之前用户总数比较是否 `-1`

---

8. `click` 新建用户按钮	(新增用户部分)
9.  填入相关信息
10.  `click`确定按钮
11.  检测是否出现新增用户成功提示
12. 比较之前用户总数是否`+1`



(审批部分——拒绝)

1. 复用 —— 登录模块（管理员）
2. 点击 “待审批”
3. 记录当前待审批人数
4. 记录第一个用户的用户名
5. 勾选第一个用户
6. 点击 “拒绝”
7. 点击 “确定”
8. 记录当前待审批人数
9. 与此前记录的待审批人数比较期待差值为1
10. 点击 “审批历史”
11. 记录第一条用户名
12. 与此前记录的用户名比较是否相同
13. 检测当前第一条信息审批状态是否显示为 “拒绝”

---

(审批部分——通过)

1. 复用 —— 登录模块（管理员）
2. 记录用户总数
3. 点击 “待审批”
4. 记录当前待审批人数
5. 记录第一个用户的用户名
6. 勾选第一个用户
7. 点击 “通过”
8. 记录当前待审批人数
9. 与此前记录的待审批人数比较期待差值为1
10. 点击 “审批历史”
11. 记录第一条用户名
12. 与此前记录的用户名比较是否相同
13. 检测当前第一条信息审批状态是否显示为 “通过”
14. 点击 “用户列表”
15. 记录用户总数
16. 与此前记录的用户总数比较，预期差值为1



### 用户详情显示&更改用户信息模块

#### 测试步骤：

1. 复用 —— 登录模块（管理员）
2. 记录第一条用户信息：
   1. 用户名
   2. 真实姓名
   3. 公司
   4. 类型
   5. 所属班级
3. 点击 “第一行用户的用户名”，进入用户详情
4. 记录页面中显示的详情
5. 与此前记录的第一条用户信息比较，期望值：true
6. 点击 “编辑”
7. 在“真实姓名” & “公司” 加入salt
8. 点击“保存”
9. 记录当前用户所有信息
   1. 用户名
   2. 真实姓名
   3. 公司
   4. 类型
   5. 所属班级
10. 点击“返回”
11. 记录第一条用户信息
12. 与上一步保存的信息做比较，期望值为：true





### 课程操作模块

#### 新建课程测试步骤：

1. 点击 “课程管理”
2. 记录当前课程总数
3. 点击 “新建课程”
4. 填入信息，记录课程名称
   1. 课程ID —— mock
   2. 课程名称 —— mock
   3. 内容类型 —— Jupyter
   4. 环境 —— PUB-GJ-JPV2_0-JPB-CHXY-PythonMathV1_0
5. 点击 “确定”
6. 检测是否弹出成功提示
7. 记录当前课程总数
8. 与之前课程数比较，期望差值为：+1



#### 删除课程测试步骤：

1. 点击 “课程管理”
2. 记录当前课程总数
3. 点击“删除”（第一条课程信息）
4. 点击“确定”
5. 检测是否提示成功删除课程
6. 记录当前课程总数
7. 与之前记录的课程总数比较，期望差值为：



#### 修改课程详情步骤：

1. 记录第一条课程信息
   1. 课程名称
   2. 课程类型
2. 点击“第一条课程的课程名称”，进入课程详情
3. 记录详情页中课程信息
   1. 课程名称
   2. 课程类型
4. 与此前记录的课程信息比较，期望值为：true
5. 点击编辑
6. 输入栏中加入salt
7. 点击“保存”
8. 记录详情页中课程信息
9. 点击 “返回”
10. 记录当前第一条课程信息
11. 与此前记录的课程信息比较，期望值为：true



### 班级功能模块测试

#### 班级详情 & 班级编辑测试步骤

1. 复用登录模块（管理员）
2. 点击 “班级管理“
3. 记录第一条班级信息
   1. 班级名称
   2. 班级ID
   3. 人数
4. 调用接口 `[GET] /claszes/search`， 并保存返回值
5. 与之前记录的第一条班级信息比较，期望值为：true
6. 点击第一条班级的班级名称，进入班级详情
7. 记录班级详情中现实的信息
8. 与此前列表中的班级信息比较，期望值为：true
9. 检测编辑按钮是否可触发
10. 如果不可以触发则：点击 “返回”
11. 如果可以触发则：点击 “编辑”
12. 在 “班级名称” 加入 salt
13. 点击 “保存”
14. 记录当前显示班级详情
15. 点击 “返回”
16. 记录当前第一条课程信息
17. 与此前记录的课程详情对比，期望值为： true



#### 新建班级测试步骤

1. 复用登录模块（管理员）
2. 点击 “班级管理“
3. 记录当前班级总数
4. 点击 “新建班级”
5. 输入新建班级信息
   1. 班级ID
   2. 班级名称
   3. 人数
   4. 创建日期
   5. 删除日期
   6. 培训机构
6. 点击 “确定”
7. 检测是否出现成功新建班级提示
8. 记录当前班级总数
9. 与此前记录的班级总数比较，期望值为：+1



### 平台管理模块测试

#### 新建内容类型测试步骤：

1. 复用登录模块（管理员）
2. 点击 “平台管理“
3. 记录当前内容类型总数
4. 点击 “新建内容类型”
5. 填入对应信息
6. 点击 “确定”
7. 记录当前内容类型总数
8. 与之前记录的进行比较，期望值为：+1



#### 新建云入口测试步骤：

1. 复用登录模块（管理员）
2. 点击 “平台管理“
3. 点击 “平台设置”
4. 记录当前云入口总数
5. 点击 “新建云入口”
6. 填写名称
7. 点击 “类型”
8. 选择第一个选项
9. 填写对应信息
   1. server
   2. username
   3. password
10. 点击状态
11. 选择 “运行中”
12. 点击 “确定”
13. 检测是否提示 “成功新建云入口”
14. 记录当前云入口总数
15. 与之前记录的云入口总数比较，期望值为：+1



#### 删除云入口测试步骤：

1. 复用登录模块（管理员）
2. 点击 “平台管理“
3. 点击 “平台设置”
4. 记录当前云入口总数
5. 点击第一条信息中的 “删除”
6. 输入 “yes”
7. 点击 “确定”
8. 检测是否提示 “成功删除云入口”
9. 记录当前云入口总数
10. 与此前记录的云入口总数比较，期望值为：-1





### 用户详情操作模块

#### 个人信息展示&修改模块

1. 复用登录模块（管理员）
2. 点击头像
3. 点击 “个人信息”
4. 记录当前用户信息
   1. 用户名
   2. 真实姓名
   3. 邮箱
   4. 公司
   5. 描述
5. 根据用户名，向 API 发送请求，使用接口：`[GET]  /users/get-info/{username}/`
6. 记录 response.data 中用户信息
   1. 用户名
   2. 真实姓名
   3. 邮箱
   4. 公司
   5. 描述
7. 与之前记录的用户信息进行比较，期望值为： true
8. 点击 “编辑”
9. 在“描述”中加入 salt
10. 保存当前用户信息
11. 点击 “保存”
12. 记录当前用户信息
13. 与点击 “保存” 前的用户信息进行比较，期望值为：true



#### 修改密码模块：

1. 进入项目首页 
2. 在用户名&密码栏中输入对应字符——config文件中设置好的输入信息
3. `click` 点击登录按钮
4. 检测是否跳转到到指定路径
5. 是否弹出登录成功提示
6. 点击 “头像”
7. 点击 “修改密码”
8. 输入对应信息
9. 点击 “保存”
10. 点击 “头像” 
11. 点击 “注销”
12. 按照修改后的信息登录
13. `click` 点击登录按钮
14. 点击 “头像”
15. 点击 “修改密码”
16. 输入对应信息
17. 点击保存
18. 检验是否弹出密码修改成功提示



### 学生用户操作模块

1. 复用登录模块（学生）
2. 点击标签 “TEST02”
3. 点击 “一键创建”
4. 检测是否出现正在创建提示栏
5. 点击 “点击进入”
6. 检测 `url` 中是否包括：`/jupyter`



aaaaa_1

aaa872841462





政治：肖四、肖八

- 400加政治

英语： 黄皮书、红宝书、唐迟阅读、刘晓燕作文新题型、句句真研

数学：汤家凤（基础）、张宇李永乐（强化）

数据结构：王道数据结构、天勤数据结构







```bash
# enter folder
cd /opt/conda/lib/python3.7/site-packages/notebook/static/base/images/

# remove logo and icons
rm -f logo.png favion.ico favicon-terminal.ico

# download images
wget https://shaiictestblob01.blob.core.chinacloudapi.cn/share-all/logo.png
wget https://shaiictestblob01.blob.core.chinacloudapi.cn/share-all/icon.png -O favion.ico
wget https://shaiictestblob01.blob.core.chinacloudapi.cn/share-all/icon.png -O favicon-terminal.ico
```















