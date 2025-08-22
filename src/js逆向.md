# js逆向：
登陆处忘记密码旁有一个账号激活功能，有任意输入账号：

<img width="1171" height="563" alt="image" src="https://github.com/user-attachments/assets/e63576a6-51da-4946-92fa-75dd504020ef" />


抓取数据包、关注到参数路径和useruid参数、发现是经过加密的(当然不是简单的base64)：

![image-20250708170800938](https://cdn.jsdelivr.net/gh/maybeyjb/blue-team/img/202507081718911.png)

开始js逆向：这里搜2个参数，我是搜的useruid ，然后分析尝试断点、点击下一步看是否能断下来：

后面发现搜findus......uid这个路径参数更容易找到断点：

![image-20250708171022893](https://cdn.jsdelivr.net/gh/maybeyjb/blue-team/img/202507081718793.png)

然后断下来、可以看到这里的a参数是传入的工号学号，进入hu()：

![image-20250708171235823](https://cdn.jsdelivr.net/gh/maybeyjb/blue-team/img/202507081718009.png)

简单的aes加密，iv和key都是硬编码：

![image-20250708171310252](https://cdn.jsdelivr.net/gh/maybeyjb/blue-team/img/202507081719592.png)

验证：

![image-20250708171337895](https://cdn.jsdelivr.net/gh/maybeyjb/blue-team/img/202507081719168.png)

然后就回到开始点击下一步后抓包：
<img width="1837" height="736" alt="image" src="https://github.com/user-attachments/assets/fb80488d-2361-4bfb-b91f-34abfbcff8a2" />

尝试将这里的code改为200，即可绕过验证：
<img width="1128" height="729" alt="image" src="https://github.com/user-attachments/assets/98a9ea3e-1e6c-4a57-a0d7-c88ed233f6ca" />
还有就是下一步时候，这里的学号需要改成你想重置的学号：
<img width="1152" height="723" alt="image" src="https://github.com/user-attachments/assets/9ac9db53-974b-4d96-a9e8-503260fc049d" />
因为是账号激活，所以绕过第一步就成功了，第二步是将你要重置的学号设定为自己手机号，和重新设置新密码。
