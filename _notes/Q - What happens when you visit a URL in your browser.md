---
Title: What happens when you visit a URL in your browser
---

# What happens when you visit a URL in your browser

This question looks to see if you understand the fundamentals of the internet and how DNS works.

A URL is just a human-readable string to remember a website. Behind the scenes, clients/servers actually connect via an IP address, and there are a series of steps taken to translate a domain name to an IP.

The series of steps that are followed until the IP address is discovered:

1.  The user enters a URL.
2.  The client checks to see if they have the IP address cached locally that matches this URL.
3.  Then a resolving nameserver is queried for the IP address. This is often your internet service provider (ISP). If it does not have the IP cached, it will then go through the process to find it for you.
4.  The resolving server queries the root server which responds with the address of the Top Level Domain (TLD) server (such as .com or .dev), which stores information for all of its domains.
5.  The TLD server is queried and responds with the nameserver that stores the IP address of the domain we want (the domain's nameserver).
6.  The domain's nameserver (called the authoritative nameserver) is queried for the IP address of the domain we're requesting.
7.  The IP address is returned to the resolving nameserver which passes it to the client and can communicate with the server through the IP address matching the domain.

![](https://skilled.dev/images/dns.png)
# ---

Tags: #coding #interview

Topics: [[Interview Qs]]

