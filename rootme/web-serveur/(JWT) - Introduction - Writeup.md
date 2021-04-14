# JWT- Introduction - Writeup

> To validate the challenge, connect as admin.

**Steps**
- [x] Start the challenge
- [x] Searching a way to log as admin

**How to log as admin ?**


First of all, remember that we are talking about JWT tokens.
Consider understanding with documentation their constitution and usage.

By checking if there are cookies stored, we see this token named jwt
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VybmFtZSI6Imd1ZXN0In0.OnuZnYMdetcg7AWGV6WURn8CFSfas6AQej4V9M13nsk

Using [**jwt.io**](https://jwt.io/) we can understand the signification of this JWT token.

Here is the result : 
![Result](https://imgur.com/ZGHDZ8Z)

** The Trick **

The method to solve this challenge is simply to set the algorythm of encryption at none, to change the data in the payload, and finally to remove the signature.


