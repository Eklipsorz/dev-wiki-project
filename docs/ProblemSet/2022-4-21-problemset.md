---
sidebar_position: 3
---

# 2022-4-21-坑/問題集


1. TLS/SSL 為何為stateful protocol？那麼https就是stateful protocol?

> TLS/SSL is stateful. The web server and the client (browser) cache the session including the cryptographic keys to improve performance and do not perform key exchange for every request.
> HTTP 1 is not stateful. HTTP/2 however defines many stateful components, but the "application layer" still remains stateless.
> TL;DR: The transport pipe (TLS) is stateful, original HTTP is not.
> Additional note: Cookies and other stateful mechanisms are later additions defined in separate RFC's. They are not part of the original HTTP/1.0 specification, although other stateful mechanisms like caching and HTTP auth are defined HTTP 1.1 RFC and RFC 2617. HTTP 1 is said to be stateless although in practice we use standardized stateful mechanisms. HTTP/2 defines stateful components in its standard and is therefore stateful. A particular HTTP/2 application can use a subset of HTTP/2 features to maintain statelessness.
> Theory aside, in practice you use HTTP statefully in your everyday life.


[Is HTTPS Stateful or Stateless?](https://stackoverflow.com/questions/11067500/is-https-stateful-or-stateless)

2. 理解JWT：
  - 理解它要解決什麼？為何而出
  - 如何解決
  - 為何叫做JWT
  - 如何進行持續時間的管理


[您是否在为用户会话正确使用JWT？](https://zhuanlan.zhihu.com/p/142352954)