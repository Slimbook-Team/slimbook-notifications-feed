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


