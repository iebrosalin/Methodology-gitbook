# OGNL

#### WHAT IS OGNL INJECTION (OGNL)?

Object-Graph Navigation Language is an open-source Expression Language (EL) for Java objects. Specifically, OGNL enables the evaluation of EL expressions in Apache Struts, which is the commonly used development framework for Java-based web applications in enterprise environments. The most critical vulnerabilities on the list of Apache Struts CVEs relate to OGNL expression injection attacks, which enable evaluation of invalidated expressions against the value stack, allowing an attacker to modify system variables or execute arbitrary code.

OGNL is infamous for related vulnerabilities found in the Struts 2 framework that relies on it. Because OGNL has the ability to create or change executable code, it is also capable of introducing critical security flaws to any framework that uses it. For example, it is possible for the attacker to inject OGNL expressions (which can execute arbitrary malicious Java code), when an OGNL expression injection vulnerability is present.

Protections against this CVE include [security solutions](https://www.contrastsecurity.com/security-influencers/cve-2018-11776?hsLang=en) that can detect the presence of vulnerable Struts2 components in software so that attacks can be prevented.



#### Tutorial

[https://pentest-tools.com/blog/exploiting-ognl-injection-in-apache-struts/](https://pentest-tools.com/blog/exploiting-ognl-injection-in-apache-struts/)

[\
](https://cta-redirect.hubspot.com/cta/redirect/203759/6c26e819-8c13-4572-9570-36ced2498e33)
