# Project 8 - Pentesting Live Targets

Time spent: 12 hours spent in total

> Objective: Identify vulnerabilities in three different versions of the Globitek website: blue, green, and red.

The six possible exploits are:
* Username Enumeration
* Insecure Direct Object Reference (IDOR)
* SQL Injection (SQLi)
* Cross-Site Scripting (XSS)
* Cross-Site Request Forgery (CSRF)
* Session Hijacking/Fixation

Each version of the site has been given two of the six vulnerabilities. (In other words,
 all six of the exploits should be assignable to one of the sites.)

## Blue

### Vulnerability #1: SQL injection
- The Blue site does not sanitize its data for retrieving salesman information. There's a lot of ways we could take advantage of this, for proof of concept, let's prove that we can cause the site to delay its response with a sleep command: %27%20OR%20SLEEP(5)=0--%27. Notice the developers were smart to filter the ' symbol, but not smart enough to filter its encoded equivalents. This does in fact cause a delay of 5 seconds. We have proven that we can exploit this vulnerability. Note that the site defaults back to Daron Burke's info page, even when Irene's info page is being displayed. A walkthrough can be found below:
<a href="https://imgur.com/z5OEPPb"><img src="https://i.imgur.com/z5OEPPb.gif" title="source: imgur.com" /></a>
	

### Vulnerability #2: Session Hijacking
- After some testing with the provided script we find that the blue site does not regenerate session its session Id. To demonstrate how dangerous this can be, we will use two browsers. On the left we have the unsuspecting "target" user, and on the right we have the attacker. Once the target has logged in, we simulate stealing the session ID using the provided script. We change the session ID in the attacker browser, and once we attempt to navigate to the main user page, we can see that the attacker now has the same access as the target user. A walkthrough can be found below:
<a href="https://imgur.com/kLmFbe4"><img src="https://i.imgur.com/kLmFbe4.gif" title="source: imgur.com" /></a>


## Green

### Vulnerability #1: Username Enumeration
- The problem here is that there are noticeable differences between a login attempt that is made with a non-existent username and a valid username where only a bad password has been given. This gives important information to somebody who may betrying to break into the sight because now they can identify valid usernames, and all that's left is to figure out the password for that user. A walkthrough can be found below:
<a href="https://imgur.com/b02GmGD"><img src="https://i.imgur.com/b02GmGD.gif" title="source: imgur.com" /></a>

### Vulnerability #2: Cross-Site-Scripting
- A common place to look for this type of vulnerability would be a place in the site where a user is able to submit to a form. When we play around with the "Contact" page on the green site, we find that inputs to "feedback" box is not being handled very well. Let's use the traditional "alert" XSS proof of concept. We will submit feedback with this line included: <IMG SRC="#" ONERROR="alert('Ryan XSS')"/> Now when we try to view feedback as an admin, we can see that the attack was successful. A walkthrough can be found below:
<a href="https://imgur.com/VjocAN0"><img src="https://i.imgur.com/VjocAN0.gif" title="source: imgur.com" /></a>
	


## Red

### Vulnerability #1: Insecure Direct Object Reference:
- In the "Find a Salesperson" page we are given a list of salespeople and links to more information about them. Clicking on one of the links brings up a new page with a URL like the following: https://35.202.170.54/red/public/salesperson.php?id=1. If we go through them all, we notice the salespeople are assigned ids 1 through 9. This is interesting, now we can try out some different ID number options and see what happens. What happens if we try ids 9 or 10? Well, we get information that we were not supposed to see. We find salespeople Testy McTesterson (id=10) and Lazy Lazyman(id=11), who we now have information about thanks to this vulnerability. A walkthrough can be found below:
<a href="https://imgur.com/vIWBb45"><img src="https://i.imgur.com/vIWBb45.gif" title="source: imgur.com" /></a>
	
	

### Vulnerability #2: Cross-Site Request Forgery
- First we will act as a regular website visitor, but we leave a comment that may seem like an innocent advertisement. Now, we log on as an admin, and take a look at the feedback page. If the admin is baited into activating the link, they will not receive any free candy, but a hard truth of life. The next time the admin looks at the salespeople page, they will see the damage that has been done by the malicious form. 
A walkthrough can be found below:
<a href="https://imgur.com/lF6b1vg"><img src="https://i.imgur.com/lF6b1vg.gif" title="source: imgur.com" /></a>



## Notes
- GIFs created with [LiceCap](http://www.cockos.com/licecap/).

- Describe any challenges encountered while doing the work:
At first I wasn't able to properly access the provided session hijacking script, once 
I got over that issue it was fairly smooth sailing, although there's always an amount of 
trial and error. Evidence of failed attempts can most likely be seen in some of these gifs.
