# Login page

## Instructions

>Un lycéen a créé une application où seulement un seul utilisateur peut se connecter, et il a implémenté une vérification "maison".
>
>Curieux, vous avez récupéré l'application, pour voir si vous pouvez vous connecter !

## Decompiling

We are given an apk, the first thing to do is decompile it, with [jadx](https://github.com/skylot/jadx) for example

While discovering the classes, we found one that looks like this

```java
public class LoginDataSource {
    public Result <LoggedInUser> login(String paramString) {
        try {
            LoggedInUser loggedInUser = 
                    new LoggedInUser(UUID.randomUUID().toString(), "Admin");
            return (Result <LoggedInUser>)(o(paramString).equals("3476614A6465357265566552743347")
                    ? new Result.Success < LoggedInUser > (loggedInUser)
                    : new Result.Error(new Exception("blep")));
        } catch (Exception exception) {
            return new Result.Error(new IOException("Error logging in", exception));
        }
    }

    public void logout() {}

    public String o(String paramString) {
        String str = "";
        for (int i = paramString.length(); i > 0; i--) {
            String str1 = String.format("%H", Character.valueOf(paramString.charAt(i - 1)));
            StringBuilder stringBuilder = new StringBuilder();
            stringBuilder.append(str);
            stringBuilder.append(str1);
            str = stringBuilder.toString();
        }
        return str;
    }
}
```

We can see that we compare a string submited by the user with this one `"3476614A6465357265566552743347"`.
We also see that the user's string goes through a `o()` function, which basically turn it into hexa.

So we go on our [favorite conversion tool](https://www.dcode.fr/ascii-code), we paste in the string, and we go this string : "4vaJde5reVeRt3G"

It's not obvious but if you [reverse it](https://codebeautify.org/reverse-string), you got something like this : G3tReVer5edJav4, which is the flag