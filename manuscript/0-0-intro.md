{class: part}
# Introduction

Azure Active Directory B2C provides business-to-customer (or consumer) identity as a service. While this is the term Microsoft uses to advertise the offer, it's just a bit of short sight. Of course, you can use Azure Active Directory B2C (from now on just 'B2C') for all kind of highly customizable Identity and Access Management solutions, connect business customers or other enterprises. 

You can let your customers use their preferred social, enterprise, or local account identities to get single sign-on access to your applications and APIs. There is, however, no need to offer any connections to social networks.

The deep integration in Azure is an advantage, but even this is not a requirement B2C can run mostly on its own. Using an Azure subscription with all the capabilities of Azure adds several features that are very helpful in real-life projects.

## What the Manufacturer Says

Microsoft has some strong language advertising for their solution. To fulfill all the promises, they created a huge framework of libraries, online tools, and command line tools. That's harrd to learn and complex. 

You might think of an alternative solution, such a Keycloak or not that complex competitors such as Okta. While it's always worth a look for others, I strongly recommend considering a closer look into the security realm at all. To run an app or piece of software seems simple nowadays. Get your homework done, use Continues Integration and Continue Delivery, use a nice project management tool such as Scrum or Kanban, use Kubernetes to scale and Grafana to monitor...it's all just a few clicks away. 

When it comes to security, things are slightly different. While a minor glitch in your app setup leads to some frustrated users and your precious website might be offline for a moment or two, a security breach can lead to harsh consequences. It's not just annoying. It can kill you entire business. You're seen yourself as a business software developer? True, most of us are. But you're a security expert, specialized in IAM solutions? Probably not. There is a reason for the vast range of security offers from companies doiung just this. 

Microsoft is not just doing this, but big enough to handle the security stuff professionally. So let's hear what they say.

> Azure AD B2C is a customer identity access management (CIAM) solution capable of supporting millions of users and billions of authentications per day. It takes care of the scaling and safety of the authentication platform, monitoring, and automatically handling threats like denial-of-service, password spray, or brute force attacks.

Don't mix it with Active Directory (on Windows Server) or Azure Active Directory (on Azure). Microsoft might be good at creating sophisticated security solutions, but it's hardly good at giving its services coherent names. All these similar named offers have remotely something to do with each other. They even provide some integration points. However,

* they do not depend on each other directly,
* they can live on its own,
* they are mostly technically different.

I'll give some insight in Azure Active Directory (AAD) in this book, because in an enterprise context its most likely you use it anyway and the integration points are important to know. In case it's all new to you, you can keep this part as a reference and come back once it's necessary. Some parts of B2C are management through the AAD portal, so you can't skip it entirely. Though, the AAD specific parts, especially the Windows AD integration can easily put out of scope.

### Target Audience

Any business or individual who wishes to authenticate end users to either web or mobile applications using a white-label authentication solution can use B2C. Apart from authentication, B2C service is used for authorization such as access to API resources by authenticated users. It is designed to be used by IT administrators and developers. The provided UI is not intended to be accessible by end users. However, and most important, B2C provides an explicit user interface for end users to sign in, sign up, reset password, use OTP (one time passwords) and many more dialogs to interact with the system.

B2C is a white-label authentication solution. You can customize the entire user experience with your brand so that it blends seamlessly with your web and mobile applications. The user interface can be static HTML, enriched with CSS and JavaScript, generated dynamically by an ASP.NET Core application (such as MVC, Razor pages, Blazor Server, and more) or hosted by any other web development environment. Even Ruby or NodeJS would to it.


## Scope of this Book

## Terms

## Pre-Authentication

In the chapter **Identity Protocols** I'm going to explain while it makes sense to have protocols such as OAuth2 and OIDC. The motivation to use these are based on the experience with elder authentication solutions. One reason to read this book is probably that you want to replace an existing solution, based on the traditional 'username-password' scheme.

Password-based authentication is around the software industry for more than half a century. So it can't be that wrong, can it? For an application, a user is just a collection of attributes that are relevant for the functionality the application provides. That's not limited to security attributes. You can assign almost everything to your users.

The Oxford Languages Dictionary gives us a hint that *authentication* means:

> The process or action of verifying the identity of a user or process.

That's a bit short, in my opinion. Let's add two additional approaches:

1. Attributes (properties) added to the user
2. It's not limited to "users" (a person), but can also apply to a service (a machine)

In the pre-authentication world the machine aspect was out of scope. To keep this in sight we nowadays replace the term "user" with the more generic term "identity". It's being used to identify something (it can be a service, machine, device, user, person, whatever...).

### Passwords and Beyond

Passwords are secrets strings that both parties (the one who provides restricted access and the one who wants to gain access to the restricted resource) agree on. They establish a contract, often silently, that both parties keep the secret what it is - a secret.

While there are many ways to define such "strings" and add additional measures such as PIN (personal identity numbers) or use more sophisticated approaches (certificates, temporary codes), the underlying mechanisms are still the same. So, what's wrong with these?

One fundamental issue here is that the application shall keep track of all the data. It must store the identity information, the attributes, the secrets (mostly, of course, just a hash of it), and all the measures required to handle the user flow. The client sends in some identity information, such as an email, username, nickname, or similar to being recognized by the application and additionally some secret.

While this seems very clever, you probably overlook the fact, that the identity is a constant. It's unlikely to change. It's tightly coupled to the user. Furthermore, it's mostly public. A security system that takes two parts of information (username + password) is in reality just taking one piece as secure (password). Hence, the whole system's security wall is just one string. No fallback. No second line.

Why it's so broadly used, then? It's easy to implement (business people read: cheap). Likewise, it is supported by a vast range of libraries and frameworks. Users are trained to use it and don't need much support. That's the reason to stay with it. Discussions about weak passwords are as old as applications exist. Password can be cracked, get lost, or the whole user identity solution leaks out. We need to memorize complex passwords. Users use keyword generators, password stores, secret stores, and even more software. Password copies are getting spread.

All the information is being sent over public networks. The time the username-password mechanism was invented we had mostly just local networks. To break in one needed physical access to get those data. Nowadays, it's just a service in the cloud, some server in rack far away, or just virtual machine none of us knows where exactly.

With the advent of online applications most complex desktop apps are being replaces by an armada of smaller apps accessible through the browser. Hence, addressing multiple apps is common for most users. Is any application keeps its own stack of identity store modules, the secret get spread wider and wider. 

In security terms we speak of "security footprint". The bigger the worth. With any app a user starts working with the footprint gets bigger. Even if they stop using the app the security information will most likely remain there. The footprint is bigger than it looks for the user. That's a huge risk indeed. That's the reason for different protocols such as OAuth.

> **OAuth and Passwords** You may ask why this is a difference. Even with OAuth driven websites you still put in a password. However, the approach addresses the security footprint. And passwords are just one option among many others. Refer to the chapter **Identity Protocols** to understand more.

Shall you replace the username-password store immediately and always? Not at all. Internal solutions, smaller non-public websites, intranet applications rarely used may not benefit so much from an integrated authentication framework. Having isolated identity information in an internal network can shrink the security footprint and help keeping a software lean. However, if you get an app that's public, accessible on the Internet, with external or private users, username-password is definitely not recommended anymore.