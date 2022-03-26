# Tom's Descend

> ### Tom's Descend
>
> #### 925 (21 Solves)
>
> Upon delivery a rather light package to a faceless man, Tom was in awe to discover his computer in the center of the pavilion. Upon getting closer to the monitor, There the company's new website is finally up and Tom is not only eager to find out the standings on the only old browser in the computer.
>
> [http://c1.lagncrash.com:9010](http://c1.lagncrash.com:9010)

![](<../../.gitbook/assets/image (20).png>)

We are greeted with a login page and a search feature. This might be indicative of an SQL Injection Challenge. I also noticed that my user agent is being shown below.

![](<../../.gitbook/assets/image (23).png>)

The first step to check if the website is vulnerable to SQL Injection is to inject quotes and to check for errors. The database used is SQLite3 using a query function.

Next, I injected the payload `' OR 1=1--` to ensure all teams are returned regardless of names.

![](<../../.gitbook/assets/image (21).png>)

We found the first portion of the flag.

Next, I tried to enumerate other tables in the database.

```
' UNION SELECT sql,2 FROM sqlite_schema-- 
```

![](<../../.gitbook/assets/image (28).png>)

Dumping the table schema from sqlite\_schema did not work hence I used another way to dump the schema.

```
' UNION SELECT sql,2  FROM sqlite_master--
```

![](<../../.gitbook/assets/image (3).png>)

I was tricked as the above portion of the flag is fake and does not work. Going back to the challenge description, it talked about using the old browser which brought me to my first observation of my user agent being shown on the page.

![](<../../.gitbook/assets/image (31).png>)

I intercepted and modified my user agent and indeed the user agent shown on the page was my modified version.

![](<../../.gitbook/assets/image (9).png>)

I changed my user agent to the oldest browser (Internet Explorer) and the end portion of the flag was shown.

