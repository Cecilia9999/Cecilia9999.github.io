---
title: Construct Your New Blog
date: 2020-04-15 21:22:00
categories:
  - website
tags:
  - hexo
  - github
---
# Hello, everyone!
Today, I try to create the first blog via **Hexo + GitHub**. This blog is for recording the process of it.
Hope that I make it simple and clear. Let's see it!

# Preparation

### Check the following packages:
* [Node.js](https://nodejs.org/en/)
* [Git](https://git-scm.com/downloads/)
* [Hexo](https://hexo.io/)

Before installing Hexo, the first two packages must be installed.

### Make Your User Page on Github
1) Log in your GitHub and create a new repository, which its name should be
> **\<YourAccountName\>.github.io**

Here,  _.github.io_  is a fixed suffix for User Page.

2) Choose a local site to clone your repository with SSH address
``` bash
$ git clone git@github.com:Name/Name.github.io.git Name
# git clone <remote repo> <local shortname>
```
In this local site you will have a empty local repository *Name* with default brach *master* and default *.git* directory (*.git* means there exists a repository).


### Install Node.js
You need use *npm* command to install *Hexo* which is based on *Node.js*. For MAC OS, you can download package from official website and install it just by a click. You will see the version if installed successfully by
``` bash
$ node -v
$ npm -v
```

### Install Hexo
``` bash
$ sudo npm install -g hexo-cli
```

# Construct Blog
Enter into your new local repository and construct a new directory ```blog/``` to save Hexo files. The construction of local directory is
```
local repo
  ├── blog
  └── .git  
```
### Initialize Hexo
``` bash
$ cd blog
$ hexo init
$ npm install  # Install
```
You will see some new documents generated:
```
  blog
   ├── _config.yml   # website configuration file
   ├── package.json
   ├── scaffolds     # Template directory
   ├── source        # Store user source
   |   ├── _drafts
   |   └── _posts
   └── themes        # Store the themes of website
```
Now, you can visit blog site locally after running Hexo server
``` bash
$ hexo server
```
Check it by searching [localhost:4000](http://localhost:4000).

### Make configurations for Hexo
Make configurations to relate local blog to your GitHub User Page

1) vim ```_config.yml``` (exists in local path ```blog/```), change the last part from default settings
``` bash
# Deployment
## Docs: http://hexo.io/docs/deployment.html
deploy:   
  type:
```
to
``` bash
# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: git
  repository: git@github.com:Name/Name.github.io.git
  branch: master
```
This configuration means everytime when you release blog, Hexo will generate a static *.html* file to remote site (your GitHub User Page) with branch of *master*.

2) Install a plugin and release your blog on website
``` bash
$ sudo npm install hexo-deployer-git --save
$ hexo clean
$ hexo generate
$ hexo deploy
```
Congratulations! Now you have a real blog on website. Check it by searching http://Name.github.io.

# Submit local Hexo files to GitHub
Everytime you release a blog, only *.html* file will be generated and submit to *master* branch in GitHub. The origin *.md* file cannot save to GitHub. Although you can use extra hard disk to save these original files...
Here, if you don't remind these files released on web, you can use a new branch **hexo** to store them.

1) Initialize an new Git repository under local hexo path
``` bash
$ git init  
```
A ```.git``` directory is generated, which includes some default files of a new repository.

2) Map local into remote repository
``` bash
$ git remote add origin git@github.com:Name/Name.github.io.git  
```
origin: default local repo shortname

3) Use a new branch **hexo** to store Hexo sources
``` bash
# create a new local brach and change into it
# check HEAD file in .git, the pointer now points to hexo
$ git checkout -b hexo

# add hexo files to index/stage
$ git add .            

# submit files in stage to current branch hexo
$ git commit -m "comment"   

# push local(origin) branch hexo to remote new branch hexo (1st time)
$ git push origin hexo:hexo
```
Note that push local branch to a non-existed/existed remote branch:
``` bash
$ git push origin <local b>:<remote b>
$ git push origin <remote b>
```
**Important**: Please change your **default** branch of **remote** repository as **hexo**. For this step, you need to go to your remote repository and set
> Settings -> Branches -> Default branch -> choose hexo -> Update

Now you can check your GitHub and there are two branch and default is hexo:
* master: save generated blog pages (.html)
* hexo: sources of Hexo blog files (.md)


# Manage Blog
Everytime you change your hexo files, please submit to **remote hexo branch** firstly
``` bash
$ git add .
$ git commit -m "comment"
$ git push origin hexo
```
Then release your blog again (automatically update remote master branch)
``` bash
$ hexo clean
$ hexo g
$ hexo d
```

## Change Themes for Your Blog

The default theme is _landscape_ and you can download other themes into ```blog/themes```. Here use the popular theme _next_ as an example.

1) Download theme packages
``` bash
$ git clone https://github.com/iissnan/hexo-theme-next.git themes/next
```
2) Change hexo configuration
 ``` bash
$ vim blog/_config.yml
```
Find keyword and change ``` theme: landscape ```  to  ``` theme: next ```.
You can also change some details as you like by _vim_ the theme's configuration file: ```theme/next/_config.yml```.

More info: [NexT theme](http://theme-next.iissnan.com/)

3) Details: *vim* ``` themes/next/_config.yml ``` 
``` bash
menu:
  home: /|| home
  about: /about/|| user
  tags: /tags/|| tags
  categories: /categories/|| th
  archives: /archives/|| archive
  #schedule: /schedule/|| calendar
  #sitemap: /sitemap.xml|| sitemap
  #commonweal: /404/|| heartbeat

# Enable/Disable menu icons.
menu_icons:
  enable: true			
```
**Importance**: no space between '/' and '||',  ' /xxx/|| icon '.
Then you need to create new pages for new-opened buttons (tags, categories, about)
``` bash
$ cd blog
$ hexo new page "tags"
$ hexo new page "categories"
$ hexo new page "about"
```
After creation, *vim* ``` blog/source/tags/index.md ```
```
---
title: tags
date: 2020-04-15 22:18:02
type: "tags"			# add this fixed sentence 
comments: false			# add
---
```
The similar settings for *about* and *categories*


## Post Your Blog
``` bash
$ cd blog
$ hexo new "PostName"
```
A new blog _PostName.md_ file is generated in ```blog/sources/_posts/```. You can also create a *.md* file directly under this path.

#### _Now, enjoy writing your blog : )_
