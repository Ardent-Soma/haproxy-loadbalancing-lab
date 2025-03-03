# haproxy-loadbalancing-lab
 ## deploying of two web servers
 First, Nginx was installed in two different VMs for the web servers with the command "sudo apt install -y nginx"
 Ngnix was enabled with the command "sudo apt systemctl enable nginx" and it was started with the command "sudo apt systemctl start nginx"
 Then the Ip address of the VMs were gotten with the command "ip a".

 ## Installation and configuration of haproxy on a different VMs
 On a different VM. haproxy was installed with the command " sudo apt install -y haproxy"
The configuration file was opened with the command "sudo nano /etc/haproxy/haproxy.cfg"
Then the implementation of the loadbalancers with different algorithm.
 
 ### Using roundrobin algorithm
frontend http_front
    bind *:80
    default_backend web_servers

backend web_servers
    balance roundrobin
    server web1 192.168.56.10:80 check
    server web2 192.168.56.11:80 check

    The IP addresses are the IP address of the different web servers, 80 is web port number and check is to do a health check on the servers.
Haproxy was restarted to apply changes with the command "sudo apt systemctl restart haproxy"

### Using leastconn
backend web_servers
    balance leastconn
    server web1 192.168.56.10:80 check
    server web2 192.168.56.11:80 check

This is the command used for leastconn
Haproxy was restarted to apply changes with the command "sudo apt systemctl restart haproxy"

### Using IP Hash
backend web_servers
    balance source
    server web1 192.168.56.10:80 check
    server web2 192.168.56.11:80 check

This is the command used for IP Hash
Haproxy was restarted to apply changes with the command "sudo apt systemctl restart haproxy"

### Configure health checks
Intense health checks was configured with this command 
frontend http_front
    bind *:80
    default_backend web_servers

backend web_servers
      balance source
    server web1 192.168.56.101:80 check inter 2000 rise 2 fall 3
    server web2 192.168.56.102:80 check inter 2000 rise 2 fall 3

check enables health checks to ensure the server is up.
inter 2000 means the health check interval is 2000ms (2 seconds).
rise 2 means the server will be marked as healthy after 2 consecutive successful health checks.
fall 3 means the server will be marked as down after 3 consecutive failed health checks.





