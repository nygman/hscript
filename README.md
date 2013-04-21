# hscript: Hola Unblocker Script
A personal configurable VPN proxy

## hscript package example:
**[Hola Redirect Demo](http://hola.org/share?sid=ca7bb33efed0bc08):**
```json
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
        "cmds": [{"hosts": ["maxmind.com"], "then": "PROXY GB"}]
      },
      "maxmind_US": {
        "description": "Route requests to Maxmind from the US",
        "link": "maxmind.com/en/locate_my_ip",
        "icon": "http://icongal.com/gallery/image/229138/us_flag.png",
        "cmds": [{"hosts": ["maxmind.com"], "then": "PROXY US"}]
      },
      "maxmind_ES": {
        "description": "Route requests to Maxmind from the ES",
        "link": "maxmind.com/en/locate_my_ip",
        "icon": "http://icongal.com/gallery/image/8135/es_flag.png",
        "cmds": [{"hosts": ["maxmind.com"], "then": "PROXY ES"}]
      }
    }
  }
```
> Access the website by routing ONLY the URLs that are authenticated through peers in that country (and leave the rest of the URLs unrouted). The above hscript package example routes IP requests through the Hola network in three different countries.  Example: instruct Hola to route all requests to yoursite.com to be routed through PROXY ES in Spain to see how your site looks to a Spanish user.

**[Hola Facebook Demo - Android](http://hola.org/share?sid=0232ffff56b8ab0f):**
```json
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
        "os": ["android"],
        "cmds": [{
          "hosts": ["facebook.com","facebook.net","fbcdn.com","fbcdn.net","fbstatic-a.akamaihd.net",
            "fbcdn-dragon-a.akamaihd.net"],
          "dst_dns": "true",
          "then": "PROXY US"
        }]
      }
    }
  }
```
> This Facebook.com hscript instructs Hola Unblocker requests to route through PROXY US agents on the Hola network (e.g. allows Iranian residents to freely use Facebook on Android). Note that routing all traffic through the Hola proxies is required for such a case, but should not be used in cases where not needed since this significantly slows down the browsing.

## Intro:
hscript, or Hola script, by [Hola](http://hola.org) lets you:
* Configure routing rules for a set of URLs for use with the Hola Unblocker VPN Proxy
* Create an hscript package by including several rules within one hscript
* Easily share your hscript rules and hscript packages by posting a simple URL on your site, Facebook, Twitter, email, etc.

## API explained:
* `name` optional string: name of the hscript package  
* `author` optional string: Author  
* `description` optional string: Description of hscript package  
* `site` optional string: URL of your website, Facebook, Twitter, Github etc.  
* `icon` optional string: Image URL used to represent the author, package, etc. (not displaying yet)  
* `unblocker_rules` required map: Routing rules for the sites you want to Unblock  
* `unblocker_rules.RULE1` required map: Name of the rule.
  Replace `RULE1` with the name of the rule. Add as many rules as you want  
* `unblocker_rules.RULE1.description` optional string: Description of the rule  
* `unblocker_rules.RULE1.link` optional string: URL of the site this rule Unblocks  
* `unblocker_rules.RULE1.icon` optional string: Defines an image URL to represent the rule  
* `unblocker_rules.RULE1.os` optional array of strings: Defines the OS support.
  When undefined, the default: ["windows","windows8","android"] is used. You may need to specify the host per OS when the site or app has different IPs from one another  
* `unblocker_rules.RULE1.def-ext` optional array of strings: Defines the default extensions.
  When undefined, the default:["gif","png","jpg","mp3","js","css","mp4","wmv","flv","swf","json","mkv"] is used.
  def-ext is used by default by 'if' cmds when undefined in RULES1.cmds[].if[].ext  
* `unblocker_rules.RULE1.cmds[]` required array: commands to be executed for each URL the browser requests.
  The purpose of these rules is to make a decision 'if' a URL belongs to the RULE1 site, and if it does, should the URL be proxied or should the browser connect directly to the web server, for faster surfing. Each site rule matching starts with a fast path match using hash on the hosts, the host array contain all the hosts that produce URLs that have GeoIP checks in them. When a request arrives the domain of the request (host) is fast searched in the currently defined hosts hash (created from the hosts array), if a match is found the other parts of the command are processed (the 'if' statements) and a 'then' is fired, if no match is found the request bypasses all the rules and send via the default path (usually directly to the web server). The cmds are executed one after another. This is very similar in concept to iptables/ipf/firewall rule programming  
* `unblocker_rules.RULE1.cmds[].hosts[]` required array: URL of host domain.
  Define as many hosts as you need  
* `unblocker_rules.RULE1.cmds[].if[]` optional array: 'if' sections can accept rules from either of 3 sources host, url and ext.  
* `unblocker_rules.RULE1.cmds[].if[].host` optional array of strings: hosts to be included in the routing.
  e.g. "if": [{"host": "^subdomain.\*\\.domain\\.com$", "type": "=~", "then": "DIRECT"}]  
* `unblocker_rules.RULE1.cmds[].if[].url` optional array of strings: URLs to be included in the routing.
  e.g. "if": [{"url": "http://subdomain.domain.com/geoip_check", "type": "!=", "then": "DIRECT"}]  
* `unblocker_rules.RULE1.cmds[].if[].ext` optional array of strings: file extensions to be included in the routing. Define 'ext' commands when you need to modify the default extension list. e.g. "if": [{"ext": "aaa", "type": "==", "then": "PROXY US"}]  
* `unblocker_rules.RULE1.cmds[].if[].type` optional string: The 'type' of the value in host/url/ext.
  equal matching:  ==, !=  (value is string); regex matching: =~, !~  (value is string converted into JS regex); array item matching: in, not_in (value is array)  
* `unblocker_rules.RULE1.cmds[].if[].then` optional string: Select the route.
  Send requests directly from your browser: "DIRECT" ; Route through Hola peers: "PROXY US" (United States); "PROXY GB" (United Kingdom); "PROXY ES" (Spain). We'll be adding more countries soon, so <a href="mailto:help@hola.org?Subject=Request%20to%20add%20a%20new%20HScript%20region"> send us your region requests for new countries</a>  
* `unblocker_rules.RULE1.cmds[].if[].dst_dns` optional string: Destination DNS resolution.
  ONLY use when DNS resolution is needed and all traffic must go through the proxy, as this will have extremely slow page loads! This is for when you need to get around country/corporate/university Firewalls. Define as "true" to activate DNS resolution on the host.
* `unblocker_rules.RULE1.cmds[].then` required string: Select the route.
  "DIRECT"; "PROXY US" ; "PROXY GB"; "PROXY ES"  
* `unblocker_rules.RULE2` optinal map: Name of the next rule, more than one rule makes a package!  

### Note on performance
Further explanation of 'hosts' and fast vs. slow matching: Example, say you have mysite.com which has 2 domains from which URLs are requested: www.mysite.com and img.mysite.com, and only the www subdomain has a GeoIP check on url "/authentication". There are basically 2 ways to go about defining the site rules:

```json
  {
    "cmds": [{
      "hosts": ["mysite.com"],
      "if": [{"host": "img.mysite.com", "type": "==","then": "DIRECT"},
        {"url": ".*/authentication$", "type": "!~", "then": "DIRECT"}],
      "then": "PROXY GB"
    }]
  }
```
or
```json
  {
    "cmds": [{
      "hosts": ["www.mysite.com"],
      "if": [{"url": ".*/authentication$", "type": "!~", "then": "DIRECT"}],
      "then": "PROXY GB"
    }]
  }
```

Both methods work, but the second one is much faster because it improves browser responsiveness, as the fast path matching of hosts is more accurate and we don't waste time on slow 'if' matchings.

## Creating hscripts
Creating new hscripts is easy!

1. Get started:
  * [Hola on Github](https://github.com/HolaAPI/HScript)
  * From [Unblocker settings page](http://client.hola.org/client_cgi/), click 'I am smart, I can write a new Unblocker script' or click 'View script...' on any of the site buttons
  * Search the web for "hscript" or "Hola script" + keywords, e.g. Google: "Social sites hscript"
  * Copy an hscript whose functionality resembles your desired hscript, and modify it to your needs.
2. Fork us, or open your own Github for the hscripts you create.
3. When you create an hscript, you receive a unique link to the hscript which you can post or share online, anywhere you choose!
  * Try sending an hscript link now with the Hola Redirect Demo [http://hola.org/share?sid=ca7bb33efed0bc08](http://hola.org/share?sid=ca7bb33efed0bc08)!

## Installing Hola Unblocker
[Hola](http://hola.org) is available for Windows/Android/Chrome/Firefox. Linux/Mac/ChromeOS is supported by the Hola Unblocker Chrome and Firefox browser extensions.

## Receiving hscripts
Sharing hscripts is even easier!

1. Add an hscript to your Hola Unblocker by clicking the hscript link.
2. Clicking the link will direct your browser to the Hola Unblocker setting page, where you must approve adding the hscript to your list of Unblocked sites.
  * If Hola is not installed or running, you will get prompted to open Hola or install now at [http://hola.org](http://hola.org).
3. Add an hscript now with the [Hola Redirect Demo](http://hola.org/share?sid=ca7bb33efed0bc08)!
4. Share and remove hscripts from the Hola Unblocker settings page at any time.

## Recommend changes and feedback
We would love your help to make the Hola API and hscript even better!

1. Fork us on Github!
2. Improve the documentation or the Hola API, etc.
3. We would also love your help with creating documentation for how to use the Chrome debugger or Firefox Firebug to help build and debug hscripts.
4. Send us a pull request and we'll update our Github page often!

Email any other feedback to <a href="mailto:api@hola.org?Subject=hscript%20feedback%20from%20Github%20user">api@hola.org</a>!
