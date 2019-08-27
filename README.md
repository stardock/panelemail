# panelemail
SSPanel notification email (PUSH)



前言

有时候需要给用户发送一些通知，节点维护啊，节点调整啊，促销活动啊等等。我看好多飞机场都是直接拿每日用量的邮件模板配合网站最新公告，然后来一条php命令实现，太没有技术含量了有木有。 本文教你如何优雅的实现 🙂
步骤

进入网站根目录，进入/app/Command目录，编辑XCat.php文件，在第43行前面插入

            case("sendUserMessage"):
                return DailyMail::sendUserMessage();
            case("sendAdminMessage"):
                return DailyMail::sendAdminMessage();

插入后保存

https://sspanel3.org/wp-content/uploads/2018/05/send-all-user-notices-1.png

然后编辑DailyMail.php文件

https://sspanel3.org/wp-content/uploads/2018/05/send-all-user-notices-2.png
在第63行插入

	public static function sendUserMessage() #sendUserMessage,给用户发送通知
    {
		$users = User::all();
		
        foreach ($users as $user) {
            if ($user->email_Notification==0) {
                #管理员账户email_Notification设为1,用户设为0
				echo "Sending:".$user->id."... Done.\r\n";
                $subject = "全体通知"; #邮件标题
                $to = $user->email;

                try {
                    Mail::send($to, $subject, 'news/Message.tpl', [
                        "user" => $user
                    ], [
                    ]);
                } catch (Exception $e) {
                    echo $e->getMessage();
                }
            }
        }
    }
	
    public static function sendAdminMessage() #sendAdminMessage,给管理员发送通知,用于测试发送效果
    {
		$users = User::all();
		
        foreach ($users as $user) {
            if ($user->email_Notification==1) {
                #管理员账户email_Notification设为1,用户设为0
				echo "Sending:".$user->id."... Done.\r\n";
                $subject = "全体通知"; #邮件标题
                $to = $user->email;

                try {
                    Mail::send($to, $subject, 'news/Message.tpl', [
                        "user" => $user
                    ], [
                    ]);
                } catch (Exception $e) {
                    echo $e->getMessage();
                }
            }
        }
    }

然后进入网站根目录的/resources/email/news文件夹，创建Message.tpl文件，文件内容如下

<!DOCTYPE html>
<html>

<head>
  <meta name="viewport" content="width=device-width"/>
  <meta http-equiv="Content-Type" content="text/html; charset=UTF-8"/>
</head>

<body>
<p>您好，{$user->user_name}。</p>
</body>

</html>

然后就是给user表新增一个字段了，登陆phpmyadmin，选中sspanel，user

https://sspanel3.org/wp-content/uploads/2018/05/send-all-user-notices-3.png

https://sspanel3.org/wp-content/uploads/2018/05/send-all-user-notices-4.png

名字填email_Notification，默认选定义，然后填入0，最后点击保存

https://sspanel3.org/wp-content/uploads/2018/05/send-all-user-notices-5.png

https://sspanel3.org/wp-content/uploads/2018/05/send-all-user-notices-6.png

然后把自己的email_Notification值改成1。用搜索，email一栏填写你的管理员账户邮箱，然后点击执行

https://sspanel3.org/wp-content/uploads/2018/05/send-all-user-notices-7.png

选中，编辑

https://sspanel3.org/wp-content/uploads/2018/05/send-all-user-notices-8.png

改成1，点执行

https://sspanel3.org/wp-content/uploads/2018/05/send-all-user-notices-9.png

至此，基本功能已经实现了，ssh连接到服务器，进入网站根目录，php xcat sendAdminMessage一下看看效果

https://sspanel3.org/wp-content/uploads/2018/05/send-all-user-notices-10.png

https://sspanel3.org/wp-content/uploads/2018/05/send-all-user-notices-12.png
使用

php xcat sendUserMessage ，给用户发送通知
php xcat sendAdminMessage ，给管理员发送通知，用于测试发送效果


Reference: https://sspanel3.org/send-all-user-notices/  

