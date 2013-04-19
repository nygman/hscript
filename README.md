# Hola Unblocker HScript
A personal configurable VPN proxy

## HScript package example:
Install the HScript package [Hola Redirect Demo](http://hola.org/share?sid=ca7bb33efed0bc08) now:

    {
        "name": "Hola Redirect Demo",
        "description": "Route your IP through the Hola peers in the US, UK and ES",
        "author": "Hola",
        "site": "https://github.com/HolaAPI",
        "icon": "http://hola.org/img/logo.png",
        "unblocker_rules": {
            "maxmind_UK": {
                "description": "Open Maxmind from the UK",
                "link": "maxmind.com/en/locate_my_ip",
                "icon": "http://icongal.com/gallery/image/132178/uk_flag.png",
                "os": [
                    "windows",
                    "windows8"
                ],
                "cmds": [
                    {
                        "hosts": [
                            "maxmind.com"
                        ],
                        "then": "PROXY GB"
                    }
                ]
            },
            "maxmind_US": {
                "description": "Open Maxmind from the US",
                "link": "maxmind.com/en/locate_my_ip",
                "icon": "http://icongal.com/gallery/image/229138/us_flag.png",
                "os": [
                    "windows",
                    "windows8"
                ],
                "cmds": [
                    {
                        "hosts": [
                            "maxmind.com"
                        ],
                        "then": "PROXY US"
                    }
                ]
            },
            "maxmind_ES": {
                "description": "Open Maxmind from the ES",
                "link": "maxmind.com/en/locate_my_ip",
                "icon": "http://icongal.com/gallery/image/8135/es_flag.png",
                "os": [
                    "windows",
                    "windows8"
                ],
                "cmds": [
                    {
                        "hosts": [
                            "maxmind.com"
                        ],
                        "then": "PROXY ES"
                    }
                ]
            }
        }
    }
> Access the website by routing ONLY the URLs that are authenticated through peers in that country (and leave the rest of the URLs unrouted). The above HScript package example routes IP requests through the Hola network in three different countries.  Example: instruct Hola to route all requests to yoursite.com to be routed through PROXY ES in Spain to see how your site looks to a Spanish user.

Install the HScript for Android [Hola Facebook Demo](http://hola.org/share?sid=0232ffff56b8ab0f) now:

    {
        "name": "Hola Facebook Demo",
        "description": "Access Facebook from blocked countries or corporate firewalls. Android only",
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
                "cmds": [
                    {
                        "hosts": [
                            "facebook.com",
                            "facebook.net",
                            "fbcdn.com",
                            "fbcdn.net",
                            "fbstatic-a.akamaihd.net",
                            "fbcdn-dragon-a.akamaihd.net"
                        ],
                        "dst_dns": true,
                        "then": "PROXY US"
                    }
                ]
            }
        }
    }
> This Facebook.com HScript instructs Hola Unblocker requests to route through PROXY US agents on the Hola network. For example, requests originating in Iran will be routed through Hola, allowing Iranian residents access to Facebook despite locally blocked IPs.

## Intro:
HScript, or Hola Script, by [Hola](http://hola.org) lets you:

* Configure routing rules for a set of URLs for use with the Hola Unblocker VPN Proxy
* Create an HScript Package by including several rules within one HScript
* Share your HScript rules and HScript Packages with the whole world

## API:
    {
        "name": "Your HScript or HScript package name",
        "author": "Your name",
        "description": "Optional - A description of the author, HScript, HScript package, etc)",
        "site": "Link to your website/Facebook/Twitter/Github, etc",
        "icon": "Optional - HTTP link to icon)",
        "unblocker_rules": {
            "unblocker_rules.RULE1": {
                "description": "Optional - A description of the HScript rule",
                "link": "HTTP link to site",
                "icon": "Optional - HTTP link to icon",
                "os": [
                    "Define the OS if necessary - leave empty for windows/windows8/android"
                ],
                "def-ext": [
                    "Define the ext if necessary - ext #1 ...",
                    "Define the ext if necessary - ext #2 ..."
                ],
                "cmds": [
                    {
                        "hosts": [
                            "Define host #1 ...",
                            "Define host #2 ..."
                        ],
                        "if": [
                            {
                                "ext": [
                                    "gif",
                                    "png",
                                    "jpg",
                                    "mp3",
                                    "js",
                                    "css",
                                    "mp4",
                                    "wmv",
                                    "flv",
                                    "swf",
                                    "json",
                                    "mkv"
                                ],
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



### Let's break it down:


    field (type, required)
*format of the following json descriptions*

    name (string, optional)
*Name of the HScript RULE or package of RULES*

    author (string, optional)
*Credits the author*

    description (string, optional)
*Describes the HScript or HScript package with free text*

    site (string, optional)
*URL of your website, Facebook, Twitter, Github etc*

    icon (string, optional)
*Defines an image URL to represent the author, package, etc (not displaying yet*

    unblocker_rules (hash, required)
*Array of rules for the sites you want to Unblock*

    unblocker_rules.RULE1 (hash)
*replace RULE1 with the name of the rule. Add as many rules as you want.*

    unblocker_rules.RULE1.description (string, optional)
*Describes the rule with free text*

    unblocker_rules.RULE1.link (string, optional)
*URL of the site this rule Unblock*

    unblocker_rules.RULE1.icon (string, optional)
*Defines an image URL to represent the rule*

    unblocker_rules.RULE1.os (array of strings, optional)
*Defines the OS support. When undefined, the default: [""windows"",""windows8"",""android""] is used.
You may need to specify the host per OS when the site or app has different IPs from one another.*

    unblocker_rules.RULE1.def-ext (array of strings, optional)
*When undefined, the default:[""gif"",""png"",""jpg"",""mp3"",""js"",""css"",""mp4"",""wmv"",""flv"",""swf"",""json"",""mkv""] is used.
def-ext is used by default by 'if' cmds when undefined in RULES1.cmds[].if[].ext*

    unblocker_rules.RULE1.cmds[] (array, required)
*Array of commands to be executed for each URL the browser requests. The purpose of these rules is to make a decision 'if' a URL belongs to the RULE1 site, and if it does, should the URL be proxied or should the browser connect directly to the web server, for faster surfing. The cmds are executed one after another. When the cmd is matched the 'then' part is executed, and the URL is considered belonging to the RULE1 site.
This is very similar in concept to iptables/ipf/firewall rule programming.*

    unblocker_rules.RULE1.cmds[].hosts[] (array, required)
*URL of host domain, define as many hosts as you need.*

    unblocker_rules.RULE1.cmds[].if[] (array, optional)
*Define 'if' commands only when you need to modify the default extension list*

    unblocker_rules.RULE1.cmds[].if[].ext (array of strings, optional)
*Array of extensions to be included in the routing.*

    unblocker_rules.RULE1.cmds[].if[].type (string, optional)
*Specify a command type: ""=="" /* equal */ | * ""!="" /* not equal */ | * ""=~"" /* regex equal */ | * ""!~"" /* not regex not equal */ | * ""in"" /* in list */ | * ""not_in"" /* no in list */*

    unblocker_rules.RULE1.cmds[].if[].then (string, optional)
*Specify a direct route or through Hola peers: ""DIRECT"" /* send requests directly from your browser */ | ""PROXY US"" /* route through Hola peers in United States */ | ""PROXY GB"" /* route through Hola peers in United Kingdom */ | ""PROXY ES"" /* route through Hola peers in Spain */. We'll be adding more countries soon, so <a href="mailto:help@hola.org?Subject=Request%20to%20add%20a%20new%20HScript%20region"> send us your region requests for new countries</a>.*

    unblocker_rules.RULE1.cmds[].if[].dst_dns (string, optional)
*ONLY use when DNS resolution is needed and all traffic must go through the proxy, as this will have extremely slow page loads! This is for when you need to get around country/corporate/university Firewalls. Define as "true" to activate DNS resolution on the host.*

    unblocker_rules.RULE1.cmds[].then (string, required)
*Specify a direct route or through Hola peers: ""DIRECT"" | ""PROXY US"" | ""PROXY GB"" | ""PROXY ES"" *

    unblocker_rules.RULE2... (hash)
*Add more rules to make a package!*

## Start HScripting:
* [Hola on Github](https://github.com/HolaAPI/HScript)
* From [Unblocker settings page](http://client.hola.org/client_cgi/):
  * Click *'I am smart, I can write a new Unblocker script'*
  * Click *'View script...' on any of the site buttons*
* Search the web for "HScript" or "Hola Script" + keywords
  * e.g. Google: "Social sites HScript"
* Copy an HScript whose functionality resembles your desired HScript, and modify it to your needs.

## Installing Hola Unblocker:
[HOLA](http://hola.org) is available for Windows/Android/Chrome/Firefox

*Linux/Mac/ChromeOS is supported by the Hola Unblocker browser extensions*

## Usage:
* We recommend opening your own Github or similar repository for all the HScript/HScript Packages you create.
* Share HScript and HScript package links with anyone by posting on your site, Facebook, Twitter, Github or anywhere you choose!
* Add an HScript by clicking the link, this will redirect to Hola Unblocker setting page and request approval for adding to the list of Unblocked sites. If Hola is not installed or running, user is prompted.
* Try it out now by adding the [Hola Redirect Demo](http://hola.org/share?sid=ca7bb33efed0bc08) to your Hola Unblocker!
* You can create, share and remove an HScript from the Hola Unblocker settings page by clicking the "I am smart" link.

## Recommend changes and feedback:
Fork us on Github and request any changes or improvements related to HScript or our documenation! You can always email us at *<a href="mailto:help@hola.org?Subject=HScript%20feedback%20from%20Github%20user">help at hola dot org</a>!*
