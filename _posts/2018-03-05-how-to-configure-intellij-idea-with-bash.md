---
title:  "How to configure Intellij Idea with bash/sh"
categories: git svn how-to
comments: true
categories:
  - How to
tags:
  - git
  - bash
  - idea
  - windows
toc: true
---
Im using Intellij Idea on my MacBook at home and Win laptop at work. Must have flow for me its work with git through console. 
But windows console is so unusfull even for simple action like: add, edit, del some files. 
There lot of thinks that Im mising while I work on windows :smile:. But one of the prolem will be fixed with this post.

## Precondition
To use bash on your Windows we need to install **git bash**: [windows download](https://git-scm.com/download/win).
And offcourse you shouhd have Intellij Idea installed.


### Step by step instructions

1. Open Idea and go to:
    ```sh
    $ File > Settings > Tools > Terminal
    ```
    You shoud get:
    ![tools terminal]({{ site.url }}{{ site.baseurl }}/assets/images/idea-bash/tools-terminal.PNG)

2. Go to created directory
    ```sh
    $ cd <Project-Name>
    ```
3. Change Settings
   Here we need to endit only **Shell path** and Apply changes.
  
   For Win 64 bit:
    ```sh
   "C:\Program Files\Git\bin\sh.exe" -login -i
    ```
    
    For or Win 32 bit:
    ```sh
    "C:\Program Files (x86)\Git\bin\sh.exe" -login -i
    ```
    And you will get:
    ![tools terminal]({{ site.url }}{{ site.baseurl }}/assets/images/idea-bash/tools-shell-path.PNG)
    
    **Note:**: Don't forget the quotes around the command.
    
4. Open\Reopen terminal
    To open terminal use: **Alt + F12** hot key or: 
    
    ![open terminal]({{ site.url }}{{ site.baseurl }}/assets/images/idea-bash/open-terminal.gif)
    
    And now we can run **bash** and all terminal looks like:
    
    ![updated terminal]({{ site.url }}{{ site.baseurl }}/assets/images/idea-bash/updated terminal.PNG)
    
    *some information private information hidded*
    
    **Note:** about how to customize your terminal I will speed in next posts, so stay tune!

### Questions?

Feel free to ask your questions via comment section bellow. 

---
Thank you for reading!
