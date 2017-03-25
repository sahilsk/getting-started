# Getting Started With Tcpdump



### CDN, HEADERS and IPs


    sudo tcpdump -A -vvvv -s 9999 -i eth0 port 80 > /tmp/sample

Here is part of packet:

    X-Geo: US
    X-Real-IP: 54.83.132.159
    X-Timer: S1398278909.637000322,VS0,VS3
    X-Varnish: 58497648
    X-Varnish: 506864951
    X-Varnish: 2932897992
    X-Varnish: 1531049198
    X-Forwarded-For: 54.83.132.159, 199.27.72.25, 50.19.19.94
    X-Forwarded-Port: 80
    X-Forwarded-Proto: http
    
    
    
### Check for traffic on port 80


    sudo tcpdump -A -vvvv -s 9999 -i eth0 src port 80 or dest port 80
  

### record all TCP traffic to port 80

  
    sudo tcpdump -i wlan0  \
               src port 80 or dst port 80 \
               -w port-80-recording.pcap
               
### Filtering packets

    stuff being sent to port 80:
        dst port 80
    you can use booleans!
        src port 80 or dst port 80
    here's how to filter on IP:
        ip src 66.66.66.66
        

### Filter packets with size greater than given size

    tcpdump -n -i eth0 -A -x dst port 443 and greater 100

filter expression has to come after all the command-line flag arguments.


References
---------


- http://jvns.ca/blog/2016/03/16/tcpdump-is-amazing/
- https://danielmiessler.com/study/tcpdump/
  
