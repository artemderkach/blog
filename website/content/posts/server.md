+++
author = "Artem Derkach"
title = "Expose App To The Internet"
date = "2021-11-07"
description = "Notes on how to deploy application"
tags = [
    "deployment",
]
+++

Notes on how to automate, deliver and make you app available for everyone.

<!--more-->
<br>
This notes talks about one of many possible ways to solve the problem.<br>
What is the problem anyway?<br>
Say, you have your application X which you want be exposed to the Internet,
for some reason, and now you have to figure how to do it in a sane way.
<br><br>

Topics covered:
- [Cloud Hosting]({{< ref "#cloud-hosting" >}})
- [Domain Name]({{< ref "#domain-name" >}})
- [SSL Certificate]({{< ref "#ssl-certificate" >}})
- [Version Control]({{< ref "#version-control" >}})
- [Application Wrapper]({{< ref "#application-wrapper" >}})
- [Application Delivery Tool]({{< ref "#docker" >}})
- [CI CD]({{< ref "#ci-cd" >}})
<br><br>

## Cloud Hosting
Our choice will be [digitalocean](https://www.digitalocean.com/), you can get minimal server fo $5 a month.<br>
Server will be exposed with public IP address.<br>
To access server:
- generate keys with `$ ssh-keygen`
- add public key to the server (cloud hosting should provide instruction on this)

To verify that server is available on given IP:
- enter the server using ssh `$ ssh root@<static_ip>`, were `<static_ip>` is given by cloud hosting.
- `$ nc -l 80` - to listen on port `80`, it should be exposed by default.
- try to use `<static_ip>` in your browser OR do `$ curl <static_ip>`. Result can be observed in cmd with `nc` listening
<br><br>


## Domain Name
For service to be exposed not via IP but domain name (e.g. google.com), you need to buy this name from domain registrar.
Some cloud hosts can also provide domain name registration.

After getting domain name, you will have to map `<static_ip>` to your `<domain_name>`. This can be achieved in 2 ways:
1. add record in your domain registrar that will map `<static_ip>` to `<domain_name>`.
2. add record in your domain registrar that will redirect to your cloud hosting name services. Then do (1) for cloud hosting to map from `<static_id>` to `<domain_name>`.
Here is the [example](https://www.digitalocean.com/community/tutorials/how-to-point-to-digitalocean-nameservers-from-common-domain-registrars) for digitalocean

Cloud host usually less painful for managing DNS records, so if your DNS config is more than few records - do (2) step.
<br><br>


## SSL Certificate
You can do all things listed here without add certificates, but security is our everything.
For generating certificates, use [letsencrypt](https://letsencrypt.org/getting-started/) as free Certificate Authority.
<br><br>


## Version Control
Kind of the obvious point, but let it also sit here, just for the sake of notes integrity.<br>
Some of available choices to store code for app:
- [github](https://github.com)
- [gitlab](https://gitlab.com)
- [bitbucket](https://bitbucket.com)
- self-hosted solution (for example [gitea](https://gitea.io)). If you're reading this notes, there is great possibility it's not an option.

Every listed solution is free, so, pick what you like (use `github` if you don't have strong opinion).
There is of course possibility to live without version control, but this is essential part to create
automation with ci/cd.
<br><br>


## Application Wrapper
We will wrap our application in `Docker` because:
- hide local dependencies behind `Docker`.
- use `Docker` registry as your application artifact storage.
- server will require only `Docker` installation to run the app.
- it is well established practice building your orchestration around containers.
<br><br>
  

## CI CD
Every major version control host provider got it's ci/cd tooling, but we are going to talk about self-hosted solution,
specifically [drone](https://www.drone.io/).

Giving that our choice is self-hosted, we need to set up server first.

Steps to set up [drone](https://www.drone.io/):
- install it on server and expose using public IP
- don't forget to install runners for executing builds
- by default drone will be exposed for everyone, add whitelist for allowed users  
- add API keys to cloud provider either in environment variables or environment file
