---
layout: post
title:  "TryHackMe OhSINT Writeup"
date:   2020-12-23
author: "Me"
---

OhSINT is a room on TryHackMe which revolves around using Open-Source Intelligence (OSINT) to find the answers to the challenges. OSINT is gathering information from publicly available sources. A large part of OSINT revovles around just being able to use Google, this means that you've probably performed a bit of OSINT before without realizing it. OSINT, however, is more than just googling as this room shows us.


# Getting Started

Our first challenge is to find the avatar of the user and all we are given is an image. When viewing the image there does not appear to be anything special about it. This is where we need our first tool, Exiftool. Exiftool can be downloaded on Windows and Mac, but I will be using it on Linux. To install Exiftool on Linux just open the terminal and run the following command:

```bash
sudo apt install exiftool
```

# Understanding Exiftool

Exiftool allows us to see metadata that is stored inside of images. This metadata can include things like the date the image was taken on, the model of camera used, the copyright, and more. This can show us a lot of information about an image that you cannot see by just opening the picture.

To use Exiftool, first download the image and then run the following command in the directory of the image.

```bash
exiftool WindowsXP.jpg
```

![Exiftool](/blog/images/TryHackMe-OhSINT-Writeup/exiftool-screenshot.png)

Running exiftool shows us some very interesting information about the image. What we are most interested in is the copyright because it shows us a name. Now that we have a name we can move Google to find more information.


# Googling

To find the answer to the first question we can try to google the name that we found from using Exiftool. When we do that we are presented with a few interesting links.

![OWoodflint-Google](/blog/images/TryHackMe-OhSINT-Writeup/owoodflint-google-search.png)

Googling the name found in the copyright reveals links to a twitter account, github account, and a WordPress website. To find the avatar and complete the first challenge we will look first at the twitter account.

![OWoodflint-twitter](/blog/images/TryHackMe-OhSINT-Writeup/twitter.png)

When we open up the Twitter account we can see what his avatar is.


# Locating the User

Our next task is to determine what city the user is from. Looking at the user's GitHub repository we can see that the user tells us they are from London. 

![OWoodflint-github](/blog/images/TryHackMe-OhSINT-Writeup/github1.jpg)


# Finding the SSID of the WAP

To solve the next challenge we need to find the SSID of the user. SSID stands for Service Set Identifier and is essentially your network's name. 

If we go back to the Twitter account we see a post that says something about a BSSID.

![BSSID-Twitter](/blog/images/TryHackMe-OhSINT-Writeup/bssid.png)

BSSID stands for Basic Service Set Identifier and is the MAC address of the Wireless Access Point (WAP). This means we can use the BSSID provided in the tweet to find the SSID that we are looking for. After doing some googling, we find a website called <a href="https://wigle.net/" target="_blank">Wigle.net</a> which we can use to find the SSID.

![Wigle-home](/blog/images/TryHackMe-OhSINT-Writeup/wigle1.png)

All we need to do is copy and paste the BSSID found in the tweet, paste it into the BSSID field, and click on filter. Once we do that all the crazy colors go away and we can see only one area left that has any color left on it (Note: This can also be used to determine the city of the user).

![Wigle-filter](/blog/images/TryHackMe-OhSINT-Writeup/wigle2.png)

Now that we have the location we just need to keep zooming in and eventually we can see the SSID, which is what we are looking for.

![Wigle-zoomin](/blog/images/TryHackMe-OhSINT-Writeup/wigle3.png)


# Finding the User's Email

The next challenge is to find the user's email. This is pretty easy since we have already found his GitHub account and you may have noticed that there was an email address listed in the repository.

![OWoodflint-github](/blog/images/TryHackMe-OhSINT-Writeup/github2.jpg)

So now we also have the user's email address and we can answer the next question as well by saying that we found the email address on the GitHub website.


# Where Did He Go on Holiday?

So far we have not seen anything about where he has gone on holiday. Now would be a good time to check the last site that we haven't visited yet, his blog. When we go to his blog, we are greeted with a post right away that says he is in New York right now and since we know he lives in London it is pretty easy to guess that this is where he spent his holiday.

![OWoodflint-blog](/blog/images/TryHackMe-OhSINT-Writeup/blog.png)


# Finding the Password

Now for the last challenge we have to find the user's password. Since we have already looked at the other sites and haven't seen anything related to a password, we should probably look around his blog a bit more. After clicking around the links on the website we don't find anything interesting, it's basically just the home page, a single post, and the contact page. It may feel like we've hit a dead end, but let's try one more thing. By opening up the browser's developer console (Pressing F12 usually does this) and clicking element inspector icon, which is on the top left corner of the console, we can see that there is an interesting looking element in the middle of the screen.

![Blog-developerconsole](/blog/images/TryHackMe-OhSINT-Writeup/console-window.png)

if we click on it we can see in the console what it is and we see that there is some text there. We can even change the color of it so that it will appear on the website itself. 

![Blog-password](/blog/images/TryHackMe-OhSINT-Writeup/console-window2(2).png)

Putting this string in as our answer for the last challenge confirms that this is the passowrd and we've completed the room!


# Conclusion

The OhSINT room on TryHackMe is a fun and fairly simple challenge to learn more of what OSINT is about. The challenges also help reveal how it can be easy to find out information about people without needing to do any hacking, but rather by using the information that they provide to us. Hopefully you keep that in mind when putting information out onto the internet. 