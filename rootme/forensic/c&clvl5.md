# Command and Control - Level 5

> Berthier, manifestement l’attaquant dispose des mots de passe des systèmes. Le programme malveillant semble être maintenu manuellement sur les machines. Le parc de la société ACME semblant à jour, c’est peut être les mots de passe qui sont faibles. John, l’administrateur des systèmes ne vous croit pas. Prouvez-lui.
> 
> Retrouvez le mot de passe de l’utilisateur.

Still using volatility, we dump the NTLM hash of users' passwords using `hashdump`

```bash
$ > volatility -f ch2.dmp --profile=Win7SP1x86_23418 hashdump
```
Output :
```bash
Administrator:500:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
John Doe:1000:aad3b435b51404eeaad3b435b51404ee:b9f917853e3dbf6e6831ecce60725930:::
```
Then, we can bruteforce `b9f917853e3dbf6e6831ecce60725930` using for example [crackstation](https://crackstation.net/)

Finally, we got the super secured password !