Bash quick tips
------


## Remove newline character from private/public keys

    $ cat privatekey | sed ':a;N;$!ba;s/\n/\\n/g'
  
[source](http://stackoverflow.com/questions/38702567/escape-new-lines-from-cat-in-sed-expression)
> may not work in mac.



## SSL pinning: generate public key from ssl cert

    $ openssl x509 -modulus -noout < wildcard.abc.com_cai.pem | sed s/Modulus=/0x/
