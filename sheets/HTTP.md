* What happens when we type www.google.com in our web browser

High Level Process:
1) Our computer makes a request, which through our modem, goes to our ISPs.
2) The request reaches google.com's servers
3) And they reply with a response.

Technical Details:
1) Need to  Translate the name of the site into an IP Address.
  * IP address identifies a server or a computer on the internet. 
  * IP address is of the form w.x.y.z
  * Each placeholder can have a number from 0-255.
  * It requires 32 bits to represent an IP address.
  * This is because representing a number between 0-255 we need 8 bits. (2<sup>8</sup> is 256)
  * Possible IP address = 2<sup>32</sup> ~ 4 billion
  * That is a lot of IP addresses, however since all of us use laptops, desktops, phones, ips we have more and more devices consuming IP addresses. And we are about to run out of IP addresses(ipv4).
  * IPv6 has begun to rolled out and v6 has 128 bit IP addresses and we will have 2<sup>128</sup> possible ip addresses.
  * These will be a:b:c:d:e:f:g:h (8 groups of 16 bits). 
  
 2) How this translation is happening? (from domain name to ip address)
  * How does our computers know what is the IP address of a certain domain name?
  * It has to do with **DNS** (Domain Name Lookup System).
  * It is an infrastructure on the internet that converts domain name and host names to ip addresses and vice versa.
  * It also does some other things like validating the ownership of domains etc.
  * Our ISPs know about these DNS Servers. They query them and get an IP address in return.

3) Now our computer puts togther a message,to send it accross the computer to google.com.
  * In its simplest form it looks like GET/HTTP/1.0
  * In reality there are some more headers,that get sent from browser to server.
  * But this message covers the most important aspect of a request.
  * Our computer will create a virtual envelope called a packet. Inside this packet is a message like this.
  * This packet also has the toAddress and returnAddress : the ip addresses of server and our computer respectively. And our computer sends this packet out in the internet.
