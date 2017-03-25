Tcpdump is cool but its output is very noise and you try to control the noise using various flags. 

Tcpflow gives you only the data without noise. 


Installation
------

$ apt-get install tcpflow


Usage
-------

You can restrict the "sniffing" to a specific server or a specific protocol

Eg: to get traffic for your mail.mac.com server through your Ethernet connection, use:

      sudo tcpflow -c -i ens3 host mail.mac.com

Similarely, this command will get all data going through POP over the Ethernet connection:

      sudo tcpflow -c -i ens3 tcp port 110

You can also redirect the result to a file:

      sudo tcpflow -c -i ens3 host mail.mac.com > ~/Desktop/tcpflow-result.txt
