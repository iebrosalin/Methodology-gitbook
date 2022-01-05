# Import

### There are many ways to import a python file, all with their pros and cons.

Don't just hastily pick the first import strategy that works for you or else you'll have to rewrite the codebase later on when you find it doesn't meet your needs.

I'll start out explaining the easiest example #1, then I'll move toward the most professional and robust example #7

**Example 1, Import a python module with python interpreter:**

1.  Put this in /home/el/foo/fox.py:

    ```python
    def what_does_the_fox_say():
      print("vixens cry")
    ```
2.  Get into the python interpreter:

    ```python
    el@apollo:/home/el/foo$ python
    Python 2.7.3 (default, Sep 26 2013, 20:03:06) 
    >>> import fox
    >>> fox.what_does_the_fox_say()
    vixens cry
    >>> 
    ```

    You imported fox through the python interpreter, invoked the python function `what_does_the_fox_say()` from within fox.py.

**Example 2, Use `execfile` or (**[**`exec` in Python 3**](https://stackoverflow.com/q/6357361/55075)**) in a script to execute the other python file in place:**

1.  Put this in /home/el/foo2/mylib.py:

    ```python
    def moobar():
      print("hi")
    ```
2.  Put this in /home/el/foo2/main.py:

    ```python
    execfile("/home/el/foo2/mylib.py")
    moobar()
    ```
3.  run the file:

    ```python
    el@apollo:/home/el/foo$ python main.py
    hi
    ```

    The function moobar was imported from mylib.py and made available in main.py

**Example 3, Use from ... import ... functionality:**

1.  Put this in /home/el/foo3/chekov.py:

    ```python
    def question():
      print "where are the nuclear wessels?"
    ```
2.  Put this in /home/el/foo3/main.py:

    ```python
    from chekov import question
    question()
    ```
3.  Run it like this:

    ```python
    el@apollo:/home/el/foo3$ python main.py 
    where are the nuclear wessels?
    ```

    If you defined other functions in chekov.py, they would not be available unless you `import *`

**Example 4, Import riaa.py if it's in a different file location from where it is imported**

1.  Put this in /home/el/foo4/stuff/riaa.py:

    ```python
    def watchout():
      print "computers are transforming into a noose and a yoke for humans"
    ```
2.  Put this in /home/el/foo4/main.py:

    ```python
    import sys 
    import os
    sys.path.append(os.path.abspath("/home/el/foo4/stuff"))
    from riaa import *
    watchout()
    ```
3.  Run it:

    ```python
    el@apollo:/home/el/foo4$ python main.py 
    computers are transforming into a noose and a yoke for humans
    ```

    That imports everything in the foreign file from a different directory.

**Example 5, use `os.system("python yourfile.py")`**

```python
import os
os.system("python yourfile.py")
```

**Example 6, import your file via piggybacking the python startuphook:**

**Update:** This example used to work for both python2 and 3, but now only works for python2. python3 got rid of this user startuphook feature set because it was abused by low-skill python library writers, using it to impolitely inject their code into the global namespace, before all user-defined programs. If you want this to work for python3, you'll have to get more creative. If I tell you how to do it, python developers will disable that feature set as well, so you're on your own.

See: [https://docs.python.org/2/library/user.html](https://docs.python.org/2/library/user.html)

Put this code into your home directory in `~/.pythonrc.py`

```python
class secretclass:
    def secretmessage(cls, myarg):
        return myarg + " is if.. up in the sky, the sky"
    secretmessage = classmethod( secretmessage )

    def skycake(cls):
        return "cookie and sky pie people can't go up and "
    skycake = classmethod( skycake )
```

Put this code into your main.py (can be anywhere):

```python
import user
msg = "The only way skycake tates good" 
msg = user.secretclass.secretmessage(msg)
msg += user.secretclass.skycake()
print(msg + " have the sky pie! SKYCAKE!")
```

Run it, you should get this:

```python
$ python main.py
The only way skycake tates good is if.. up in the sky, 
the skycookie and sky pie people can't go up and  have the sky pie! 
SKYCAKE!
```

If you get an error here: `ModuleNotFoundError: No module named 'user'` then it means you're using python3, startuphooks are disabled there by default.

Credit for this jist goes to: [https://github.com/docwhat/homedir-examples/blob/master/python-commandline/.pythonrc.py](https://github.com/docwhat/homedir-examples/blob/master/python-commandline/.pythonrc.py) Send along your up-boats.

**Example 7, Most Robust: Import files in python with the bare import command:**

1. Make a new directory `/home/el/foo5/`
2. Make a new directory `/home/el/foo5/herp`
3.  Make an empty file named `__init__.py` under herp:

    ```python
    el@apollo:/home/el/foo5/herp$ touch __init__.py
    el@apollo:/home/el/foo5/herp$ ls
    __init__.py
    ```
4. Make a new directory /home/el/foo5/herp/derp
5.  Under derp, make another `__init__.py` file:

    ```python
    el@apollo:/home/el/foo5/herp/derp$ touch __init__.py
    el@apollo:/home/el/foo5/herp/derp$ ls
    __init__.py
    ```
6.  Under /home/el/foo5/herp/derp make a new file called `yolo.py` Put this in there:

    ```python
    def skycake():
      print "SkyCake evolves to stay just beyond the cognitive reach of " +
      "the bulk of men. SKYCAKE!!"
    ```
7.  The moment of truth, Make the new file `/home/el/foo5/main.py`, put this in there;

    ```python
    from herp.derp.yolo import skycake
    skycake()
    ```
8.  Run it:

    ```python
    el@apollo:/home/el/foo5$ python main.py
    SkyCake evolves to stay just beyond the cognitive reach of the bulk 
    of men. SKYCAKE!!
    ```

    The empty `__init__.py` file communicates to the python interpreter that the developer intends this directory to be an importable package.

If you want to see my post on how to include ALL .py files under a directory see here: [https://stackoverflow.com/a/20753073/445131](https://stackoverflow.com/a/20753073/445131)
