# Introduction

## What is it?

ngrok acts as a globally distributed [reverse proxy](https://www.cloudflare.com/learning/cdn/glossary/reverse-proxy/) that protects, secures and accelerates your applications and network services. Once ngrok sits in front of your app or environment you can add different modules that will provide features and other behaviors ([HTTP Modules](https://ngrok.com/docs/http/#modules)). ngrok is environment independent because it can deliver traffic to services running anywhere with no changes to your environments networking.  

ngrok operates a global network of servers where it will accept traffic to your upstream services from clients on the internet. The URLs that it receives traffic on are your endpoints and as mentioned earlier, you can run different modules on those endpoints to up your features. Unlike traditional reverse proxies, ngrok does not transmit traffic to your upstream services by forwarding to an IP address. Instead, you run [ngrok agent](https://ngrok.com/docs/agent/) alongside your service, which will connect to ngrok's global service via secure, outbound persistent TLS connections. When traffic is received on your endpoints at ngrok edge, it is transmitted to the agent via those TLS connections and finally from the agent to your upstream service. We can make sure that the only way to access the application is via their endpoint URL achieving network isolation.  

This unique architecture confers several important benefits over traditional reverse proxies. For starters, it means you can run your services anywhere if the agent is set up properly. Secondly, it abstracts networking configurations because DNS, IPs, certificates and ports are pushed to the ngrok edge. It can enhance security and protect you from attacks since its able to enforce authentication. 

If you want to know more about ngrok and clearly convey what advantages is provides you can learn more in the following link: [why ngrok](https://ngrok.com/docs/why-ngrok/)

![High level overview of how ngrok operates.](images/High%20level%20overview%20of%20how%20ngrok%20operates.png)

## QuickStart

In order to install ngrok agent on your local environment so you can make your local applications available through a global URL endpoint you need to follow certain steps. [Sign up](https://ngrok.com/signup) for an ngrok account to be able to the online dashboards and GUI. [Download and install](https://ngrok.com/download) the ngrok agent for your local environment. Lastly add you auth-token to the default ngrok.yml [configuration file](https://ngrok.com/docs/agent/config/) allowing you to connect to your ngrok account. 

Once that everything is correctly set up, assuming you have a working web application listening on "http://localhost:8080" or a different URL putt your application online using the following command: 

- ``ngrok http http://localhost:8080``

The forwarding URL is the URL that will be available to anyone on the internet. But if you want to use the same URL each time you use ngrok instead of a ephemeral domain you can create a static domain. But remember we just used the ngrok agent in its CLI form but it can be used in other types of flavors. 

![Steps to set up ngrok.](images/Steps%20to%20set%20up%20ngrok.png)

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

![Edge middleware for incoming traffic.](images/Edge%20middleware%20for%20incoming%20traffic.png)

## Additional features

As every other cloud service ngrok includes identity and access management features. It has concepts such as principals, bot users (service accounts), role base access control (RBAC), account domain controls,... .  

One of the key aspects of ngrok [IAM](https://ngrok.com/docs/iam/) are bot users where other platforms sometimes call this concepts service accounts. Bot users are intended for automated systems that programmatically interact with your ngrok accounts either by starting ngrok agents or making requests to the API. As every other user they must authenticate themselves with ngrok.  

ngrok provides [observability](https://ngrok.com/docs/obs/) that covers from changes to your account (audit events) to when traffic transits through your endpoints (traffic events). Users, in turn may subscribe, filter and publish these events and do with them a number of different things. 

![Observability pillars of ngrok.](images/Observability%20pillars%20of%20ngrok.png)

# In Detph

## Help Command

![ngrok help command.](images/ngrok%20help%20command.png)

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

### Basic Authentication

This is a module that enforces HTTP Basic Authentication in front of your endpoints. Only requests with a valid username and passwords will be sent to your upstream service, all others will be rejected with a 401 Unauthorized response. You can find more information [here](https://ngrok.com/docs/http/basic-auth/?cty=agent-cli#overview). 

Its a rather straightforward authentication that has several things to take into account. For starters, you either use basic auth on the ngrok agent or on you upstream service. It is not recommended to have this type of basic authentication in both, because it requires both credentials to be the same otherwise a failure will occur in any of the steps. If basic auth is required in both steps you can use the Request Headers module to rewrite the authorization header to the value expected by the upstream service. 

**Credentials**:= A list of username/password pairs specified as 'username:password'. 

**Errors**:= HTTP Status 401 is returned in case a request is disallowed. 

**Events**:= When this module is enabled, it populates the following fields in the http_request_complete.v0 event. [basic_auth.decision ; basic_auth.username] 

**Note**: *Basic Auth is not a supported HTTPS Edge module yet*. 

![Basic authentication flow.](images/Basic%20authentication%20flow.png)

### IP Restrictions

The IP restriction module allows or denies traffic based on the source IP of the connections that was initiated to your ngrok endpoint.  You define rules which allow or deny connections from IPv4 or IPv6 CIDR blocks.  A connection will be allowed as long as is present in a whitelist and its not present in any denylist. If this was the case, the denylist takes precedence over everything else. All policies are grouped together for evaluation and they will be evaluated against the layer 4 source IP of a connection and not any HTTP headers. 

The agent will be configured with the allow CIDRs and the deny CIDRs on the other hand at the edge we can configure a set of IP policies that will be used to check if a source IP is allowed access. IP restrictions is part of HTTPS Edge module. For more information refer to [Edge Route IP Restriction Module](https://ngrok.com/docs/api/resources/edge-route-ip-restriction-module/). 

Errors are returned to HTTP endpoints if connections aren't allowed: [ERR_NGROK_3205](https://ngrok.com/docs/errors/err_ngrok_3205/) / 403. Lastly when IP restriction module is enforced the ip_policy.decision field for [http_request_complete.v0](https://ngrok.com/docs/obs/reference/#http-request-complete) event is filled with whether the request was allowed or denied. 

![IP restrictions with Edge.](images/IP%20Restriction%20with%20Edge.png)

### Mutual TLS

Mutual TLS module performs mTLS authentication when the ngrok edge terminates TLS on incoming connection to your HTTP endpoint where the client must present a valid x509 cert and key signed by a specified by one of the trusted CA provided to the module. Both the client and the server authenticate each other using X.509 certificates.  

Note: x509 certificates contain a basic constrain attribute called cA which defines whether or not the certificate may be used as a CA. For more information about basic constraints refer to the [official documentation](https://datatracker.ietf.org/doc/html/rfc5280#section-4.2.1.9). 

As it was the case with IP Restrictions module, we can either configure the ngrok agent with a PEM encoded certificate authorities list file or we can rely on edge configuration through 'certificate_authorities.id' . This last approach is more a resource oriented approach using the ngrok cloud TLS certificate authorities dashboard.

This module does add headers to the HTTP request sent to your upstream service with details regarding the client certificate presented at the mTLS handshake.

![mutual TLS](images/Mutual%20TLS.png)

### OAuth

The OAuth module enforces a browser-based OAuth flow in front of your HTTP endpoints to an identity provider like Google. In this case, we are going to be the resource owner and ngrok will be the client.  

Usually you, as resource owner, are presented with a Consent form based on the Scopes requested by the Client as well as a Redirect URI and a Response Type. Instead, we are already granting this at the Google OAuth consent screen at Google Cloud Platform and creating credentials to the Client ID and Client Secret. For application that use the OAuth protocol to call Google APIs, you can use the Client ID to generate an access token. This way your application will have the ability to securely communicate and access Google APIs on behalf of the client that made the connection if your application needs access to Google Calendar for example. 

OAuth is an authorization flow and in order to provide authentication we need to add a layer of Open ID Connect. The OIDC layer on top of OAuth that provides more information to the client about the identity of the user.

![Simplified OAuth flow.](images/Simplified%20OAuth%20flow.png)

Digging into the mechanics, the ngrok edge is acting as a lightweight policy enforcement point ensuring that any traffic attempting to enter the tunnel has an access token created by the corresponding OAuth provider.

### Request and Response Headers

Both of these modules add an remove headers from either HTTP request or response. In the case of requests headers, they will be added before sending them to the upstream service although changes made to request will not be visible to other modules just your upstream service. On the other hand, if headers are added or removed when sending them to the client we are dealing with HTTP responses. 

You can add and remove headers in a wide variety of configurations but bear in mind while populating some variables with values they only will be enabled if the required module is active otherwise they will be empty. On the following list you may find a comprehensive [list](https://ngrok.com/docs/http/request-headers/#backend) of possible variables. 

![Request and Response headers acting stage.](images/Request%20and%20Response%20headers%20acting%20stage.png)

### SAML

SAML stands for Security Assertion Markup Language,  which is a XML-based-open standard for transferring identity data between two parties: an identity provider and a service provider. In our case the identity provider would be the IdP that ngrok uses and the service provider would be ngrok agent. 

Upstream servers behind a SAML protected endpoint can safely assume that requests are from users authorized by the SAML IdP to access the protected resource. SAML greatly relies on single sing-on scheme which allows to log in with a single ID to any of several related, yet independent software systems. When planning for SAML there are three variants to take care of: IdP, End User and the Service Provider. Next we can find a high level overview of the SAML workflow. 

![SAML simplified workflow](images/SAML%20simplified%20workflow.png)


### Traffic Policy

We can configure inbound and outbound rules to our endpoints so we can influence and control traffic to and from our upstream service. Traffic will be filtered using Common Expression Language expressions and each policy must be evaluated true in order for the rule to take effect. In the following link we can find a list of some of the variables we are able to make expressions for. 

Policy rules will be composed of expressions that filter the traffic on which they are applicable and actions that should take effect. Policy rules with their attached expressions and actions will be evaluated in order and the actions they may be able to take after an expression is evaluated are the following ones. 

One of the most interesting actions is JSON Web Token validation at cloud edge before routing the traffic to upstream service. You can configure the agent and the configuration will be pushed to the global network which secures traffic before it hits our network. 

As another filtering policy there is the user agent filter, it is not exactly part of traffic policy but its a similar kind of work. Where the edge will read the User-Agent header from an HTTP request to identify the user agent responsible for making a given HTTP request and thus enables you to block accessing your web application. Its another rule that will be evaluated at this module.

![Traffic Policy structure](images/Traffic%20Policy%20structure.png)
