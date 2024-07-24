# 大数据ETL处理工具Kettle


## 1、Kettle介绍

* 介绍: kettle是一个ETL工具
* ETL: 抽取、转换、加载
* Kettle作用：不同的数据源按照统一格式输出
* Kettle提供可视化界面



## 2、Kettle安装

* 安装jdk ： 安装java环境
* 修改系统环境变量 : JAVA_HOME, Path
* 安装Kettle ：
  * 解压 ->
    *  windows : spoon.bat  
    *  Linux/macos : spoon.sh




## 3、Kettle使用入门

* 文本文件输入 -> excel输出

![image-20211221113551084](https://github.com/user-attachments/assets/e9601366-26d9-4d7f-ad84-b8753aec0aed)

* 构建kettle数据流图
  * 输入
    * 找到文本文件输入
  * 输出
    * Excel输出
  * 连接
    * shift 键 + 鼠标拖动

![image-20210829104127228](https://github.com/user-attachments/assets/118d25a2-30e7-4112-961e-da35f82115d6)

* 配置组件

  * 文本文件输入组件配置

  ![image-20211221112929221](https://github.com/user-attachments/assets/70ac2095-954e-413e-8724-296bddb155f8)

  ![image-20211221112958466](https://github.com/user-attachments/assets/d6079ab3-169c-42ab-ac28-68881099dc67)

  ![image-20211221113042721](https://github.com/user-attachments/assets/900e1c6c-78ab-4568-a40c-e1767eeb77cb)

  

  * Excel输出组件配置

  ![image-20211221113131133](https://github.com/user-attachments/assets/902f8080-c6d1-468f-a6dc-453022143bc3)

  ![image-20211221113237860](https://github.com/user-attachments/assets/55dd6b4d-b9e0-4b70-aeca-9f6611be6168)



* 保证并启动执行

  ![image-20211221113319514](https://github.com/user-attachments/assets/c269793b-7086-4048-8519-b6b33092e69e)

  ![image-20211221113342860](https://github.com/user-attachments/assets/bfc1e9a3-edbf-477f-943c-e8e16e256a54)

  ![image-20211221113434935](https://github.com/user-attachments/assets/9abfc2f0-a32c-4b10-a4a4-44bc77f08dff)



## 4、Kettle实现excel到mysql表

* 创建mysql数据库kettle_demo

  * ```mysql
    drop database if exists kettle_demo;
    create database if not exists kettle_demo charset=utf8;
    ```

* kettle中的lib目录中加入mysql的jar文件（已经完成）

* 构建流程图

  * ![image-20211221151242839](https://github.com/user-attachments/assets/f2d490be-a097-488a-b7cd-4f12ed8741bc)

* 配置输入和输出组件

  * 配置输入

    * ![image-20211221151329097](https://github.com/user-attachments/assets/cfdcc7fc-b47d-4f51-9575-4373c80b78a0)
    * ![image-20211221151350861](https://github.com/user-attachments/assets/20248c3d-ef1e-487f-ac57-47115b5c0f63)
    * ![image-20211221151415928](https://github.com/user-attachments/assets/a299c79a-cbb6-447e-8c33-90a1bd0d59d1)
    * ![image-20211221151533458](https://github.com/user-attachments/assets/e00ea5da-a509-4343-9e70-24bb9414132f)

  * 配置输出（表输出）

    * ![image-20211221151753406](https://github.com/user-attachments/assets/6ce55a1f-8550-4132-83d6-41ac0c5f7a8c)

    * ![image-20211221150151989](https://github.com/user-attachments/assets/7ac47524-d9de-4597-8041-128d8bd99a2e)

      ![image-20211221150503572](https://github.com/user-attachments/assets/4df5e2af-1dbc-46a7-a3ba-b8ddd20f7414)

      ![image-20211221150640452](https://github.com/user-attachments/assets/dc9b7319-7b06-478a-9aa7-c588dd19740a)

      ![image-20211221150607797](https://github.com/user-attachments/assets/6fe0fc62-b2f6-466e-92ea-343d4f1b94a5)

      ![image-20211221150738013](https://github.com/user-attachments/assets/e4a3c7b7-7a4d-46a8-ba05-a6b01e863ffa)

      ![image-20211221150806522](https://github.com/user-attachments/assets/9179e6c9-9d6d-4b70-b21b-fbc5c120e465)

      ​	

* 保存并启动执行

  	* 执行报错

​	![image-20211221151153237](https://github.com/user-attachments/assets/9f2078a8-4a6b-4268-aaf5-a56d3f67490b)

![image-20211221151115873](https://github.com/user-attachments/assets/c1023875-5c35-45a5-818a-3bc46df56af9)

​	![image-20211221151221241](https://github.com/user-attachments/assets/1bd49d29-a4c3-4f97-b42f-b304302c514c)



## 5、Kettle实现mysql表到mysql表2的转换

* 共享数据连接

![image-20210829150402819](https://github.com/user-attachments/assets/72962762-d8b9-40f9-956b-1cbe20dc0278)

* 创建数据流图

* 配置表输入组件和表输出组件

* ![image-20211221171705263](https://github.com/user-attachments/assets/a8f2938e-8c77-449d-9f37-4b2d958e4a58)

* ![image-20211221171750992](https://github.com/user-attachments/assets/06ef4964-4195-4e5d-b495-db6c4de5f2cb)

* 保存并执行

  

## 6、Kettle的插入更新组件

Kettle软件在进行转换与装载数据过程中，有两种装载形式：① 全量装载 ② 增量装载

全量装载

数据表A：①②③④⑤⑥

数据表B：①②③④⑤⑥①②③④⑤⑥①②③④⑤⑥



增量装载

数据表A：①②③⑦⑤⑥

数据表B：①②③⑦⑤⑥



* 插入更新和表到表区别
  * t_user_to_t_user1 : 只进行全量追加.
  * 插入更新 : 对比关键字段，更新所有数据. (不会删除)
* 创建数据流图

![image-20211221164643007](https://github.com/user-attachments/assets/13d6710d-793e-4077-9ee8-c2a86d9f69d8)

* 配置表输入组件和插入更新组件

![image-20211221164736296](https://github.com/user-attachments/assets/c2bf6faa-eb21-4969-8f11-24839c8b25a1)

![image-20210829161312501](https://github.com/user-attachments/assets/069d8c6b-66ce-4350-bba4-12bf99b1b8eb)

* 保存并执行



* **扩展:裁剪表**

![image-20210829161453537](https://github.com/user-attachments/assets/74e7894a-80a6-4904-977a-a0cce182efdf)

裁剪表到底是干什么？

简单来说：就是使用`truncate 数据表`在执行任务前，先把数据表清空，然后再把Excel中的文件内容导入到MySQL的里面

## 7、Kettle的switch/case组件

* 创建数据流图
* ![image-20210830143329486](https://github.com/user-attachments/assets/245e4814-950c-4962-8260-a2643295fcf2)

![image-20210830093148198](https://github.com/user-attachments/assets/624eafc6-0366-4fe1-aa7d-b7412490291b)



* 配置表输入组件、switch组件、excel输出组件

![image-20210830093244949](https://github.com/user-attachments/assets/cdd38a9a-4b9c-4d7f-bc4e-846fce36749c)

* 保存并执行



## 8、Kettle的SQL脚本

* 创建数据流图

![image-20210830095131269](https://github.com/user-attachments/assets/b145f902-6d4a-4a59-92b3-ba26e109d3e1)

* 配置SQL组件

![image-20210830095157167](https://github.com/user-attachments/assets/5fc0daa5-1150-442a-9821-7657e228930b)

* 保存并执行



## 9、设置转换命名参数

* 配置转换参数

![image-20210830100426235](https://github.com/user-attachments/assets/29c114a2-d2b6-4757-b753-c1c1222ec805)

* 配置sql组件: 转换参数使用格式 **${参数名}**

![image-20210830100257706](https://github.com/user-attachments/assets/58e2aa5a-661f-4459-b973-1d216ad3c275)

* 保存并执行(执行时需要设置参数值)

![image-20210830100643037](https://github.com/user-attachments/assets/13e57d02-4995-4457-b584-11f354a0f7db)



## 10、Kettle的作业

* 创建数据流图

* 配置转换

![image-20210830105804774](https://github.com/user-attachments/assets/869138d7-dc8e-4694-a28f-2c61f6ff4cc5)

* 配置start

![image-20210830110325891](https://github.com/user-attachments/assets/5e893b2f-abbf-462e-a67f-ad92a630aa73)

* 保存并启动

![image-20210830111044152](https://github.com/user-attachments/assets/a06f4465-f698-48cf-8c9a-edc8aea51df7)



