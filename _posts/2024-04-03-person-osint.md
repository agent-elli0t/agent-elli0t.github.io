---
title: OSINT In-Person Investigation Guide
date: 
categories: [OSINT]
tags: [OSINT, INVESTIGATION]
author: 
---

# OSINT In-Person Investigation Guide

Open-source intelligence (OSINT) is a powerful technique for obtaining personal data, with applications spanning from digital forensics and cybersecurity to security and investigations. The nuances of OSINT in-person investigation will be thoroughly covered in this extensive guide, which covers reconnaissance, Google Dorking, Image OSINT, examining previous account pages, and a variety of tools and online resources.

## OSINT Reconnaissance: Identifying Social Media Accounts

### Platforms to Investigate
- [Facebook](https://www.facebook.com/)
- [GitHub](https://github.com/)
- [Instagram](https://www.instagram.com/)
- [Reddit](https://www.reddit.com/)
- [Threads](https://www.threads.net/)
- [TikTok](https://www.tiktok.com/)
- [X (formerly Twitter)](https://twitter.com/)
- [YouTube](https://www.youtube.com/)

### Google Dorking for a Precise Search
**Basic searches**
```
"john smith" facebook
"john smith" github
"john smith" instagram
"john smith" reddit
```

**Including usernames**
```
"jsmith" facebook
"jsmith" reddit
"jsmith" twitter
```

**Adding the year of birth**
```
"john1999" facebook
"john1999" github
```

#### Examples
Execute these searches on respective platforms:
- Facebook: `"john1999" site:facebook.com`
- Twitter: `"j.smith" site:twitter.com`

## Image OSINT: Decoding Account Pictures

When the target person uses pictures, Image OSINT unveils a treasure trove of information:
- **Brand of Clothes:** Leverage reverse image searches on [Google Images](https://www.google.com/imghp) to identify clothing brands.
- **Scenery Reflection:** Scrutinize images for reflections revealing surroundings, providing clues about locations.
- **Numerical Values:** Examine images for numerical data, such as dates, reflected in surfaces.

### Tools for Image OSINT
- [Google Images](https://www.google.com/imghp): For reverse image searches.
- [TinEye](https://tineye.com/) and [Yandex](https://yandex.com/images/): Additional engines for reverse image searches.

#### Examples
- Upload the profile picture to [Google Images](https://www.google.com/imghp) for associated information.
- Use [TinEye](https://tineye.com/) to search for instances of the image on the internet.

## WayBackMachine: Delving into Older Account Pages

### Searching Historical Reddit and Twitter Pages
Explore the past using WayBackMachine:
- Reddit:
  - [http://old.reddit.com/user/<username>](http://old.reddit.com/user/<username>)
  - [https://www.reddit.com/user/<username>](https://www.reddit.com/user/<username>)
- Twitter:
  - [https://twitter.com/<username>](https://twitter.com/<username>)

### Examples
Input the provided Reddit and Twitter URLs into WayBackMachine. Look through old material to find out information from the past.

## A Wide Range of Tools and Platforms

### Tools
- [OSINT Framework](https://osintframework.com/): A repository of various OSINT tools and resources.
- [Shodan](https://www.shodan.io/): Identifies specific types of computers, aiding in asset identification.
- [theHarvester](https://github.com/laramies/theHarvester): Gathers information from diverse public sources.
- [SpiderFoot](https://www.spiderfoot.net/): Automates OSINT data collection and analysis.
- [IntelTechniques](https://inteltechniques.com/): Offers tools and resources by OSINT expert Michael Bazzell.
- [Maltego](https://www.maltego.com/): A powerful OSINT tool for link analysis and data visualization.

### Platforms
- [Pipl](https://pipl.com/): A people search engine aggregating information from various sources.
- [Spokeo](https://www.spokeo.com/): Organizes white pages listings, public records, and social network information.
- [PeekYou](https://www.peekyou.com/): Searches for people using online usernames.
- [Archive.org](https://archive.org/): A vast collection of archived web pages.
- [Echosec](https://www.echosec.net/): A geospatial OSINT tool for exploring social media content based on location.

## Your Online Presence

Keep a low online profile to protect your privacy. For OSINT activities, use virtual private networks (VPNs) and consider pseudonymous identities.

# Conclusion

Conducting person investigations using OSINT is a delicate art that necessitates the use of a variety of techniques and tools. Your actions should always be guided by ethical considerations and legal boundaries. Staying informed about new tools and methodologies is critical for success as the OSINT landscape evolves.
