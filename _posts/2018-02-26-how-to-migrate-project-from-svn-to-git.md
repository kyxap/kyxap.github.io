---
title:  "How to migrate repo(s) from SVN to GIT"
categories: git svn how-to
comments: true
categories:
  - How to
tags:
  - git
  - svn
toc: true
---
Today I will speak about one of the challenges that I have faced on my current project and this is svn to git migration.

## Problem
Migrate all projects source code (I had more than 5 projects with 2+ years of active developming) from one version control to another and save all commits history if you offcourse need it :smile:. From my expirience SVN used as one repo for all projects without branches, tabs etc. Mostly becaouse those are QA projects. That's mean each folder in svn trunk will be our new project in git.

## Solution without history
Yes, you have a really fast way to do it: just copy all files and make one git init commit. But in this case you will lost all your history and it's to easy so this is definitely not my way.

## Solution with full SVN history

### Precondition
Below I will use bash only commands, so use **linux**, **mac** or **git-bash**, etc on **Windows**. You should have **git** and **svn** installed. Let's imagine that this is our svn project path: ```http://<svn.domain>:<port>/svn/trunk/<Project-Name>``` which we should migrate first.

### Step by step instructions

1. Check the SVN project code if you didn't do it before. Please use your full path to project
    ```sh
    $ svn co http://<svn.domain>:<port>/svn/trunk/<Project-Name>
    ```
    After that directory with your `<Project-Name>` should be created and message displayed:
    ```sh
    Checked out revision <revision-number>.
    ```

2. Go to created directory
    ```sh
    $ cd <Project-Name>
    ```
3. We need to get authors from all SVN commits. This script will check svn log and grab commits info which we will use for next migration steps.
    ```sh
    $ svn log -q | awk -F '|' '/^r/ {sub("^ ", "", $2); sub(" $", "", $2); print $2" = "$2" <"$2">"}' | sort -u > authors.txt
    ```
    
4. Now the **authors.txt** created based on your svn log information. We need edit this file to apply git style log which is slightly different that svn. Open file with your favorite editor and change each line as showed bellow:
    You shoud have something like this:
    ```sh
    rxkukharuk = rxkukharuk <rxkukharuk>
    rxtest = rxtest <rxtest>
    ```
    
    Need change to:
    ```sh
    rxkukharuk = Roman Kukharuk <roman.kukharuk@domain.com>
    rxtest = Romario Test <romario.test@domain.com>
    ```
    
    What means: **old-svn-user = Name Last-Name <your-git-email-adress>**. Be aware that your users in Git shoud have the same name and email.
    
    **Note**: Do not delete users from file. You may add email and name for only users that you need or have accounts in git. All other not edited user commits will display with old svn user names. And can be updated in feature, but better to add all now.

5. Go up and create empty folder to work in
    ```sh
    $ cd .. && mkdir git-svn-repo && cd git-svn-repo
    ```
    
6. Initialize empty git svn repo without metadata
    ```sh
    $ git svn init http://<svn.domain>:<port>/svn/trunk/<Project-Name>  --no-metadata
    ```
    You shoud get message like this:
    ```
    Initialized empty Git repository in <your-path>/git-svn-repo/.git/
    ```

7. Apply your updated **authors.txt** to current repo
    ```sh
    $ git config svn.authorsfile ../<Project-Name>/authors.txt
    ```
    
8. Fetch code to the repo **git-svn-repo**
    ```sh
    $ git svn fetch
    ```
    You shoud get message like: 
    ```
    This may take a while on large repositories
    ```
    And you need just to wait for couple minutes, maybe 20-30 it depends on how fast your *ssd* :smile:
    When all will completed you will get message:
    ```
    Checked out HEAD:
    http://<svn.domain>:<port>/svn/trunk/<Project-Name> <revision-num>
    ```

9. Go up and clone it to clean git repo
    ```sh
    $ cd ..
    $ git clone git-svn-repo git-repo
    ```
    You should get:
    ```
    Cloning into 'git-repo'...
    done.
    ```
    
10. Publish your migrated repo to remote
    **Note**: before those steps you need to create **empty** repository on your GitHub.
    
    After repo is created:
    ```sh
    $ cd git-repo
    $ git remote set-url origin git@github.com:<Organization>/repo-name.git
    $ git push --set-upstream origin master
    ``` 
    After last command you will get something like:
    ```
    remote: Resolving deltas: 100% (2437/2437), done.
    To github.com:<Organization>/repo-name.git
    * [new branch]      master -> master
    Branch 'master' set up to track remote branch 'master' from 'origin'.
    ```
    That's means that all done!
    
    **Note**: I used **ssh** you can use **https** link to your repo insted.  
    
12. Check your GitHub that all your commits history migrated!

**652** commits migrated, yay!

![Commits Nums]({{ site.url }}{{ site.baseurl }}/assets/images/svn-git-commits-nums.PNG)
{: .full}

History commits linked to my GitHub User:

![Commits History]({{ site.url }}{{ site.baseurl }}/assets/images/svn-git-commits-history.PNG)
{: .full}

Contributors tab shows that all your work done succesfully:

![Contributors]({{ site.url }}{{ site.baseurl }}/assets/images/svn-git-contr.PNG)
{: .full}
    
## Q&A

### Can I update commit author after applying authors.txt or pushing to the git?

Yes, you can update the project after push without authors file by using this script:
    
```sh
git filter-branch --commit-filter 'if [ "$GIT_AUTHOR_NAME" = "rxkukharuk" ]; then \
export GIT_AUTHOR_NAME="Roman Kukharuk";\
export GIT_AUTHOR_EMAIL=Roman.Kukharuk@domain.com;\
export GIT_COMMITTER_NAME="Roman Kukharuk";\
export GIT_COMMITTER_EMAIL=Roman.Kukharuk@domain.com;\
fi;\
git commit-tree "$@"'
```

### Other Questions?

Feel free to ask your questions via comment section bellow. 

---
Thank you for reading!
