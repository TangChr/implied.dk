---
title: 	jhub
layout: project
github: "https://github.com/TangChr/jhub"
npm: 	"https://www.npmjs.com/package/jhub"
meta:
  github:
    user: TangChr
    repo: jhub
  npm: jhub
  travis:
    active: true
    branch: master
---
jhub is a simple JavaScript library, which makes it possible to query the GitHub API for various information, such as:

- Repositories
  - Commits
  - Releases
- Users
- Organizations
  - Repositories
  - Members
- Gists
  - Files

<div class="seperator"></div>
The source code for jhub can be found [here]({{ page.github }}), or installed using [npm]({{ page.npm }}).

### Basic examples
```javascript
/*
Get repositories
*/
var hub = jhub.init('TangChr');
hub.userRepos(function(repos) {
    for(r in repos) {
        console.log(repos[r].name);
    }
});
```

<div class="seperator"></div>
```javascript
/*
Get starred repositories
*/
var hub = jhub.init('TangChr');
hub.starredRepos(function(starred) {
    for(s in starred) {
        console.log(starred[s].name);
    }
});
```

<div class="seperator"></div>
```javascript
/*
Get releases
*/
var hub = jhub.init('TangChr');
var repo = hub.userRepo('jhub');
repo.releases(function(releases) {
    for(r in releases) {
        console.log(releases[r].tagName + ': ' + releases[r].name);
    }
});
```

<div class="seperator"></div>
```javascript
/*
Get commits
*/
var hub = jhub.init('TangChr');
var repo = hub.userRepo('jhub');
repo.commits(function(commits) {
    for(c in commits) {
        console.log(commits[c].author);
        console.log(commits[c].comitter);
        console.log(commits[c].message);
    }
});
```

### "Advanced" examples
```javascript
/*
List a users organizations
*/
var hub = jhub.init('TangChr');
hub.userOrgs(function(orgs) {
    for(o in orgs) {
        var org = hub.org(orgs[o].login);
        org.get(function(info) {
            console.log(info.login+': '+info.name);
        });
    }
});
```

<div class="seperator"></div>
```javascript
/*
List all members of a specific organization
*/
var hub = jhub.init();
var org = hub.org('TangMedia');
org.members(function(members) {
    for(m in members) {
        var user = hub.user(members[m].login);
        user.get(function(info) {
            console.log(info.name);
        });
    }
});
```

### Gists
```javascript
/*
List all Gists for a specific user (only public Gists will be shown)
*/
var hub = jhub.init('TangChr');
hub.userGists(function(gists) {
    for(g in gists) {
        console.log(gists[g].description);
    }
});
```

<div class="seperator"></div>
```javascript
/*
List all Gists for a specific user (only public Gists will be shown)
This example also lists the files in each Gist
*/
var hub = jhub.init('TangChr');
hub.userGists(function(gists) {
    for(g in gists) {
        var gist = hub.gist(gists[g].id);
        gist.get(function(info) {
            console.log('Gist: '+info.description);
                for(f in info.files)
                    console.log('- '+info.files[f].name);
        });
    }
});
```