---
Version: 1.0.0
Last Modified: 2016-08-23
---

# Kurio Publisher Guide

## Overview
In order to integrate your website articles into Kurio application, you must provide feed of content to be consumed by Kurio automated crawler.

In summary, the feed must contain these items:
- Article headline / title
- Article original URL, not a shortened link (eg. using bit.ly or goo.gl)
- Full article body, not just summary or incomplete paginated content
- At least one image to be previewed as article thumbnail, with minimum 300px dimension
- Minimum 15 articles and maximum 50 articles inside a feed

## Feed Format
The required feed can be provided as one of these formats:
- RSS 2.0
- JSON

Both formats must be accessible via public URL (eg. http://example.com/feed/rss.xml) as the crawler will consume them using HTTP request.

### RSS 2.0 Format
RSS 2.0 format uses XML as the syntax. This format is usually supported out of the box by CMS platform like Wordpress.

Note that if the you have already supported Facebook's Instant Article, that can be used too as feed for Kurio.

#### Example

```xml
<?xml version="1.0" encoding="UTF-8"?>
<rss version="2.0" xmlns:content="http://purl.org/rss/1.0/modules/content/" xmlns:media="http://search.yahoo.com/mrss/">
    <channel>
        <title>Your Website Name Here</title>
        <link>http://your-website-url.com/</link>
        <description>
            Your Website Description (optional)
        </description>

        <lastBuildDate>Tue, 23 Aug 2003 00:52:11 +0700</lastBuildDate>
        <updated>Tue, 23 Aug 2003 00:52:11 +0700</updated>

        <item>
            <title>Sample Article Title</title>
            <link>http://example.com/sample-article-url</link>
            <pubDate>Tue, 10 Jun 2003 04:00:00 +0700</pubDate>
            <media:thumbnail url="http://media.example.com/1992/01/09/squirrel.jpg" />
            <description><![CDATA[
            Summary of the article here. Up to 1 paragraph. No HTML allowed
           ]]></description>
            <content:encoded><![CDATA[ <p>Full content of the article <em>goes here</em>. HTML is welcomed here</p> <img src="http://media.example.com/1992/01/09/squirrel.jpg" width="1200" height="900"> <p>Images and videos should be placed within the article copy.</p> <p>More content goes here ...</p> ]]></content:encoded>
        </item>

        <item>
            <title>Another Article Title</title>
            <link>http://example.com/another-article-url</link>
            <pubDate>Tue, 09 Jun 2003 04:00:00 +0700</pubDate>
            <media:thumbnail url="http://media.example.com/1992/01/09/squirrel.jpg" />
            <description><![CDATA[
            Summary of the article here. Up to 1 paragraph. No HTML allowed
           ]]></description>
            <content:encoded><![CDATA[ <p>Full content of the article <em>goes here</em>. HTML is welcomed here</p> <img src="http://media.example.com/1992/01/09/squirrel.jpg" width="1200" height="900"> <p>Images and videos should be placed within the article copy.</p> <p>More content goes here ...</p> ]]></content:encoded>
        </item>
    </channel>
</rss>
```

### JSON Format
If you prefer to provide the contents using JSON instead, please use below format.

#### Example
```json
{
    "name": "Your Website Name",
    "link": "http://your-website-url.com/",
    "description": "Your Website Description (optional)",
    "last_updated": 1471946907,

    "items": [
        {
            "title": "Sample Article Title",
            "link": "http://example.com/sample-article-url",
            "publish_time": 1471946907,
            "thumbnail": "http://media.example.com/1992/01/09/squirrel.jpg",
            "description": "Summary of the article here. Up to 1 paragraph. No HTML allowed",
            "content": "<p>Full content of the article <em>goes here<\/em>. HTML is welcomed here<\/p> <img src=\"http:\/\/media.example.com\/squirrel.jpg\" width=\"1200\" height=\"900\"> <p>Images and videos should be placed within the article copy.<\/p> <p>More content goes here ...<\/p>"
        },
        {
            "title": "Sample Of Another Article",
            "link": "http://example.com/another-article-url",
            "publish_time": 1471946907,
            "thumbnail": "http://media.example.com/1992/01/09/squirrel.jpg",
            "description": "Summary of the article here. Up to 1 paragraph. No HTML allowed",
            "content": "<p>Full content of the article <em>goes here<\/em>. HTML is welcomed here<\/p> <img src=\"http:\/\/media.example.com\/squirrel.jpg\" width=\"1200\" height=\"900\"> <p>Images and videos should be placed within the article copy.<\/p> <p>More content goes here ...<\/p>"
        }
    ]
}
```

## Format Spec and Comparison

### Website Information
Website information is placed in the outermost section of the feed. On the RSS it's placed directly under `<channel>` section, while on JSON it's placed on root document.

Item | RSS | JSON | Description
-----|-----|------|------------
Website name        | `<title>`                         | `name`            | Name of the website
Website URL         | `<link>`                          | `link`            | URL of the website homepage
Website description | `<description>`                   | `description`     | Description of the website **(Optional)**
Last build          | `<lastBuildDate>` and `<updated>` | `last_updated`    | Timestamp of the last time the feed is built. See **Timestamp** section below for the date time reference

### Article Feed
On RSS format, the articles are represented using `<item>` tags and placed under `<channel>` section. While on JSON format, the articles are represented as list of object under the key `items`

Item | RSS | JSON | Description
-----|-----|------|------------
Article title       | `<title>`             | `title`           | Title of the article
Article URL         | `<link>`              | `link`            | URL of the article. See **URL** section below for further reference
Publish time        | `<pubDate>`           | `publish_time`    | Timestamp when the article is published
Thumbnail image     | `<media:thumbnail>`   | `thumbnail`       | URL of the article thumbnail image. Minimum dimension is 300px
Article summary     | `<description>`       | `description`     | Summary / excerpt of the article. No HTML tags is allowed here. Please note that on RSS format, the value of this tag is wrapped by `<![CDATA[ ]]>`
Article content     | `<content:encoded>`   | `content`         | Full content of the article. HTML tags is allowed here.  Please note that on RSS format, the value of this tag is wrapped by `<![CDATA[ ]]>`, and on JSON format all quotes, slash and backslash are escaped

## Value References

### Timestamp
On RSS format all date time values are shown as [RFC-822](http://www.faqs.org/rfcs/rfc822.html) format. See RSS example above for the example of the valid format.

On JSON the date time values are shown as Unix timestamp, which is a plain integer represent number of seconds since Jan 1st 1970

### URL
As stated above, all article URLs must be the complete and final URL, rather than shortened one. You may add UTM tracker in the query string to track the traffic that comes from Kurio.

Please note that when using RSS format, query string for the tracker must be separeted by `&amp;` instead of regular `&` to make sure the RSS still a valid XML.

Example of the valid URL with tracker installed:

```
http://example.com/article/sample.html?utm_campaign=content_distribution&utm_medium=Kurio&utm_source=rss_feed
```

Example of the valid URL with tracker installed in RSS feed:

```
http://example.com/article/sample.html?utm_campaign=content_distribution&amp;utm_medium=Kurio&amp;utm_source=rss_feed
```

## Authorization
All of the article feed must have public URL accessible by the crawler. If you want to give some restriction right now we can accept [HTTP Basic Authentication](https://en.wikipedia.org/wiki/Basic_access_authentication).

For example if the URL of the feed is `http://example.com/feed`
and the username / password to access it is `username` and `password`, then the crawler will access using this URL:

```
http://username:password@example.com/feed
```

## Contact
If you have further question regarding the integration to Kurio app, please reach us via `hello@kurio.co.id`
