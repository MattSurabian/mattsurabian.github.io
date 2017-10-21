+++
date = "2017-10-20T17:00:00+09:00"
title = "You're Using SSH Wrong"
description = "A review of some of my favorite features of SSH that can save you time and trouble."
thumbnail = "images/boat-first.jpg"
+++

Ok, maybe you’re not using it wrong exactly, but I’m willing to bet there are some pretty awesome 
features you could be using that would make your life easier. In this series I’ll cover some of my 
favorite SSH features that you can hopefully put to use right away and save yourself some time. 
This series assumes you already have at least one SSH key pair created that lives in the default 
location `~/.ssh/id_rsa.pub` and `~/.ssh/id_rsa`. 

If you don’t have an SSH key pair yet you can generate one with the following command:

```
ssh-keygen -t rsa -b 4096
```

Over the next week or two I'll be publishing posts that show how I:

  - Automatically configure SSH key-based access to remote machines with a single command.
  - Get information quickly from remote machines by running one off commands over SSH.
  - Give remote machines access to things like git without needing to copy your private key(s) onto them.
  - Access resources on external networks from a remote machine using port forwarding
  - Debug common SSH access problems.
  
<br> 



