 
# Load Balancer Project

This project focuses on improving the web stack by adding redundancy and distributing traffic across multiple web servers using HAProxy.

The infrastructure consists of:

- **web-01**: First Nginx web server
- **web-02**: Second Nginx web server
- **lb-01**: HAProxy load balancer

The load balancer allows requests to be shared between both web servers, improving reliability and allowing the system to handle more traffic.

## Files

### 0-custom_http_response_header

Configures Nginx on web servers to add a custom HTTP response header.

Features:
- Installs and configures Nginx.
- Adds the `X-Served-By` HTTP header.
- The header value is automatically set to the server hostname.

Example response:


X-Served-By: student-web-01


This helps identify which backend server handled a request.

---

### 1-install_load_balancer

Installs and configures HAProxy on the load balancer server.

Features:
- Installs HAProxy.
- Enables HAProxy as a service.
- Configures HTTP traffic forwarding.
- Uses the `roundrobin` algorithm.
- Sends requests to both web servers.

Traffic flow:

          Client
             |
             v
         HAProxy
             |
    -----------------
    |               |
    v               v
web-01          web-02
 Nginx           Nginx

HAProxy distributes incoming requests alternately between the two backend servers.

---

## Testing

Check which server responds through the load balancer:

```bash
curl -sI http://LOAD_BALANCER_IP | grep X-Served-By

Repeated requests should alternate between:

X-Served-By: student-web-01

and

X-Served-By: student-web-02

confirming that HAProxy is correctly balancing traffic.

Requirements

All scripts:

Run on Ubuntu.
Start with #!/usr/bin/env bash.
Contain a description comment on the second line.
Are executable.
Automate configuration of a fresh server.
