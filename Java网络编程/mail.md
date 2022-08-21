邮箱发送代码

```
import java.security.Security;
import java.util.Properties;
import javax.mail.Authenticator;
import javax.mail.Message;
import javax.mail.PasswordAuthentication;
import javax.mail.Session;
import javax.mail.Transport;
import javax.mail.internet.InternetAddress;
import javax.mail.internet.MimeMessage;

public class MailClient {
  public static void main(String[] args) {
    try {
      final String SSL_FACTORY = "javax.net.ssl.SSLSocketFactory";

      //配置邮箱信息
      Properties props = System.getProperties();
      //邮件服务器
      props.setProperty("mail.smtp.host", "smtp.qq.com");
      props.setProperty("mail.smtp.socketFactory.class", SSL_FACTORY);
      props.setProperty("mail.smtp.socketFactory.fallback", "false");
      //邮件服务器端口
      props.setProperty("mail.smtp.port", "465");
      props.setProperty("mail.smtp.socketFactory.port", "465");
      //鉴权信息
      props.setProperty("mail.smtp.auth", "true");
      //建立邮件会话
      Session session = Session.getDefaultInstance(props, new Authenticator() {
        //身份认证
        protected PasswordAuthentication getPasswordAuthentication() {
          //1.账户 授权码
          return new PasswordAuthentication("xxxxxxx@qq.com", "xxxx");
        }
      });
      //建立邮件对象
      MimeMessage message = new MimeMessage(session);
      //设置邮件的发件人
      message.setFrom(new InternetAddress("xxxxxxx@qq.com"));
      //2.设置邮件的收件人，可多个，用逗号隔开
      message.setRecipients(Message.RecipientType.TO, "xxxxxxx@qq.com");
      //设置邮件的主题
      message.setSubject("通过javamail发出！！！");
      //文本部分
      message.setContent("文本邮件测试", "text/html;charset=UTF-8");
      message.saveChanges();
      //发送邮件
      Transport.send(message);
    } catch (Exception e) {
      e.printStackTrace();
    }
  }
}
```

固定用法，只需修改邮箱配置
