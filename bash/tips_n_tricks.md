Bash quick tips
------


## Remove newline character from private/public keys

    $ cat privatekey | sed ':a;N;$!ba;s/\n/\\n/g'
  
[source](http://stackoverflow.com/questions/38702567/escape-new-lines-from-cat-in-sed-expression)
> may not work in mac.



## SSL pinning: generate public key from ssl cert

    $ openssl x509 -modulus -noout < wildcard.abc.com_cai.pem | sed s/Modulus=/0x/

## Send mail from server

Install mailutils with configuration "internet site"

for non-interactive installation:

        debconf-set-selections <<< "postfix postfix/mailname string `hostname -f` "
        debconf-set-selections <<< "postfix postfix/main_mailer_type string 'Internet Site'"
        apt-get install postfix mailutils -y
	 apt-get install mailutils -y

Try sending mail now

	echo "message body goes here" | mailx -a 'From:backup-scheduler@no-reply.com' -s "OK: Logs uploaded" sonukr666@xxx.com;

To check maill queue
    
         mailq
 
To forcefully send emails outta mail queue

        sendmail -q -v
