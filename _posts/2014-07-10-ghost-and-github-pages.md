---
layout: post
title: Ghost and Github Pages
date: '2014-07-11'
---

I had been searching for a fun way to learn new skills and centralize my online presence. Awhile ago I stumbled upon  [Ghost](http://ghost.org) which is a blogging platform built around the idea _that blogging platforms should be for blogging_. That's not a direct qoute but that's how I see the _Ghost_ platform. [Blah, blah show me the code!](#SetUpGithubPages)

Long story short I wanted to use Ghost but currently I'm on a tight budget. Though they have really cheap plans offered by the folks over at Ghost, I still couldn't take the plunge. That's where [Github Pages](http://pages.github.com) and a great article over at [LED Technica](http://ledtechnica.com/free-ghost-hosting-on-github-pages/) came in! Github pages is a simple way to host static pages for your Github repos. So I went over and created a Github page and then followed the instruction over at LED Technica to get Ghost working locally.

I ran into some minor road block mainly because the author over at LED Technica was on a Mac and I am a Linux (Ubuntu) user. So I decided to fill some of those gaps so hopefully someone finds this useful.

##Set Up Github Pages {#SetUpGithubPages}

If you do not have a Github account please go create one. Then follow their instructions on how to setup a [Github Page](http://pages.github.com). Also setup git on your local machine.

##Install Buster {#installbuster}

Buster is a tool which works to make Ghost run static pages. Which is needed for Github pages. So install Buster like so...

*If you don't have python-pip installed, install via the terminal like so...

~~~ bash

sudo apt-get install python-pip

~~~

*Now Buster

~~~ bash
pip install buster

~~~

##Setup Up Ghost (locally) {#setupupghostlocally}


*First, create 2 different folders in whatever directory you want (make sure they are both in the same directory). One folder is for Ghost and the other is for the _Buster Install_. Name the 2 folders whatever you want.

*Second, install NodeJS and NPM.

~~~ bash

sudo apt-get install nodejs

~~~

~~~ bash

sudo apt-get install npm

~~~

_Notice: you may need to restart you machine to get NodeJS to work properly. To check if NodeJS and NPM are installed properly run in the terminal `node -v` and `npm -v`

*Download the latest version of [Ghost](http://ghost.org/download) to the folder you created for Ghost and unzip it.

*Thirdly, after unzipping the Ghost archive change into the directory you have Ghost in.

~~~ bash

cd /path/to/ghost

~~~

*Fourth step is to install Ghost locally.

~~~ bash

npm install

~~~

_Notice:_ The article on LED Technica states to use `npm install` while the Ghost Docs state to use `npm install --production` but I ran both and saw no big drawbacks. There are technical reasons for using `npm install --production` if you want to read more on that I suggest you check out the [Ghost Docs](http://docs.ghost.org/installation/linux/)

*Fifth, run Ghost.

~~~

npm start

~~~

Go to this address [../index.html'>http://127.0.0.1:2368](../index.html'>http://127.0.0.1:2368) in a browser, to see your newly setup Ghost. The admin page is at [http://127.0.0.1:2368/ghost](http://127.0.0.1:2368/ghost)

_Notice:_ step 4 and 5 require you be in the directory of your Ghost unzipped file

####Setup Buster {#setupbuster}

*First, change into the directory you created earlier for the Buster Install. Once there run the following to setup Buster to your Github repo for your Github Page.

`buster setup` You should get a prompt to enter an address. Enter your Github Page repo like ([https://github.com/username/username.github.io.git](https://github.com/username/username.github.io.git))

Then...

`buster generate --domain=http://127.0.0.1:2368`

You should now have a folder named `static` in the folder you created for Buster. Change into that directory...

`cd /path/to/buster/static`

Now push everything that's in the `static` folder to Github.

`git add --all`

`git commit -m '[Enter witty comment]'`

`git push`

##Updating your Ghost/Github Page {#updatingyourghostgithubpage}

The one thing that I struggled with and the other authors left out was that you will have to run `buster generate --domain=http://127.0.0.1:2368` evertime you want to update your blog, that means everytime you write a new blog or change the layout run the `buster generate` command in the `static` directory. There may be an easier way to do this or maybe some settings tweaking will fix the problem but this is the solution I have for now. Also remember to push and commit after running the `buster generate` command.

##Enjoy! {#enjoy}

If I didn't screw up the instructions and you follow along diligently you should have a Github page running on Ghost!! Just give Github about 10 minutes to update your Github Page.

###Final Thought {#finalthoughts}

This was a really fun project and I hope you enjoy my tutorial. One of the coolest functions of this projects is the ability to test thing locally and then push them live to Github Pages. You can also publish all your blogs locally and when they are ready simply push them to Github and they'll be published. The total time to complete was about 40 minutes. Not bad considering I some road blocks to overcome. Well hope you enjoyed this tutorial.
