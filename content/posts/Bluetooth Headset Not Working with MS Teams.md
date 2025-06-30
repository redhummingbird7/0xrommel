---
# Common-Defined params
title: "Bluetooth Headset Not Working with MS Teams"
date: "2022-11-14"
description: "How-to fix: Bluetooth Headset Not Working with MS Teams"
categories:
  - "Troubleshooting"
tags:
  - "Technology"
  - "Software Engineering"
  - "MS Teams"
  - "Bluetooth"
# menu: main # Optional, add page to a menu. Options: main, side, footer

# Theme-Defined params
# thumbnail: "img/placeholder.png" # Thumbnail image
# lead: "Example lead - highlighted near the title" # Lead text
comments: false # Enable Disqus comments for specific page
authorbox: true # Enable authorbox for specific page
pager: true # Enable pager navigation (prev/next) for specific page
toc: false # Enable Table of Contents for specific page
mathjax: true # Enable MathJax for specific page
sidebar: "right" # Enable sidebar (on the right side) per page
widgets: # Enable sidebar widgets in given order per page
  - "search"
  - "recent"
  - "taglist"
---

![Bluetooth headset not working in teams](/img/become-good-software-developer.jpg "Bluetooth headset not working in teams")

### Hope this can help someone going through a similar issue.

## Issue :

I used **MS Teams** for meetings with colleagues and for some reason my Bluetooth headphone (Mpow H7) stopped working. I use it as speaker and there was just no sound coming out. It used to work before, so I'm not quite sure what went wrong. I'm suspecting some update could have change some setting or changed something.

Note that the headphone works perfectly well with Zoom and other applications. So, it isn't the hardware that has issues. It's just not working with MS Teams for some reason. **

Also, would like to note, the Bluetooth headphone creates a couple of headphone options to use (see below):

1. Headset (Mpow H7 Hands-Free AG Audio)
2. Headphones (Mpow H7 Stereo)

When I choose option 1 above, I can hear sound in MS Teams. So, that option works. But, I don't like using it as the sound quality is really bad. Sound is teeny-tiny and I could barely hear people in the meeting.

Option 2 is way better. So, I need to make it work.

## Solution :

Google is always your best friend when troubleshooting. My search, led me to this page:
https://answers.microsoft.com/en-us/msteams/forum/all/bluetooth-headset-not-working-with-teams/ceda5246-b818-4a79-b575-0c32dfcdb507?page=17

**To summarize the solution that worked for me:**

I unchecked the "Handsfree Telephone" Service from the list of Bluetooth Services available on my headphone device (Mpow H7). That's it.

## Step by Step : (with screenshots)

**1.** On the Search Box next to the Windows icon (lower left of the display screen), type Bluetooth and other devices.

![](/img/windows_button.PNG)

**2.** When in the Bluetooth and other devices panel, click on Devices and printers. It is under "Related Settings" to the right.

![](/img/Pasted_image_20221114172636.png)

**Alternate Way:** Control Panel\View Devices and Printers

![](/img/Pasted_image_20221114172838.png)

**3.** In Devices and Printers, right click on your Bluetooth device. Mine is the Mpow H7.

![](/img/Pasted_image_20221114172959.png)

**4.** Click on Properties and then the Services tab.

![](/img/Pasted_image_20221114173042.png)

**5.** Uncheck Handsfree Telephony to disable.

![](/img/Pasted_image_20221114173225.png)

**6.** Go to MS Teams and make a Test Call to check.

![](/img/Pasted_image_20221114173443.png)

**7.** That should be it!

Note: The solution above essentially removed the first option (Headset (Mpow H7 Hands-Free AG Audio) from your list of speakers to choose from. You will only see one option for your headphone and that's the "Headphones (Mpow H7 Stereo)".

This is totally fine with me as I don't like using the "Hands-Free" one because of the bad sound quality. If you need the "Hands-Free" option working then this solution is not for you. Check the thread in the link I shared above for other possible fixes.
