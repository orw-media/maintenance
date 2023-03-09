![Status Badge Web-Update](https://healthchecks.io/badge/c7cfa7e6-4207-4122-b4db-87013e/7Ku4mFBW/web-update.svg)

# Maintenance Page

Incidents happen and when we schedule maintenance we often face the problem of downtime. To inform our users about the ongoing maintenance to our platform, we've designed this simple maintenance page.

## Technical information

### Transform-Rule

To redirect the user to the maintenance page, we use a set of transform- and page rules at Cloudflare to do url rewrites at the edge. 

We have a transform rule that checks for https hosts in a list and gets activated when we need a redirect to the maintenance page.

```
(http.host in {"cloud.octopus-report.de" "octopus-reoprt.de" "www.octopus-report.de"})
```

The hosts in the string can be changed to the hosts that are currently under maintenance or accrue an incident. 

The next step is a static rewrite to `/302/maintenance`. This is where our second ruleset comes in place

### Page-Rule

We have configured an always active page-rule for the domain which forwards everything matching the url `*.octopus-report.de/302/maintenance` to `https://maintenance.octopus-report.de`. 

With this solution, we can decide which hosts are forwarded to the maintenance page with editing only one transform rule

## Implementation

We designed the html-page to be as simple as possible, this includes inline css and so on. The webserver which holds the page pulls the html-file from github in an intervale to keep it up to date. Changes to the html only are made with git, so that we don't need to connect to the webserver every time we want to change a maintenance-host.