---
layout: post
title: "Safe Secure Suckless"
date: 2020-02-07 00:00:00 -0600
categories: [suckless, web, minimalism]
---
So you've finished another Luke Smith video and drank the UNIX kool-aid. 
Finished setting up your server with his instructed nginx setup all is well and working in the world.

But there's an issue here, nginx seems a bit bloated for our purposes. This site, 
for example, is static and simple. I use a little bit of client side JavaScript 
for MathJax but thats about it. So a rather complex server like Apache or Nginx 
seems unneeded, and we always strive for minimalism here.

Enter Suckless, and more importantly Quark. Quark is a simple webserver for static content 
that presently runs this site. Quark itself is easy enough to set up however there's a bit 
of a frustrating section at the bottom of its instructions page.

```
TLS-support
quark does not natively support TLS. A more suckless approach than to implement TLS into it is to use a TLS reverse proxy (e.g. tlstunnel, hitch or stunnel). It accepts encrypted TLS connections and forwards them as unencrypted requests to a server. In this case, one can run such a reverse proxy to listen on a public IP address and forward the requests to a local port or UNIX-domain socket.
```

With absolutely no instructions on how to do so. As someone who values privacy even in the mundane https even on this dumb blog is important to me.
The following will be those instructions, along with a basic Quark Tutorial.

```
$ quark -p 80 -h zacharyowen.xyz -d _site
```

at a very basic level this command will get your site up and running without tls. Quark is the 
command (obviously), the -p argument instructs Quark which port to listen on, since we're serving html
to a web browser its going out on good ole port 80. My site is zacharyowen.xyz, so we set that as our 
hostname with -h, and I like to keep the whole thing in a directory called ``_site`` and I set that 
with -d (this should be an absolute path).

Now, for this tutorial we'll be using stunnel. This is because tlstunnel has be deprecated, and 
I couldn't figure out how to get hitch to work. After installing stunnel4 from your package manager
you will be able to find stunnel's config file at `/etc/stunnel/stunnel.conf`, and the setup we'll
 be working with is as easy as:
```
[site]
accept = 443
connect = localhost:80
cert = /etc/stunnel/stunnel.pem

```
The site tag lets stunnel know the name of this connection. Since this will be an end point for an https request we will accept incoming connections the standard port, 443. And then we'll have stunnel forward our connection back into the server on port 80. And if you followed Luke's Video on setting up a site
you should already have certbot set up, just point cert at those.

This was my sticking point for a long while. The eventual cause and fix comes from that Quark is 
expecting to be talked to as if it is talking to a remote website. But stunnel is not talking to 
another server, it's talking to itself. It seems weird but Quark will only respond properly if the hostname matches the request. Potentially we could just connect to the site externally but its better to keep the traffic on the machine. We can do that by simply spinning up another Quark process with its server hostname as localhost like so:

```
$ quark -p 8000 -d _site
```

I like to set this process to listen on a seperate port than the first one. Not only is this good form, but it also will cause issues for people who don't read the articles explaination and just try to copy things.

There are still an issue or two to take care of here. Mainly ensuring that quark will run as a daemon and can be restarted by a supervisor like systemd, or even a good one like openrc.

We'll look at that next time.

