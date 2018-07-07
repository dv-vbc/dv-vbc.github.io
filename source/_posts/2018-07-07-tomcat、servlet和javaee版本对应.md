---
layout: "post"
title: "Tomcat、Servlet和JavaEE版本对应"
date: "2018-07-07 15:48"
tags:
    - tomcat
    - servlet
---

## Servlet和JSP规范版本对应关系：

Servlet规范版本 | JSP版本        | JSF版本        | JAVA EE版本
----------------|----------------|----------------|------------
Servlet2.3      | JSP1.2、JSP1.1 |                | J2EE1.3
Servlet2.4      | JSP2.0         | JSF1.1         | J2EE1.4
Servlet2.5      | JSP2.1         | JSF1.2、JSF2.0 | Java EE5
Servlet3.0      | JSP2.2         |                | Java EE6
## Tomcat所对应的Servlet/JSP规范和JDK版本：

Servlet Spec | JSP Spec | EL Spec | WebSocket Spec | JASPIC Spec | Apache Tomcat Version | Latest Released Version | Supported Java Versions
-------------|----------|---------|----------------|-------------|-----------------------|-------------------------|----------------------------------------
4.0          | 2.3      | 3.0     | 1.1            | 1.1         | 9.0.x                 | 9.0.8                   | 8 and later
3.1          | 2.3      | 3.0     | 1.1            | 1.1         | 8.5.x                 | 8.5.31                  | 7 and later
3.1          | 2.3      | 3.0     | 1.1            | N/A         | 8.0.x (superseded)    | 8.0.51 (superseded)     | 7 and later
3.0          | 2.2      | 2.2     | 1.1            | N/A         | 7.0.x                 | 7.0.86                  | 6 and later (7 and later for WebSocket)
2.5          | 2.1      | 2.1     | N/A            | N/A         | 6.0.x (archived)      | 6.0.53 (archived)       | 5 and later
2.4          | 2.0      | N/A     | N/A            | N/A         | 5.5.x (archived)      | 5.5.36 (archived)       | 1.4 and later
2.3          | 1.2      | N/A     | N/A            | N/A         | 4.1.x (archived)      | 4.1.40 (archived)       | 1.3 and later
2.2          | 1.1      | N/A     | N/A            | N/A         | 3.3.x (archived)      | 3.3.2 (archived)        | 1.1 and later

Apache官方对各版本的解释：[http://tomcat.apache.org/whichversion.html](http://tomcat.apache.org/whichversion.html)。
