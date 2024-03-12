# Introduction

## What is it?

ngrok acts as a globally distributed [reverse proxy](https://www.cloudflare.com/learning/cdn/glossary/reverse-proxy/) that protects, secures and accelerates your applications and network services. Once ngrok sits in front of your app or environment you can add different modules that will provide features and other behaviors ([HTTP Modules](https://ngrok.com/docs/http/#modules)). ngrok is environment independent because it can deliver traffic to services running anywhere with no changes to your environments networking.  

ngrok operates a global network of servers where it will accept traffic to your upstream services from clients on the internet. The URLs that it receives traffic on are your endpoints and as mentioned earlier, you can run different modules on those endpoints to up your features. Unlike traditional reverse proxies, ngrok does not transmit traffic to your upstream services by forwarding to an IP address. Instead, you run [ngrok agent](https://ngrok.com/docs/agent/) alongside your service, which will connect to ngrok's global service via secure, outbound persistent TLS connections. When traffic is received on your endpoints at ngrok edge, it is transmitted to the agent via those TLS connections and finally from the agent to your upstream service. We can make sure that the only way to access the application is via their endpoint URL achieving network isolation.  

This unique architecture confers several important benefits over traditional reverse proxies. For starters, it means you can run your services anywhere if the agent is set up properly. Secondly, it abstracts networking configurations because DNS, IPs, certificates and ports are pushed to the ngrok edge. It can enhance security and protect you from attacks since its able to enforce authentication. 

If you want to know more about ngrok and clearly convey what advantages is provides you can learn more in the following link: [why ngrok](https://ngrok.com/docs/why-ngrok/)

