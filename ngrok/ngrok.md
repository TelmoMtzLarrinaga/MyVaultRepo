# Introduction

## What is it?

ngrok acts as a globally distributed [reverse proxy](https://www.cloudflare.com/learning/cdn/glossary/reverse-proxy/) that protects, secures and accelerates your applications and network services. Once ngrok sits in front of your app or environment you can add different modules that will provide features and other behaviors ([HTTP Modules](https://ngrok.com/docs/http/#modules)). ngrok is environment independent because it can deliver traffic to services running anywhere with no changes to your environments networking.  

ngrok operates a global network of servers where it will accept traffic to your upstream services from clients on the internet. The URLs that it receives traffic on are your endpoints and as mentioned earlier, you can run different modules on those endpoints to up your features. Unlike traditional reverse proxies, ngrok does not transmit traffic to your upstream services by forwarding to an IP address. Instead, you run [ngrok agent](https://ngrok.com/docs/agent/) alongside your service, which will connect to ngrok's global service via secure, outbound persistent TLS connections. When traffic is received on your endpoints at ngrok edge, it is transmitted to the agent via those TLS connections and finally from the agent to your upstream service. We can make sure that the only way to access the application is via their endpoint URL achieving network isolation.  

This unique architecture confers several important benefits over traditional reverse proxies. For starters, it means you can run your services anywhere if the agent is set up properly. Secondly, it abstracts networking configurations because DNS, IPs, certificates and ports are pushed to the ngrok edge. It can enhance security and protect you from attacks since its able to enforce authentication. 

If you want to know more about ngrok and clearly convey what advantages is provides you can learn more in the following link: [why ngrok](https://ngrok.com/docs/why-ngrok/)

![High level overview of how ngrok operates.](https://github.com/TelmoMtzLarrinaga/MyVaultRepo/blob/main/ngrok/images/High%20level%20overview%20of%20how%20ngrok%20operates.png)

## QuickStart

In order to install ngrok agent on your local environment so you can make your local applications available through a global URL endpoint you need to follow certain steps. [Sign up](https://ngrok.com/signup) for an ngrok account to be able to the online dashboards and GUI. [Download and install](https://ngrok.com/download) the ngrok agent for your local environment. Lastly add you auth-token to the default ngrok.yml [configuration file](https://ngrok.com/docs/agent/config/) allowing you to connect to your ngrok account. 

Once that everything is correctly set up, assuming you have a working web application listening on "http://localhost:8080" or a different URL putt your application online using the following command: 

- ``ngrok http http://localhost:8080``

The forwarding URL is the URL that will be available to anyone on the internet. But if you want to use the same URL each time you use ngrok instead of a ephemeral domain you can create a static domain. But remember we just used the ngrok agent in its CLI form but it can be used in other types of flavors. 

![Steps to set up ngrok.](https://github.com/TelmoMtzLarrinaga/MyVaultRepo/blob/main/ngrok/images/Steps%20to%20set%20up%20ngrok.png)

## Platform

**Network Edge**: The globally distributed infrastructure that ngrok operates. It accepts incoming traffic to your services' endpoint URLs, applies your module configurations and routes those connections to the appropriate connected ngrok agents. The globally distributed infrastructure is grouped at locations known as points of presence (POP) to enable fast, low latency traffic to your applications. 

**IP Addresses**: Do not rely on ngrok's IPs because they are dynamic and may change frequently or on DNS records past their time to live.  

**TLS Termination**: ngrok terminates TLS traffic at its edge for HTTPS endpoints. Traffic is then re-encrypted with TLS as it is transmitted to your upstream service via the agent. (Remember there are three types of endpoint types: HTTPS, TLS, TCP) 

**Security Measures**: ngrok helps protect your service by applying a layer of DDoS protection to all your endpoints at their network edge. You can additionally bump security on said endpoints by enforcing authentication modules, thus only authenticated traffic can reach your upstream service. Lastly, it has a [Circuit Breaker](https://ngrok.com/docs/http/circuit-breaker/) module which blocks al traffic when services become overloaded. 

**Domains**: You are able to create endpoints to listen to traffic on those domain names. Traffic can be received on all of global points of presence due to ngrok's Global Server Load Balancing without the need of running geographically redundant copies. There are a number of different types of domains that endpoints use, you can find them listed on [the official documentation](https://ngrok.com/docs/network-edge/domains-and-tcp-addresses/).  

**TCP Addresses**: TCP are network addresses which are formed by a host and a port that you can use to deliver applications via TCP endpoints. 

**GSLB**: [Global Server Load Balancing](https://ngrok.com/docs/network-edge/gslb/) improves the performance and resiliency of your applications by distributing traffic to the nearest healthy point of presence (measure by latency). It basically improves your ngrok endpoints and relieves you of many of the undifferentiated management, just start your new application service with their ngrok agent and you're done. 

**Edges**: enable management of your endpoints module configuration via ngrok dashboard or API instead of defining via an Agent or Agent SDK. One of the main advantages of edges is that when you make changes to your [Edge's](https://ngrok.com/docs/network-edge/edges/) configuration your app doesn't go offline and you don't have to restart the agent to pick up new command line flags or configuration files. Additionally, they have one or more routes which enables you to process traffic differently 

**TLS Certificates**: when traffic reaches an HTTPS endpoint or a TLS endpoint instead of passing the encrypted traffic directly to the destination server, ngrok decrypts it at its own servers for modification before re encrypting and forwarding it to the intended destination. For your endpoints it is recommended that you always choose to use [Automated TLS Certificates](https://ngrok.com/docs/network-edge/tls-certificates/#automated). 

![Edge middleware for incoming traffic.](https://github.com/TelmoMtzLarrinaga/MyVaultRepo/blob/main/ngrok/images/Edge%20middleware%20for%20incoming%20traffic.png)

## Additional features

As every other cloud service ngrok includes identity and access management features. It has concepts such as principals, bot users (service accounts), role base access control (RBAC), account domain controls,... .  

One of the key aspects of ngrok [IAM](https://ngrok.com/docs/iam/) are bot users where other platforms sometimes call this concepts service accounts. Bot users are intended for automated systems that programmatically interact with your ngrok accounts either by starting ngrok agents or making requests to the API. As every other user they must authenticate themselves with ngrok.  

ngrok provides [observability](https://ngrok.com/docs/obs/) that covers from changes to your account (audit events) to when traffic transits through your endpoints (traffic events). Users, in turn may subscribe, filter and publish these events and do with them a number of different things. 

![Observability pillars of ngrok.](https://github.com/TelmoMtzLarrinaga/MyVaultRepo/blob/main/ngrok/images/Observability%20pillars%20of%20ngrok.png)

# In Detph

## Help Command

![ngrok help command.](https://github.com/TelmoMtzLarrinaga/MyVaultRepo/blob/main/ngrok/images/ngrok%20help%20command.png)

## Security

We need to focus on the authentication capabilities of ngrok and how complex they may be. Below we can find an index of the different solutions ngrok provides. Be mindful for they might be configuration for the Edge as a hole or for a single Route, ... . 

- Basic Authentication. 

- IP Restriction. 

- Mutual TLS. 

- OAuth & OpenID Connect. 

- Request & Response headers. 

- SAML. 

- (TLS Termination). 

- Traffic Policy. 

Following there are some recommendation for ensuring you use ngrok securely. 

**Authtokens**. Each ngrok agent authenticates with an authtoken which will appear on its configuration file its recommended to have each authtoken be attached to a single agent. Authtoken ACL (Access Control Lists) restrict what actions can a ngrok agent connecting with a specific authtoken is allowed to make. 

**Encryption**. Usually TLS is terminated at the edge but if the ngrok agent and your local service is not running on the same machine as the ngrok agent, it is recommend that TLS is maintained between the ngrok agent to the upstream service. Additionally, for HTTPS endpoints ngrok takes care of TLS certificates automatically. 

**Dashboard**. For authenticating access to the dashboard, ngrok has features for role base access control which we are able to configure permissions for groups of users within your team. 