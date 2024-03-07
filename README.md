# Slimbook Notifications

Slimbook notifications are based on *rss* Specification. We only use some parts of it and we add some spicies on others. This is how they work:  

Notifications are *xml* *rss* files divided by language, using this pattern: 

```
slb-rss-LOCALE.xml
```

At this moment, we are only looking for either english or spanish locale, that means slb-rss-en.xml and slb-rss-es.xml.  

Inside each *rss* feed, a **rss** section following of a **channel** section **MUST** exist.  

```
<?xml version="1.0" encoding="UTF-8" ?>
<rss version="2.0">
	<channel>
  		<title>Slimbook RSS Feed</title>
  		<link>https://www.slimbook.es</link>
  		<description>Slimbook english RSS feed</description>
	</channel>
</rss>
```

Notifications are wrapped inside **item** sections. Each item **MUST** contain a **title** and a **description** subsection. Items **MAY** include other subsections specified by *rss* standard but only **link** and **category** are useful here.  

Link section provides an hyperlink alongside with mandatory description field. When notifications are received and shown on desktop, users can click on them and a web browser will show up.  

Category sections are used to filter notifications. Categories **ARE NOT** mandatory, so an item **MAY** have zero or multiple **category** sections. However, at the moment only the following categories have sense:  

* **firmware**: This category tags notifications as firmware related, so a special icon is shown, helping users identifying them.  
* **model:[FILTER]**: This category are used by clients to filter notifications shown, using *dmi* product string as target. FILTER is a *glob* syntax pattern. Product strings are converted to lowcase, so there is no need to worry about case.  

## Examples

Simple Notification:

```
<item>
    <title>Welcome</title>
    <description>Thanks for buying a new Slimbook laptop</description>
</item>
```

Notification with a link:
```
<item>
    <title>Forum improvements</title>
    <link>https://slimbook.com/forum/foro-de-la-comunidad-1/question/aviso-slimbook-mejoras-en-el-foro-questions-1176</link>
    <description>We have improved forum. Go checkt it out!</description>
</item>
```

Using category filters:
```
<item>
    <title>Firmware update</title>
    <description>There is a firmware update for ProX</description>
    <category>model:prox*</category>
    <category>firmware</category>
</item>
```

## Available models

The list of available model *dmi* names can be obtained from here:  
<https://github.com/Slimbook-Team/libslimbook/blob/main/src/slimbook.cpp>


# Client side notes

Notifications are provided by slimbook-service package. This software runs two daemons, a system one, running as root, and a session one, running with user credentials. In fact, if another user session is opened, another user daemon will spawn.  

Notifications are fetched from user side, along with its settings. This is how notifications are currently fetched:  

* A fetch is scheduled to 5 seconds since application starts
* Since first startup fetch, another one is scheduled each 6 hours
* A fetch process is canceled if cached *xml* is less than an hour old

## Cache

Notifications *xml* is stored at home user under following path:  

```
~/.cache/slimbook-service/sb-rss.xml
```

The file name is not locale aware, so no matter if sb-rss-en.xml or sb-rss-es.xml is fetched, it will be stored as sb-rss.xml. You may want to delete this file in order to force service to fetch another one.  

Once xml is parsed, a *MD5* hash is computed for each item. Hash is then look up in local hash cache, if it does not exists there, that notification item is considered as new, and then service will pop up a message to user.  

Hash cache is stored here:  

```
~/.cache/slimbook-service/feeds.dat
```

The file is only a line separated list of md5 in ascii format.



