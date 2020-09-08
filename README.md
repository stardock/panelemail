# panelemail
SSPanel notification email (PUSH)



# 前言  

有时候需要给用户发送一些通知，节点维护啊，节点调整啊，促销活动啊等等。我看好多飞机场都是直接拿每日用量的邮件模板配合网站最新公告，然后来一条php命令实现，太没有技术含量了有木有。 本文教你如何优雅的实现 🙂  

# 步骤  

### 将`ExtMail.php`上传至 `src/Command` 覆盖源文件  
请将 '全体通知内容' 修改为你想要通知的内容

### 将`message2.tpl`上传至 `resources/email/ext`  

然后就是给user表新增一个字段了，登陆phpmyadmin，选中sspanel，user  
![](/pic/send-all-user-notices-3.png)  
![](/pic/send-all-user-notices-4.png)  
名字填email_Notification，默认选定义，然后填入0，最后点击保存  
![](/pic/send-all-user-notices-5.png)  
![](/pic/send-all-user-notices-6.png)  
然后把自己的email_Notification值改成1。用搜索，email一栏填写你的管理员账户邮箱，然后点击执行  
![](/pic/send-all-user-notices-7.png)  
选中，编辑  
![](/pic/send-all-user-notices-8.png)  
改成1，点执行  
![](/pic/send-all-user-notices-9.png)  
至此，基本功能已经实现了，ssh连接到服务器，进入网站根目录，php xcat ExtMail sendAdminMessage一下看看效果  
![](/pic/send-all-user-notices-10.png)  
![](/pic/send-all-user-notices-12.png)  

# 使用

`php xcat ExtMail sendUserMessage` ，给用户发送通知  
`php xcat ExtMail sendAdminMessage` ，给管理员发送通知，用于测试发送效果  


Reference: https://sspanel3.org/send-all-user-notices/  

