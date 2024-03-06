---
title: An issue with Chirpy theme on TOC
author: chulmin
date: 2024-03-08 00:00:00 -0500
categories: [HOW TO START GITHUB BLOG for beginners, Setting up GitHub page]
tags: [Chirpy, TOC, Issue]
---

This guide is specifically for Windows 10 users.

![Alt Text](https://lh3.googleusercontent.com/fife/ALs6j_EvsjAIDOULtACWxIPD7_5JYv5S33P-VwcVmFIVSguOfJgXF4QCn_vtWEuRjBeIdzpjethSyh2wTENilttZGlaLKLiIDQQGBww1cjeCXhjXn6-LiPsLGs1AZF9kLCIu93i7asq1euyVhVpzj8gfduLFS0e6hlTXlyE1dHOTcF4kjMDOQM1zGRDuY0g7DRqEIQnN8JP8Uz7RFaRw6jHWPLBxVivYS9oJ1-g6cJKHHMPDlar-8cw2RFJtGcK1Xj9yHFiRPGRlf00jHw9BfJPHghYFqDN8NtVRhCpBy8Mx92TIXMJD8KhiOww-retqTyoGnE7fKIuU6rKxBX7rag-LY4ixRwfnEseC7ZxeAXLFA3AglM0O-j-_r35ifeEgXUWVV8SYtGPLrieWfmmcyf8Ns_oncoBGpKRfvL3WciGZlfZxqq-ifTioJdLRtuUiRNTgrZbVDtHe2oYP7M9-mg-khZtpYIzxUnS2DFeYtnF5BjHrpGm54Bh51BOgpty0V1ZDoo5NNvVCIWBT0fOyMSESvPeYN0hygbXMLj1kuWWYqaNspwM_QHz-_ezR6U17iog7ygHfMZdcRrkxurv-hJf7VOP_Ax9lqvPHGNEQxuSPYOrIADxAZl1IQCLj96zDDff6367zz_LFpC79T1yqglQqsGDF7fwXscY5LwdbZUXb6h7n2RX20dWP8R7DlNlpDHTlNd-PDG5Ayj7RKogg4eDvPPrztFpepDJ_uBwRKAmw3BTtpSYU1MxA4b1JPgE5wb0SjPCkonGC9ZTUJ3q7i0HEi4ThcKvjWIp3vS-SCrScJX5hgqFTOxUa57TBp4FoEzHo5Fn1xhJFo7bl_TTUF4R5EV6lInKwe4IYIhr8q9KpxPEbkf93j3d459O0H2uoIB0rA3e10m8HbLOTxiTFBmp1INKVrq-KTrW-HmfWfFzjDbA8JKP-vosrFB3zg_EHtGRZ68mI-oIAU00I-aV7uN69kuUAp5Xad7mHSMIHjWA3pe9JqSm3PQDRSXcJvRaC2wKa5quX5Ufq=w1292-h944)
*Figure 1. Nothing shows under the contents.*


After executing **bundle exec jekyll serve** on ruby prompt, I encountered some issues: the Table of Contents(TOC) didn't show up, as showin in Figure 1, and the website behaved abnormally on mobile phone environments. Users in the [forum](https://github.com/cotes2020/jekyll-theme-chirpy/issues/1090) suggested various solutions, but none of them directly resolved my issue.

The problem stemmed from certain JavaScript files not being generated in my directory, and the commands provided in the forum post didn't work on Windows 10:

```bash
npm install
NODE_ENV=production npx rollup -c --bundleConfigAsCjs
```
{: file='Start Command Prompt with Ruby'}


And ChatGPT resolved the issue. For Windows 10 users, you can run the following script:

```bash
set NODE_ENV=production && npx rollup -c --bundleConfigAsCjs
```
{: file='Start Command Prompt with Ruby'}


To be frank, I had executed numerous scripts beforhand. Therefore, I am not sure some other commands should be executed in advance (Probably, npm should be downloaded from <https://nodejs.org/en/download>). I hope this information helps others who may be facing similar challenges.


## reference
- forum - <https://github.com/cotes2020/jekyll-theme-chirpy/issues/1090>
- <https://nodejs.org/en/download>