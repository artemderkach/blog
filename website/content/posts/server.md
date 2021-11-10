+++
author = "Artem Derkach"
title = "Expose App To The Internet"
date = "2021-11-07"
description = "Notes on how to deploy application"
tags = [
    "deployment",
]
+++

Notes on how to automate, deliver and make you app available for everyone.

<!--more-->
<br />
This notes talks about one of many possible ways to solve the problem.<br>
What is the problem anyway?<br>
Say, you have your application X which you want be exposed to the Internet,
for some reason, and now you have to figure how to do it in a sane way.
<br /><br />

Here is the point that will be covered:
- [Version Control / Application Code Storage]({{< ref "#version-control" >}})
- storage
<br /><br />


## Version Control / Application Code Storage
Kind of the obvious point, but let it also sit here, just for the sake of notes integrity.<br />
Some of available choices to store code for app:
- [github](https://github.com)
- [gitlab](https://gitlab.com)
- [bitbucket](https://bitbucket.com)
- self-hosted solution (for example [gitea](https://gitea.io)). If you're reading this notes, there is great possibility it's not an option.

Every listed solution is free, so, pick what you like (use `github` if you don't have strong opinion).

<br />
## Content
- domain
- cloud
- ci/cd