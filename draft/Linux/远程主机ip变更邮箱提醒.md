```pip install PyEmail```
```easy_install email```
```python
import socket
import smtplib
from email.header import Header
from email.mime.multipart import MIMEMultipart
from email.mime.text import MIMEText
from time import asctime

def send_an_email(email_content): # email_content是一个字符串
    mail_host = "smtp.sina.com" # 这个去邮箱找，这里用的sina邮箱
    mail_user = '17839906091@sina.cn'
    mail_auth_code = '9f8f7149d10d0b1f'
    mail_sender = mail_user # 用mail_user 作为发送人
    mail_receivers = ['2524370217@qq.com']
    
    message = MIMEMultipart()
    message['From'] = Header(mail_sender)  # 寄件人
    message['Subject'] = Header("主题名字")
    message.attach(MIMEText(asctime(), 'plain', 'utf-8'))
    # message.attach(MIMEText('<html><h1>你好<h1></html>', 'html', 'utf-8'))
    message.attach(MIMEText(email_content, 'plain', 'utf-8'))
    print("message is {}".format(message.as_string())) # debug用
    
    smtpObj = smtplib.SMTP(mail_host)
    # smtpObj.set_debuglevel(1) # 同样是debug用的
    smtpObj.login(mail_user, mail_auth_code) # 登陆
    smtpObj.sendmail(mail_sender, mail_receivers, message.as_string()) # 真正发送邮件就是这里

def get_temp_ip(current_ip):
    # 失败时可尝试删除/var/tmp/ip.json
    temp_ip_json_path = "/var/tmp/ip.json"

    if not os.path.exists(temp_ip_json_path):
        print("No {}, dump it.".format(temp_ip_json_path))
        with open(temp_ip_json_path, 'w') as jo:
            json.dump(current_ip, jo)
            return True, current_ip
    else:
        with open(temp_ip_json_path, 'r') as jo:
            origin_ip = json.load(jo)
        if origin_ip == current_ip:
            print("Current ip {} do not change, no need to send".format(current_ip))
            return False, current_ip
        else:
            print("The ip updated from {} to {}, update it.".format(origin_ip, current_ip))
            os.remove(temp_ip_json_path)
            with open(temp_ip_json_path, 'w') as jo:
                json.dump(current_ip, jo)
                return True, current_ip


def get_ip():
    hostname = socket.gethostname()
    addr_infos = socket.getaddrinfo(hostname, None)
    ips = set([addr_info[-1][0] for addr_info in addr_infos])
    global_ips = [ip for ip in ips if ip.startswith("24")]
    whether_to_send, send_ip = get_temp_ip(global_ips)
    send_ip = json.dumps(send_ip)
    return whether_to_send, send_ip

if __name__ == "__main__":
    whether_to_send, global_ips = get_ip()
    if whether_to_send:
        send_an_email(global_ips)
    else:
        print("wait and no send")
```


还要给脚本添加上可执行权限：

```sudo chmod +x send_ip_to_mailbox.py```
使用到linux的排程工具crond将任务排期自动执行，这里配置的是每隔1分钟执行一次ipv6地址检查。
