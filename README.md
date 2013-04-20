# hscript: Hola Unblocker Script
A personal configurable VPN proxy

## hscript package example:
Install the hscript package [Hola Redirect Demo](http://hola.org/share?sid=ca7bb33efed0bc08) now:

    {
        "name": "Hola Redirect Demo",
        "description": "Route requests to Maxmin through Hola peers around the world",
        "author": "Hola",
        "site": "https://github.com/HolaAPI",
        "icon": "http://hola.org/img/logo.png",
        "unblocker_rules": {
            "maxmind_UK": {
                "description": "Route requests to Maxmind from the UK",
                "link": "maxmind.com/en/locate_my_ip",
                "icon": "http://icongal.com/gallery/image/132178/uk_flag.png",
                "os": [],
                "cmds": [{
                        "hosts": ["maxmind.com"],
                        "then": "PROXY GB"}
                ]
            },
            "maxmind_US": {
                "description": "Route requests to Maxmind from the US",
                "link": "maxmind.com/en/locate_my_ip",
                "icon": "http://icongal.com/gallery/image/229138/us_flag.png",
                "os": [],
                "cmds": [{
                        "hosts": ["maxmind.com"],
                        "then": "PROXY US"}
                ]
            },
            "maxmind_ES": {
                "description": "Route requests to Maxmind from the ES",
                "link": "maxmind.com/en/locate_my_ip",
                "icon": "http://icongal.com/gallery/image/8135/es_flag.png",
                "os": [],
                "cmds": [{
                        "hosts": ["maxmind.com"],
                        "then": "PROXY ES"}
                ]
            }
        }
    }
> Access the website by routing ONLY the URLs that are authenticated through peers in that country (and leave the rest of the URLs unrouted). The above hscript package example routes IP requests through the Hola network in three different countries.  Example: instruct Hola to route all requests to yoursite.com to be routed through PROXY ES in Spain to see how your site looks to a Spanish user.

Install the hscript for Android [Hola Facebook Demo](http://hola.org/share?sid=0232ffff56b8ab0f) now:

    {
        "name": "Hola Facebook Demo",
        "description": "Access Facebook from countries that block Facebook or from corporate firewalls. Android only",
        "author": "Hola",
        "site": "https://github.com/HolaAPI",
        "icon": "http://hola.org/img/logo.png",
        "unblocker_rules": {
            "facebook": {
                "description": "Unblock Facebook app",
                "link": "facebook.com",
                "icon": "http://icongal.com/gallery/image/11148/facebook.png",
                "os": [
                    "android"
                ],
                "cmds": [{
                        "hosts": ["facebook.com","facebook.net","fbcdn.com","fbcdn.net","fbstatic-a.akamaihd.net","fbcdn-dragon-a.akamaihd.net"
                        ],
                        "dst_dns": true,
                        "then": "PROXY US"
                    }
                ]
            }
        }
    }
> This Facebook.com hscript instructs Hola Unblocker requests to route through PROXY US agents on the Hola network (e.g. (e.g. allows Iranian residents to freely use Facebook on Android). Note that routing all traffic through the Hola proxies is required for such a case, but should not be used in cases where not needed since this significantly slows down the browsing.

## Intro:
hscript, or Hola script, by [Hola](http://hola.org) lets you:

* Configure routing rules for a set of URLs for use with the Hola Unblocker VPN Proxy
* Create an hscript Package by including several rules within one hscript
* Easily share your hscript rules and hscript Packages by posting a simple URL on your site, Facebook, Twitter, email, etc.

## API:
    {
        "name": "Your hscript or hscript package name",
        "author": "Your name",
        "description": "Optional - A description of the author, hscript, hscript package, etc.)",
        "site": "Link to your website/Facebook/Twitter/Github, etc.",
        "icon": "Optional - HTTP link to icon)",
        "unblocker_rules": {
            "unblocker_rules.RULE1": {
                "description": "Optional - A description of the hscript rule",
                "link": "HTTP link to site you want to reroute traffic on for this rule",
                "icon": "Optional - HTTP link to icon",
                "os": ["Define the OS if necessary - empty == windows & windows8 & android"],
                "def-ext": ["Define the default extensions if necessary - ext #1 ...","ext #2 ..."],
                "cmds": [{
                        "hosts": ["Define the host #1 ...","host #2 ..."],
                        "if": [{
                                "ext": ["gif","png","jpg","mp3","js","css","mp4","wmv","flv","swf","json","mkv"],
                                "type": "Specify a type",
                                "then": "Specify a route"
                            }
                        ],
                        "then": "specify a route"
                    }
                ]
            },
            "unblocker_rules.RULE2...": {
            }
        }
    }



### API explained

Item                                        | Type                       | Defines                                                               | Description
------------------------------------------- | -------------------------- | --------------------------------------------------------------------- | -----------------------------------------------
name                                        | string, optional           | *Name of the hscript RULE or package of RULES*                        | 
author                                      | string, optional           | *Author*                                                              |  
description                                 | string, optional           | *Description of hscript or hscript package with free text*            | 
site                                        | string, optional           | *URL of your website, Facebook, Twitter, Github etc.*                 | 
icon                                        | string, optional           | *Image URL*                                                           | *Image used to represent the author, package, etc. (not displaying yet)* | 
unblocker\_rules                            | map, required              | *Array of rules for the sites you want to Unblock*                    | 
unblocker\_rules.RULE1                      | map                        | *Name of the rule*                                                    | *Replace RULE1 with the name of the rule. Add as many rules as you want*
unblocker\_rules.RULE1.description          | string, optional           | *Description of the rule*                                             | 
unblocker\_rules.RULE1.link                 | string, optional           | *URL of the site this rule Unblocks*                                  | 
unblocker\_rules.RULE1.icon                 | string, optional           | *Defines an image URL to represent the rule*                          | 
unblocker\_rules.RULE1.os                   | array of strings, optional | *Defines the OS support*                                              | *When undefined, the default: [""windows"",""windows8"",""android""] is used. You may need to specify the host per OS when the site or app has different IPs from one another*
unblocker\_rules.RULE1.def-ext              | array of strings, optional | *Defines the default extensions*                                      | *When undefined, the default:[""gif"",""png"",""jpg"",""mp3"",""js"",""css"",""mp4"",""wmv"",""flv"",""swf"",""json"",""mkv""] is used. def-ext is used by default by 'if' cmds when undefined in RULES1.cmds[].if[].ext*
unblocker\_rules.RULE1.cmds[]               | array, required            | *Array of commands to be executed for each URL the browser requests*  | *The purpose of these rules is to make a decision 'if' a URL belongs to the RULE1 site, and if it does, should the URL be proxied or should the browser connect directly to the web server, for faster surfing. Each site rule matching starts with a fast path match using hash on the hosts, the host array contain all the hosts that produce URLs that have GeoIP checks in them. When a request arrives the domain of the request (host) is fast searched in the currently defined hosts hash (created from the hosts array), if a match is found the other parts of the command are processed (the 'if' statements) and a 'then' is fired, if no match is found the request bypasses all the rules and send via the default path (usually directly to the web server). The cmds are executed one after another. This is very similar in concept to iptables/ipf/firewall rule programming. *
unblocker\_rules.RULE1.cmds[].hosts[]       | array, required            | *URL of host domain*                                                  | *Define as many hosts as you need*
unblocker\_rules.RULE1.cmds[].if[]          | array, optional            |                                                                       | *'if' sections can accept rules from either of 3 sources host, url and ext.*
unblocker\_rules.RULE1.cmds[].if[].host      | array of strings, optional | *Array of host to be included in the routing*                        | *e.g. "if": [{"host": "^subdomain.\*\\.domain\\.com$", "type": "=~", "then": "DIRECT"}]*
unblocker\_rules.RULE1.cmds[].if[].url     | array of strings, optional | *Array of URLs to be included in the routing*                          | *e.g. "if": [{"url": "http://subdomain.domain.com/geoip\_check" <http://subdomain.domain.com/geoip_check>, "type": "!=", "then": "DIRECT"}]* 
unblocker\_rules.RULE1.cmds[].if[].ext      | array of strings, optional | *Array of extensions to be included in the routing*                   | *Define 'ext' commands when you need to modify the default extension list. e.g. "if": [{"ext": "aaa", "type": "==", "then": "PROXY US"}]*
unblocker\_rules.RULE1.cmds[].if[].type     | string, optional           | *The 'type' of the value in host/url/ext*                             | *equal matching:  ==, !=  (value is string); regex matching: =~, !~  (value is string converted into JS regex); array item matching: in, not_in (value is array)*
unblocker\_rules.RULE1.cmds[].if[].then     | string, optional           | *Route*                                                               | *Send requests directly from your browser: ""DIRECT"" ; Route through Hola peers: ""PROXY US"" (United States); ""PROXY GB"" (United Kingdom); ""PROXY ES"" (Spain). We'll be adding more countries soon, so <a href="mailto:help@hola.org?Subject=Request%20to%20add%20a%20new%20HScript%20region"> send us your region requests for new countries</a>*
unblocker\_rules.RULE1.cmds[].if[].dst\_dns | string, optional           | *Destination DNS resolution*                                          | *ONLY use when DNS resolution is needed and all traffic must go through the proxy, as this will have extremely slow page loads! This is for when you need to get around country/corporate/university Firewalls. Define as "true" to activate DNS resolution on the host.*
unblocker\_rules.RULE1.cmds[].then          | string, required           | *Route*                                                               | *""DIRECT""; ""PROXY US"" ; ""PROXY GB""; ""PROXY ES""*
unblocker\_rules.RULE2                      | map                        | *Name of the next rule*                                               | *Add more rules to make a package!*

### Note on performance

Further explanation of 'hosts' and fast vs. slow matching: Example, say you have mysite.com which has 2 domains from which URLs are requested: www.mysite.com and img.mysite.com, and only the www subdomain has a GeoIP check on url "/authentication". There are basically 2 ways to go about defining the site rules:

    {
        "cmds": [
            {
                "hosts": [
                    "mysite.com"
                ],
                "if": [
                    {
                        "host": "img.mysite.com",
                        "type": "==",
                        "then": "DIRECT"
                    },
                    {
                        "url": ".*/authentication$",
                        "type": "!~",
                        "then": "DIRECT"
                    }
                ],
                "then": "PROXY GB"
            }
        ]
    }

or

    {
        "cmds": [
            {
                "hosts": [
                    "www.mysite.com"
                ],
                "if": [
                    {
                        "url": ".*/authentication$",
                        "type": "!~",
                        "then": "DIRECT"
                    }
                ],
                "then": "PROXY GB"
            }
        ]
    }

Both methods work, but the second one is much faster because it improves browser responsiveness, as the fast path matching of hosts is more accurate and we don't waste time on slow 'if' matchings.

## Start hscripting

* [Hola on Github](https://github.com/HolaAPI/HScript)
* From [Unblocker settings page](http://client.hola.org/client_cgi/):
  * Click *'I am smart, I can write a new Unblocker script'*
  * Click *'View script...' on any of the site buttons*
* Search the web for "hscript" or "Hola script" + keywords
  * e.g. Google: "Social sites hscript"
* Copy an hscript whose functionality resembles your desired hscript, and modify it to your needs.

## Installing Hola Unblocker:
[Hola](http://hola.org) is available for Windows/Android/Chrome/Firefox. Linux/Mac/ChromeOS is supported by the Hola Unblocker Chrome and Firefox browser extensions.

## Usage:
* We recommend opening your own Github or similar repository for all the hscript/hscript Packages you create.
* Share hscript and hscript package links with anyone by posting on your site, Facebook, Twitter, Github or anywhere you choose!
* Add an hscript by clicking the link, this will redirect to Hola Unblocker setting page and request approval for adding to the list of Unblocked sites. If Hola is not installed or running, user is prompted.
* Try it out now by adding the [Hola Redirect Demo](http://hola.org/share?sid=ca7bb33efed0bc08) to your Hola Unblocker!
* You can create, share and remove an hscript from the Hola Unblocker settings page by clicking the "I am smart" link.

## Recommend changes and feedback:
Fork us on Github and request any changes or improvements related to hscript or our documentation! You can always email us at *<a href="mailto:api@hola.org?Subject=HScript%20feedback%20from%20Github%20user">api@hola.org</a>!*

