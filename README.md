<h1 align="center">Hi 👋, I'm Sai Venkat</h1>
<h3 align="center">A passionate AI and Data engineering Student from India</h3>

<p align="left"> <img src="https://komarev.com/ghpvc/?username=saiiexd&label=Profile%20views&color=0e75b6&style=flat" alt="saiiexd" /> </p>

<p align="left"> <a href="https://github.com/ryo-ma/github-profile-trophy"><img src="https://github-profile-trophy.vercel.app/?username=saiiexd" alt="saiiexd" /></a> </p>

- 🔭 I’m currently working on [Boot Camp](https://github.com/saiiexd?tab=repositories)

- 🌱 I’m currently learning **AI and DATA enginnering**

- 🫱🏻‍🫲🏻 I’m looking to collaborate on **AI and Data Related Projects**

- 🤗 I’m looking for help with **AI based projects which would be helpful for my career**

- 👨‍💻 All of my projects are available at [https://github.com/saiiexd?tab=repositories](https://github.com/saiiexd?tab=repositories)

- 📝 I regularly give updats about my official life on [https://www.linkedin.com/in/sai-venkat-7a2843218/](https://www.linkedin.com/in/sai-venkat-7a2843218/)

- 📫 How to reach me **saivenkat262005@gmail.com**

- 📄 Know about my experiences [https://drive.google.com/file/d/1UZpN0qoNTWJvWAgu5us5cryR-lGSO4rY/view?usp=sharing](https://drive.google.com/file/d/1UZpN0qoNTWJvWAgu5us5cryR-lGSO4rY/view?usp=sharing)

- ⚡ Fun fact **I love to take random pics...**



Skip to content
DEV Community
Search...
Powered by  Algolia
Log in
Create account

45
Jump to Comments
79
Save

Cover image for How to enable GitHub Actions on your Profile README for a snake-eating contribution graph 🐍
Michelle Mannering
Michelle Mannering
Posted on Jun 28, 2021 • Updated on Feb 22


102

11
How to enable GitHub Actions on your Profile README for a snake-eating contribution graph 🐍
#
github
#
markdown
#
opensource
#
tutorial
GitHub Like a Boss (23 Part Series)
1
GitHub Desktop for better repo organisation
2
GitHub README for easy to understand code
...
19 more parts...
22
How to automatically close your issues once you merge a PR
23
How to get more suggestions from GitHub Copilot
Lots of people have been talking about and sharing their GitHub Profile READMEs. It was a project we launched last year and developers are loving it. As this feature became more popular, more developers were building templates, plugins, and apps for you to use on your profile.

This post from Supritha caught my attention. I like the way they talk about various pieces of code to have on your GitHub Profile README.

supritha 
How to have an awesome GitHub profile ?
Supritha ・ Jun 8 '21
#github #markdown #opensource #git
One of the comments from Guillaume Falourd pointed to a really cool project from Platane.

GitHub logo Platane / snk
🟩⬜ Generates a snake game from a github user contributions graph and output a screen capture as animated svg or gif
snk
GitHub Workflow Status GitHub release GitHub marketplace type definitions code style

Generates a snake game from a github user contributions graph

github contribution grid snake animation
Pull a github user's contribution graph Make it a snake Game, generate a snake path where the cells get eaten in an orderly fashion.

Generate a gif or svg image.

Available as github action. It can automatically generate a new image each day. Which makes for great github profile readme

Usage
github action

- uses: Platane/snk@v3
  with
    # github user name to read the contribution graph from (**required**)
    # using action context var `github.repository_owner` or specified user
    github_user_name: ${{ github.repository_owner }}

    # list of files to generate.
    # one file per line. Each output can be customized with options as query string.
    #
    #  supported options:
    #  - palette:     A preset of color, one of [github, github-dark, github-light]
    #  - color_snake: Color of the snake
    #  - color_dots:  Coma separated list of dots color.
    #                 The
…
View on GitHub
It uses GitHub Actions to build and update a user's contribution graph, and then has a snake eat all your contributions. The output generates a gif file, that you can then show on your GitHub Profile README. I thought this was pretty cool, so I set about adding this to my profile.

Step 1. Setting up GitHub Actions
The first thing you want to ensure before you start this project, is that you have GitHub Actions setup. Head to your Profile and ensure the "Actions" tab is available. Everyone should now have this feature.

Actions Button

Next, head to Platane's Action (available on the Marketplace) and make a copy of the code. You'll need to add this snippet into your Actions file.

Step 2. Creating your GitHub Actions yaml file
This is definitely the part where I got stuck. When looking at the code, I wasn't sure exactly where to add the lines of code mentioned on Marketplace.

After looking at the way Platane had their Actions file setup, I was able to generate the code (with a little help from Bdougie of course).

I've added the full code snippet below and added plenty of comments to (hopefully make it easy to understand).

You can copy this code straight into a blank *.yml file. Make sure you create a new *.yml file under the following directory:

YOUR_GITHUB_USERNAME --> .github --> workflows --> FILE_NAME.yml

# GitHub Action for generating a contribution graph with a snake eating your contributions.

name: Generate Snake

# Controls when the action will run. This action runs every 6 hours.

on:
  schedule:
      # every 6 hours
    - cron: "0 */6 * * *"

# This command allows us to run the Action automatically from the Actions tab.
  workflow_dispatch:

# The sequence of runs in this workflow:
jobs:
  # This workflow contains a single job called "build"
  build:
    # The type of runner that the job will run on
    runs-on: ubuntu-latest

    # Steps represent a sequence of tasks that will be executed as part of the job
    steps:

    # Checks repo under $GITHUB_WORKSHOP, so your job can access it
      - uses: actions/checkout@v2

    # Generates the snake  
      - uses: Platane/snk@master
        id: snake-gif
        with:
          github_user_name: mishmanners
          # these next 2 lines generate the files on a branch called "output". This keeps the main branch from cluttering up.
          gif_out_path: dist/github-contribution-grid-snake.gif
          svg_out_path: dist/github-contribution-grid-snake.svg

     # show the status of the build. Makes it easier for debugging (if there's any issues).
      - run: git status

      # Push the changes
      - name: Push changes
        uses: ad-m/github-push-action@master
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          branch: master
          force: true

      - uses: crazy-max/ghaction-github-pages@v2.1.3
        with:
          # the output branch we mentioned above
          target_branch: output
          build_dir: dist
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
Step 3. Running GitHub Actions
Once you've added the code above and committed it, head to your Actions tab. Under "Workflows", you should see the "Generate Snake" Action you created above.

Workflow

Click "Run Workflow" and watch your workflow run. You can even expand your workflow and see what's happening in real time.

Running Actions

Once your workflow has finished running, you should have received a green ✅ "build" checkmark. If so, this means your Action is working nicely. If not, you'll have to go through the code and see where the errors are. The "build" status should give you some indication of where your errors lie. You can also compare it with the yaml file on my GitHub Profile README.

Build green

If you have the green "build" ✅ then you should be good to go. Head back to your repo's "Code" directory, and change your branch to "output". Here you'll find the two versions of your contribution graph with the snake eating it: a *.gif and a *.svg. The good thing about these files is the file is rewritten over itself every time the Action runs. Thus, your contribution graph will always be up to date.

CodeOutput

Step 4. Adding a contribution-eating snake to your profile
Now the final set is to add this to your profile. Grab the file location of your *.gif. It should be something like:

https://github.com/YOUR_USERNAME/YOUR_USERNAME/blob/output/github-contribution-grid-snake.gif

Add this to your profile by copying the file location onto a new line in your markdown file:

![snake gif](https://github.com/YOUR_USERNAME/YOUR_USERNAME/blob/output/github-contribution-grid-snake.gif)

Now you should have a really cool looking snake eating your contributions!

GIFSnakeEating
Hopefully this guide was helpful for you and gives you a good base to start building and adding more Actions on your profile. What have you been adding to your GitHub Profile README? Other there other cool Actions I should be looking at?

GitHub Like a Boss (23 Part Series)
1
GitHub Desktop for better repo organisation
2
GitHub README for easy to understand code
...
19 more parts...
22
How to automatically close your issues once you merge a PR
23
How to get more suggestions from GitHub Copilot
👋 Before you go

Do your career a big favor. Join DEV. (The website you're on right now)

It takes one minute, it's free, and is worth it for your career.

Get started

Community matters

Top comments (45)
Subscribe
pic
Add to the discussion
 
 
kushaltanna24 profile image
KushalTanna24
•
Jun 3 '22

Work fine.! Here is mine - KushalTanna24


2
 likes
Like
Reply
 
 
msoftware profile image
Michael jentsch
•
Jul 9 '21

Great tutorial! Works for me.
Here's mine - github.com/msoftware


2
 likes
Like
Reply
 
 
gustavohgmartins profile image
Gustavo Martins
•
Sep 3 '21

Awsome! How did you change the background-color?


Like
Reply
 
 
mishmanners profile image
Michelle Mannering 
•
Sep 3 '21

I didn't change the background colour. It's a .png/.svg image, meaning the background is transparent. It looks dark on mine because I have dark mode enabled on GitHub. You can enable it by clicking Settings --> Appearance --> choosing your mode.


1
 like
Like
Thread
 
gustavohgmartins profile image
Gustavo Martins
•
Sep 3 '21

Thank you! actually I was using the .gif version, so I switched to the .svg one and voilá


2
 likes
Like
Thread
 
mishmanners profile image
Michelle Mannering 
•
Sep 3 '21

Awesome! Glad to be of help 😄


1
 like
Like
Reply
 
 
1seamy profile image
seamy
•
Apr 9 • Edited on Apr 9

Thanks Michelle Mannering for your (dev.to/mishmanners/how-to-enable-g...) topic.

If other users wish to utilize this, kindly modify this topic;

1-Use this (thanks Platane github.com/Platane) file;

github.com/1SeaMy/1SeaMy/blob/main...

2-Use this;

![snake gif](https://github.com/YOUR_USERNAME/YOUR_USERNAME/blob/output/github-contribution-grid-snake-dark.svg)

or this

![snake gif](https://github.com/YOUR_USERNAME/YOUR_USERNAME/blob/output/github-contribution-grid-snake.svg)


3
 likes
Like
Reply
 
 
l3on06 profile image
L3on06
•
Feb 17 '23

Hello everyone, before proceeding, please navigate to 'Settings -> Actions -> General -> Workflow Permissions' in your README.md file and select 'Read and Write' permissions. This is important because if you have other permissions selected, it may result in errors.


2
 likes
Like
Reply
 
 
unknow0074 profile image
Unknown
•
May 27

I specially created account to say thank you I almost wasted my 3 hours in wandering around to fix issue and you saved my life.** Thank you**


Like
Reply
 
 
jakon62508112 profile image
Jakob
•
Jan 21 '23 • Edited on Jan 21

Getting this error when I try to run workflow and after it fails:
Node.js 12 actions are deprecated. Please update the following actions to use Node.js 16: actions/checkout@v2. For more information see: https://github.blog/changelog/2022-09-22-github-actions-all-actions-will-begin-running-on-node16-instead-of-node12/.


1
 like
Like
Reply
 
 
shuvajitgeek profile image
Shuvajit
•
Feb 2 '23

go to setting -> action -> and check the workflow as read and write
(run build again) hopefully you will not get error


Like
Reply
 
 
baayah profile image
BAayah
•
Jan 11

Thank you so much! I tried to change in settings 'Contribution' to 'include my private contributions' but anyway my Readme profile doesn't show My Contributions. Could you please help me with this?
github.com/BAayah/BAayah


Like
Reply
 
 
hoangtien_2k3 profile image
Hoàng Anh Tiến
•
Jan 29 '23 • Edited on Jan 29

It does not work for me. Can someone help me?
github.com/hoangtien2k3qx1

Error:
actions/checkout@v2, platane/snk@master, ad-m/github-push-action@master, and crazy-max/ghaction-github-pages@v2.1.3 are not allowed to be used in hoangtien2k3qx1/hoangtien2k3qx1. Actions in this workflow must be: within a repository owned by hoangtien2k3qx1.


1
 like
Like
Reply
 
 
bpolaswar profile image
Bhumesh Polaswar
•
Oct 17 '21

Great worked for me. See github.com/bpolaswar
Thanks.


2
 likes
Like
Reply
 
 
ariafatah profile image
Ariafatah
•
Sep 18 '23

github.com/ariafatah0711/ariafatah...

Check my github and change this file because I tried the file above only as much as it appears if you want to use the full one I use. And don't forget to give stars😁


1
 like
Like
Reply
 
 
ariafatah profile image
Ariafatah
•
Sep 18 '23

If the action fails, you can go to repo settings, click action, then select general, and look at the bottom where it says read and write.


1
 like
Like
Reply
 
 
phuocantd profile image
An Nguyễn Phước
•
Jun 1 '22

How to auto build workflow everyday


1
 like
Like
Reply
 
 
eajahmed profile image
EAJUDDIN AHMED
•
May 31

Why is my snake not running?
Please Help Me 😢😭😭
Here is my Repo
github.com/eajahmed/EAJAHMED


1
 like
Like
Reply
 
 
1seamy profile image
seamy
•
Apr 9

Thanks :)


1
 like
Like
Reply
 
 
hudsonpufferfish profile image
Hudson Nguyen
•
Jan 8 '23

I got error "The process '/usr/bin/git' failed with exit code 128". Even though I tried to generate a new personal access token, it didn't work. Anybody has any solution for this error?


1
 like
Like
Reply
 
 
nirav97120 profile image
Nirav Prajapati
•
Dec 16 '22

Awesome My github profile


2
 likes
Like
Reply
 
 
wkylin profile image
wkylin.w
•
May 1 '22

Here's mine:
github.com/wkylin


1
 like
Like
Reply
View full discussion (45 comments)
Code of Conduct • Report abuse
profile
Pieces.app
PROMOTED

A Workflow Copilot. Tailored to You.
Pieces.app image

Our desktop app, with its intelligent copilot, streamlines coding by generating snippets, extracting code from screenshots, and accelerating problem-solving.

Read the docs

Read next
dk119819 profile image
Beginner's Guide to Clocks: Understanding the Essentials
Deepak Kumar - Jun 8

g_venkatasandeepreddy_b profile image
Overview of GitHub Enterprise
G Venkata Sandeep Reddy - Jun 7

yogameleniawan profile image
Penerapan Domain-Driven Design dan CQRS Pattern di Golang untuk Pemula
Yoga Meleniawan Pamungkas - Jun 7

keploy profile image
Understanding GitHub Webhooks
keploy - Jun 6


Michelle Mannering
Follow
Developer Advocate | Hackathon Queen | International Speaker
LOCATION
Australia
EDUCATION
The University of Melbourne
WORK
Developer Advocate at GitHub
JOINED
May 29, 2020
More from Michelle Mannering
Release Radar · May 2024: Major updates from the open source community
#github #community #news #developers
Release Radar · April 2024 Edition: Major updates from the open source community
#github #developers #news #community
Release Radar • March 2024 Edition
#github #opensource #community #news
DEV Community

Become a Moderator!
Check out this survey and help us moderate our community by becoming a tag moderator here at DEV.


# GitHub Action for generating a contribution graph with a snake eating your contributions.

name: Generate Snake

# Controls when the action will run. This action runs every 6 hours.

on:
  schedule:
      # every 6 hours
    - cron: "0 */6 * * *"

# This command allows us to run the Action automatically from the Actions tab.
  workflow_dispatch:

# The sequence of runs in this workflow:
jobs:
  # This workflow contains a single job called "build"
  build:
    # The type of runner that the job will run on
    runs-on: ubuntu-latest

    # Steps represent a sequence of tasks that will be executed as part of the job
    steps:

    # Checks repo under $GITHUB_WORKSHOP, so your job can access it
      - uses: actions/checkout@v2

    # Generates the snake  
      - uses: Platane/snk@master
        id: snake-gif
        with:
          github_user_name: mishmanners
          # these next 2 lines generate the files on a branch called "output". This keeps the main branch from cluttering up.
          gif_out_path: dist/github-contribution-grid-snake.gif
          svg_out_path: dist/github-contribution-grid-snake.svg

     # show the status of the build. Makes it easier for debugging (if there's any issues).
      - run: git status

      # Push the changes
      - name: Push changes
        uses: ad-m/github-push-action@master
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          branch: master
          force: true

      - uses: crazy-max/ghaction-github-pages@v2.1.3
        with:
          # the output branch we mentioned above
          target_branch: output
          build_dir: dist
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
DEV Community — A constructive and inclusive social network for software developers. With you every step of your journey.

Home
Podcasts
Videos
Tags
DEV Help
Forem Shop
Advertise on DEV
DEV Challenges
DEV Showcase
About
Contact
Guides
Software comparisons
Code of Conduct
Privacy Policy
Terms of use
Built on Forem — the open source software that powers DEV and other inclusive communities.

Made with love and Ruby on Rails. DEV Community © 2016 - 2024.


<h3 align="left">Connect with me:</h3>
<p align="left">
<a href="https://twitter.com/its_sai_ra" target="blank"><img align="center" src="https://raw.githubusercontent.com/rahuldkjain/github-profile-readme-generator/master/src/images/icons/Social/twitter.svg" alt="its_sai_ra" height="30" width="40" /></a>
<a href="https://linkedin.com/in/https://www.linkedin.com/in/sai-venkat-7a2843218/" target="blank"><img align="center" src="https://raw.githubusercontent.com/rahuldkjain/github-profile-readme-generator/master/src/images/icons/Social/linked-in-alt.svg" alt="https://www.linkedin.com/in/sai-venkat-7a2843218/" height="30" width="40" /></a>
<a href="https://instagram.com/_saiie.xd_" target="blank"><img align="center" src="https://raw.githubusercontent.com/rahuldkjain/github-profile-readme-generator/master/src/images/icons/Social/instagram.svg" alt="_saiie.xd_" height="30" width="40" /></a>
<a href="https://www.codechef.com/users/saiiexd" target="blank"><img align="center" src="https://cdn.jsdelivr.net/npm/simple-icons@3.1.0/icons/codechef.svg" alt="saiiexd" height="30" width="40" /></a>
<a href="https://www.hackerrank.com/saivenkat262005" target="blank"><img align="center" src="https://raw.githubusercontent.com/rahuldkjain/github-profile-readme-generator/master/src/images/icons/Social/hackerrank.svg" alt="saivenkat262005" height="30" width="40" /></a>
<a href="https://www.leetcode.com/saivenkat262005" target="blank"><img align="center" src="https://raw.githubusercontent.com/rahuldkjain/github-profile-readme-generator/master/src/images/icons/Social/leet-code.svg" alt="saivenkat262005" height="30" width="40" /></a>
<a href="https://www.hackerearth.com/saivenkat262005" target="blank"><img align="center" src="https://raw.githubusercontent.com/rahuldkjain/github-profile-readme-generator/master/src/images/icons/Social/hackerearth.svg" alt="saivenkat262005" height="30" width="40" /></a>
</p>

<h3 align="left">Languages and Tools:</h3>
<p align="left"> <a href="https://www.w3schools.com/css/" target="_blank" rel="noreferrer"> <img src="https://raw.githubusercontent.com/devicons/devicon/master/icons/css3/css3-original-wordmark.svg" alt="css3" width="40" height="40"/> </a> <a href="https://git-scm.com/" target="_blank" rel="noreferrer"> <img src="https://www.vectorlogo.zone/logos/git-scm/git-scm-icon.svg" alt="git" width="40" height="40"/> </a> <a href="https://www.w3.org/html/" target="_blank" rel="noreferrer"> <img src="https://raw.githubusercontent.com/devicons/devicon/master/icons/html5/html5-original-wordmark.svg" alt="html5" width="40" height="40"/> </a> <a href="https://www.adobe.com/in/products/illustrator.html" target="_blank" rel="noreferrer"> <img src="https://www.vectorlogo.zone/logos/adobe_illustrator/adobe_illustrator-icon.svg" alt="illustrator" width="40" height="40"/> </a> <a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript" target="_blank" rel="noreferrer"> <img src="https://raw.githubusercontent.com/devicons/devicon/master/icons/javascript/javascript-original.svg" alt="javascript" width="40" height="40"/> </a> <a href="https://www.microsoft.com/en-us/sql-server" target="_blank" rel="noreferrer"> <img src="https://www.svgrepo.com/show/303229/microsoft-sql-server-logo.svg" alt="mssql" width="40" height="40"/> </a> <a href="https://www.mysql.com/" target="_blank" rel="noreferrer"> <img src="https://raw.githubusercontent.com/devicons/devicon/master/icons/mysql/mysql-original-wordmark.svg" alt="mysql" width="40" height="40"/> </a> <a href="https://www.oracle.com/" target="_blank" rel="noreferrer"> <img src="https://raw.githubusercontent.com/devicons/devicon/master/icons/oracle/oracle-original.svg" alt="oracle" width="40" height="40"/> </a> <a href="https://www.python.org" target="_blank" rel="noreferrer"> <img src="https://raw.githubusercontent.com/devicons/devicon/master/icons/python/python-original.svg" alt="python" width="40" height="40"/> </a> </p>

<h3 align="left">Support:</h3>
<p><a href="https://www.buymeacoffee.com/https://www.buymeacoffee.com/saii.xd"> <img align="left" src="https://cdn.buymeacoffee.com/buttons/v2/default-yellow.png" height="50" width="210" alt="https://www.buymeacoffee.com/saii.xd" /></a><a href="https://ko-fi.com/https://ko-fi.com/saivenkat"> <img align="left" src="https://cdn.ko-fi.com/cdn/kofi3.png?v=3" height="50" width="210" alt="https://ko-fi.com/saivenkat" /></a></p><br><br>

<p><img align="left" src="https://github-readme-stats.vercel.app/api/top-langs?username=saiiexd&show_icons=true&locale=en&layout=compact" alt="saiiexd" /></p>

<p>&nbsp;<img align="center" src="https://github-readme-stats.vercel.app/api?username=saiiexd&show_icons=true&locale=en" alt="saiiexd" /></p>

<p><img align="center" src="https://github-readme-streak-stats.herokuapp.com/?user=saiiexd&" alt="saiiexd" /></p>
