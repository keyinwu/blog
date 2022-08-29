---
layout: post
title: "Summary of Gitalk Error: Validation Failed"
date:   2022-08-08
tags: [debug log]
comments: true
author: keyinwu
---

When setting up Gitalk, I came across `Gitalk Error: Validation Failed`, and the error message shown on the website 
is `Request failed with status code 422`. It took me a while to go through related issues on Gitalk repo. Here, I 
would like to summarize things I tried and summarize possible solutions that can help with this case.

<img src="https://github.com/keyinwu/blog/raw/main/images/Gitalk/validation_failed.jpeg" width="70%"/>


## 1. Account Intialization

First, you should check if you are logged in to your GitHub account on your blog site. It requires your login to initialize, 
and you should see a similar authorization window like this:

<img src="https://github.com/keyinwu/blog/raw/main/images/Gitalk/authorize.jpeg" width="55%"/>


## 2. Length of Title

Second, your post title might be too long. Gitalk will create issues under your repository to record comments and each issue id should be unique and its length should be less than 50. When you have a longer id, i.e. `location.pathname`, the process will fail.

In order to solve this problem, there are two ways: 1) truncate the id 2) use md5. The former has more readability but the latter might be more elegant.

I used the second method and simply changed id from `location.pathname` into `md5(location.pathname)`, meanwhile adding

```
<script type="text/javascript" src="https://cdn.jsdelivr.net/gh/gangdong/gangdong.github.io@dev/assets/js/md5.min.js"></script>
```

## 3. OAuth Setting

Finally, when filling in the `Authorization callback URL` in OAuth setting, it should be your blog site if you also have a main portfolio site. For example, it should be `https://www.keyinw.site/blog` instead of `https://www.keyinw.site`.