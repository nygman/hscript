# hscript: Hola Unblocker Script
A scripting language for configuring the [Hola Unblocker VPN](https://hola.org) that lets you:  
* Unblock sites by creating a script for configuring routing rules (hscript) for use with the Hola Unblocker VPN  
* Create an hscript package by including several rules within one hscript  
* Easily share your hscript by posting a simple URL on your site, Facebook, Twitter, email, etc.  
* Edit and update your hscripts. Anyone using the script will receive the updates automatically!  

Know **iptables**? You'll find writing hscript rules a breeze!  
We recommend using **FireBug** or **Chrome Developer Tools** to assist in rule development.  

## hscript package example:
**[Redirect Demo](https://hola.org/unblocker?hscript=ca7bb33efed0bc08):**
```json
  {
    "name": "Redirect Demo",
    "description": "Route requests to Maxmind's IP to Geographical location page through Hola peers around the world",
    "author": "John Doe",
    "site": "http://john-doe-blog.com",
    "icon": "http://john-doe-blog.com/img/logo.png",
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
        "description": "Route requests to Maxmind from Spain",
        "link": "maxmind.com/en/locate_my_ip",
        "icon": "http://icongal.com/gallery/image/8135/es_flag.png",
        "cmds": [{"hosts": ["maxmind.com"], "then": "PROXY ES"}]
      }
    }
  }
```
> Access the website by routing ONLY the URLs that are authenticated through peers in that country  
(and leave the rest of the URLs unrouted).

**[Facebook Demo - Android](https://hola.org/share?sid=0232ffff56b8ab0f):**
```json
  {
    "name": "Facebook Demo",
    "description": "Access Facebook from countries that block it or from behind a corporate Firewall. Android only",
    "author": "John Doe",
    "site": "http://john-doe-blog.com",
    "icon": "http://john-doe-blog.com/img/logo.png",
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
> This Facebook.com hscript instructs Hola Unblocker requests to route through PROXY US agents on the Hola network  
(e.g. allows Iranian residents to freely use Facebook on Android). Note that routing all traffic through the Hola
proxies is required for such a case, but should not be used in cases where not needed since this significantly slows down the browsing.

## API explained:
* `name` *optional string*: **name of the hscript package**  
* `description` *optional string*: **Description of hscript package**  
* `author` *optional string*: **Author name**  
  e.g.: `John Doe`  
* `site` *optional string*: **Author's website URL**  
  e.g.: `john-doe-blog.com`, `facebook.com/john.doe`, `twitter.com/john.doe`, `github.com/john.doe`.  
* `icon` *optional string*: **Author's Image URL**  
  Image that represents the author, package, etc: `john-doe-blog.com/john.png`   
  Currently not displayed.  
* `unblocker_rules` *required map*: **Routing rules for the sites to Unblock**  
* `unblocker_rules.RULE1` *required map*: **Unblocker rule/site ID**  
  Replace `RULE1` with the rule ID.  
  * One hscript package can have multiple rules/sites. User can enable each site independently.  
  * You can now add spaces in the rule ID  
  Examples:  
  * `unblocker_rules.facebook` for unblocking facebook.com  
  * `unblocker_rules.wiki website` for unblocking wikipedia.com  
  * `unblocker_rules.google from us` for unblocking google.com via the US  
* `unblocker_rules.RULE1.description` *optional string*: **Description of the rule**   
* `unblocker_rules.RULE1.link` *optional string*: **URL of the site this rule Unblocks**   
* `unblocker_rules.RULE1.icon` *optional string*: **Image URL of the rule/site**  
* `unblocker_rules.RULE1.root_url` *optional array of strings*: **Array of URLs that identify the root site**  
  * Each URL can be an exact site or a URL pattern matching.  
  Examples:  
  * http://www.site.com (match www.site.com and www.site.com/xxx)  
  * \*://\*.site.com/** (match a.site.com/xxx but not a.b.site.com/xxx)  
  * \*://\*\*.site.com/** (match a.site.com/xxx and a.b.site.com/xxx)  
  * \*\*.site.com (same as \*://\*\*.site.com/**)  
* `unblocker_rules.RULE1.os` *optional array of strings*: **OS/device support**  
  Default when undefined: `["windows","android"]`.  
  If the rule doesn't work for you on all OS/devices, then it's likely you will need to specify the host per
  OS/device as the site or app has different IPs from one OS/device to another.   
* `unblocker_rules.RULE1.apk` *optional map*: **Add Android app APK download feature**
* `unblocker_rules.RULE1.apk.pkg` *required string*: **Add Android app pkg name**  
  * To find the pkg name of the app, go to http://play.google.com and search for application.  
  * From the URL address, copy the pkg id, e.g. org.hola  
* `unblocker_rules.RULE1.apk.url` *optional string*: **Add Android app APK download URL location**  
  * e.g. "https://www.cubby.com/pl/Viber_3.0.2.5.apk/_efd19a1656ae4865886d0e738073de02"  
  * If no apk.url is defined, Hola will try to automatically download pkg APK file from Play  
  Note: Android 2.3 cannot use HTTPS links, only HTTP!  
* `unblocker_rules.RULE1.def-ext` *optional array of strings*: **Default extensions**  
  Default when undefined: `["gif","png","jpg","mp3","js","css","mp4","wmv","flv","swf","json","mkv"]`.  
  Note: `def-ext` is used by default by `if` cmds when undefined in `RULES1.cmds[].if[].ext`  
* `unblocker_rules.RULE1.cmds[]` *required array*: **Commands to be executed for each URL the browser requests**  
  The purpose of these rules is to make a decision `if` a URL belongs to the `RULE1` site, and if it does,
  should the URL be proxied or should the browser connect directly to the web server, for faster surfing.   
  Each site rule matching starts with a fast path match using hash on the hosts, the host array contain all the
  hosts that produce URLs that have GeoIP checks in them.  
  When a request arrives the domain of the request (host) is fast searched in the currently defined hosts hash
  (created from the hosts array), if a match is found the other parts of the command are processed
  (the `if` statements) and a `then` is fired, if no match is found the request bypasses all the rules and
  send via the default path (usually directly to the web server).  
  The cmds are executed one after another. This is very similar in concept to iptables/ipf/firewall rule programming.  
* `unblocker_rules.RULE1.cmds[].hosts[]` *required array*: **URL of host domain**  
  Specify as many hosts as you need.  
  * You can now also specify an IP range  
  Examples:
  *  "108.175.32.0/24" -> 108.175.32.0 - 108.175.32.255 [see ip range calculator](http://www.csgnetwork.com/ipinfocalc.html)  
  *  "108.175.32.123' === '108.175.32.123/32"
* `unblocker_rules.RULE1.cmds[].if[]` *optional array*: **`if` can accept rules from either of 3 host sources, URL and ext.**  
* `unblocker_rules.RULE1.cmds[].if[].host` *optional array of strings*: **Hosts to be included in the routing**  
  e.g. `"if": [{"host": "^subdomain.\*\\.domain\\.com$", "type": "=~", "then": "DIRECT"}]`  
* `unblocker_rules.RULE1.cmds[].if[].url` *optional array of strings*: **URLs to be included in the routing**  
  e.g. `"if": [{"url": "http://subdomain.domain.com/geoip_check", "type": "!=", "then": "DIRECT"}]`  
* `unblocker_rules.RULE1.cmds[].if[].ext` *optional array of strings*: **File extensions to be included in the routing**  
  Define `ext` commands when you need to modify the default extension list.  
  e.g. `"if": [{"ext": "aaa", "type": "==", "then": "PROXY US"}]`  
* `unblocker_rules.RULE1.cmds[].if[].type` *optional string*: **Comparison method for `host`/`url`/`ext`**  
  * `==` equal matching of string value  
  * `!=` not equal matching of string value  
  * `=~` regex matching: `^foo\..*\.com$` will match foo.bar.com foo.b.a.r.com but not foo.com  
  * `!~` regex not matching.  
  * `in` array matching: `["foo.com", "bar.foo.com"]` will match bar.foo.com but not other.foo.com  
  * `not_in` array not matching.  
* `unblocker_rules.RULE1.cmds[].if[].then` *optional string*: **Route directly or via VPN**  
  * `"DIRECT"` Don't proxy. Send requests directly from your browser to the web server.  
  * `"PROXY US"` Route through the United States  
  * `"PROXY GB"` Route through the United Kingdom  
  * `"PROXY ES"` Route through Spain  
  * `"PROXY DE"` Route through Germany  
  * `"PROXY FR"` Route through France  
  * `"PROXY RU"` Route through Russia  
  * `"PROXY AU"` Route through Australia  
  * `"PROXY CA"` Route through Canada  
  * `"PROXY IT"` Route through Italy  
  * `"PROXY SE"` Route through Sweden  
  * `"PROXY CL"` Route through Chile  
  * `"PROXY NL"` Route through the Netherlands  
  * `"PROXY IE"` Route through Ireland  
  * `"PROXY DK"` Route through Denmark  
  * `"PROXY IN"` Route through India  
  * `"PROXY EG"` Route through Egypt  
  * `"PROXY CH"` Route through Switzerland  
  * `"PROXY BR"` Route through Brazil  
  * `"PROXY TR"` Route through Turkey  
  * `"PROXY PL"` Route through Poland  
  * `"PROXY SG"` Route through Singapore  
  * `"PROXY HK"` Route through Hong Kong  
  <a href="https://github.com/hola/hscript/blob/master/README.md#become-a-country-maintainer">Become a Country maintainer</a>  
  We'll be adding more countries soon!
  <a href="mailto:api@hola.org?Subject=Request%20to%20add%20a%20new%20hscript%20region">
  Send a request to add a new country</a>  
* `unblocker_rules.RULE1.cmds[].if[].dst_dns` *optional string*: **Destination DNS resolution**  
  **ONLY** use when DNS resolution is needed and all traffic must go through the proxy, as this will
  have extremely slow page loads!  
  This is for when you need to get around country/corporate/university Firewalls.  
  Define as `"true"` to activate DNS resolution on the host.  
* `unblocker_rules.RULE1.cmds[].then` *required string*: **Route directly or via VPN**  
  `"DIRECT"`, `"PROXY US"`, `"PROXY GB"`, `"PROXY ES"`, `"PROXY DE"`, `"PROXY FR"`, `"PROXY RU"`,
  `"PROXY AU"`, `"PROXY CA"`, `"PROXY IT"`, `"PROXY SE"`, `"PROXY CL"`, `"PROXY NL"`, `"PROXY IE"`,
  `"PROXY DK"`, `"PROXY IN"`, `"PROXY EG"`, `"PROXY CH"`, `"PROXY BR"`, `"PROXY TR"`  

### Script debug
* This feature is only available for the Hola client for Windows (not the Hola Unblocker browser extensions for Chrome & Firefox).
* To enter debug mode click "Developer mode" then "Create new script". To disable debug mode click "Simple mode".
* In debug mode, Hola will add 'X-Hola-Unblocker-Debug' response header to any http request (you can view all network traffic using Chrome Developer Tools and selecting the network tab).
* X-Hola-Unblocker-Debug syntax:
  * "rule xxx js direct" the request was captured by rule xxx but the traffic was sent directly without using a vpn proxy.
  * "rule xxx country us cpnnn" the request was captured by rule xxx and the traffic was sent to a US proxy (where nnn is an integer).
  * "rule xxx 'type' direct" e.g. 'rule cnn jpg direct' where rule 'cnn' has captured this url but it was identified as type 'jpg' which is marked for 'direct' download by the 'cnn' rule.

### Note on performance
Further explanation of `"host"` and fast vs. slow matching:  
*Example*: Say you have mysite.com which has 2 domains from which URLs are requested:  
`www.mysite.com` and `img.mysite.com`, and only the www subdomain has a GeoIP check on url `"/authentication"`.  
There are basically 2 ways to go about defining the site rules:

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

Both methods work, but the second one is much faster because it improves browser responsiveness,  
as the fast path matching of hosts is more accurate and we don't waste time on slow `"if"` matchings'.  

## Creating hscripts
Creating new hscripts is easy!  

1. Get started by finding a script with functionality similar to what you need to use as a template:  
  * From existing hscripts: from the [Unblocker settings page](https://client.hola.org/client_cgi/),
    press 'Create a new rule (power users)' and then 'View script...' on any of the site buttons
    that are similar to what you want to accomplish
  * or from similar scripts on the web: Search for "hscript" + site name
  * or from one of the examples on this Github page
  * or start with a new rule template: From the [Unblocker settings page](https://client.hola.org/client_cgi/),
    press 'Create a new rule (power users)' then 'Create new script'
2. Modify the template: Use the API reference on this page to modify the template to your needs
3. Share your hscript:
  * When you create an hscript (or when you press 'share' on an hscript), you receive a unique link
    to the hscript which you can post or share online with other Hola users
  * Create your own hscript page with links to all the hsripts that you recommend for others to use
  * or Fork this Github repository, then create your own hscripts.
  * To see how this works, try sending an hscript link now with the Hola Redirect Demo
    [https://hola.org/unblocker?hscript=0ca1bf7f4747e408&sites=maxmind_US+maxmind_GB+maxmind_ES](http://hola.org/unblocker?hscript=0ca1bf7f4747e408&sites=maxmind_US+maxmind_GB+maxmind_ES)  

## Android tips
You can write rules for Android, but you cannot write rules ON Android (there is no proper development environment).
Create Android hscripts on your PC and share the link with yourself to test on Android. When you modify the hscript,
your Android client will automatically receive the updated hscript.

Add the Android application APK download URL link to the hscript rule, so that users can automatically download and install once the hscript rule is enabled.
Example:
```json
  {
    "name": "Viber from Saudi Arabia",
    "author": "Hola",
    "site": "https://hola.org",
    "unblocker_rules": {
      "viber_app": {
        "active": true,
        "description": "Access Viber from Saudi Arabia (Android app)",
        "link": "www.viber.com",
        "icon": "http://viber.com/logo_viber.png",
        "os": ["android"],
        "apk": {"pkg": "com.viber.voip", "url": "https://www.cubby.com/pl/Viber_3.0.2.5.apk/_efd19a1656ae4865886d0e738073de02"},
        "cmds": [{
          "hosts": [
            "aloha.viber.com", "secure.viber.com", "share.viber.com",
	          "download.viber.com", "54.225.248.0/24"
      }
    }
  }
```

## Installing Hola Unblocker
[Hola](https://hola.org) is available for Windows/Android/Chrome/Firefox.  
Linux/Mac/Chrome OS is supported by the Hola Unblocker extension for Chrome and Firefox browsers.  

## Sharing hscripts

  * When you create an hscript (or when you press 'share' on an hscript), you receive a unique link
    to the hscript which you can post or share online with other Hola users
  * Create your own hscript page with links to all the hscripts that you recommend for others to use
  * or Fork this Github repository, then create your own hscripts.
  * To see how this works, try sending an hscript link now with the 
    [https://hola.org/unblocker?hscript=0ca1bf7f4747e408&sites=maxmind_US+maxmind_GB+maxmind_ES] Redirect Demo  
  * Sharing link syntax:
    https://hola.org/unblocker?hscript=sid&enable=site1+site2&sites=sites1+sites2
    * hscript: the hscript id of the hscript you want to share
    * enable: optional, list of rules that will be enabled by default upon hscript import
    * sites: optional, list of domains that the hscript Unblocks

## Publicizing your hscripts

Help the community by sharing your hscripts on Github!
This will help other users develop their own rules based on your creations.

If you don't already have a Github account, [open one](http://github.com) (it's free!) and fork our hscript project.
You can use it as a base for your own rules, and then create your own hscripts projects which can be forked and shared.

## Become a Country maintainer
Hola creates the generic VPN technology, but you can make Hola Unblocker more powerful for other Hola users.
How? Start a website, blog or social media page to collect, create and share Unblocker hscripts, so that other users can easily add more sites and web apps to their Hola Unblocker.

Get started by forking this Github project and reading the API documention. Then, start creating your own hscripts, copying other scripts into your project (start by pulling the fork requests we 
receive from other users) and publishing both the hscript json code and hscipt links.

hscript feature highlights:
* Hscript packages: one link lets users add a collection of sites to unblock
* Editing scripts: it's easy, and users automatically receive your updates
* Android: include a link to download the APK file for Android apps you unblock
    
## Recommend changes and feedback
We would love your help to make the Hola API and hscript even better!  

1. Fork us on Github!  
2. Improve the documentation or the Hola API, etc.  
3. We would love your help with creating how to documentation for Chrome debugger and Firefox Firebug for building hscripts.  
4. Send us a pull request and we'll update our Github page often!  

Email any other feedback to 
<a href="mailto:api@hola.org?Subject=hscript%20feedback%20from%20Github%20user">api@hola.org</a>!  

## Become a Beta tester
[Join the Hola Beta Tester community](https://plus.google.com/u/0/communities/104757099977208104685) to get access to pre-release versions and new API features.

[![githalytics.com alpha](https://cruel-carlota.pagodabox.com/bb22ca5bd34cd07fba652c3a14c2eb07 "githalytics.com")](http://githalytics.com/holaAPI/hscript)
