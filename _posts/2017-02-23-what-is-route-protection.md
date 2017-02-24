---
layout: post
title: "What is route protection?"
categories: code
tags: code
---
 
When I searched this, I found a lot of mentions of Laravel and PHP. Whaat? What is this Laravel? And I certainly don’t want to start getting involved with PHP. Surely there is a simpler explanation for this!  

Well, now there is! Right here. On your very own screen.  
<!--more-->  

“Route protection” is a term referring to the idea that not all users should be able to access all parts of your app. These parts of your app are (theoretically) accessible at various *routes* or *URLs*. For example, perhaps users who are not logged in should be denied access to somewebsite.com/fancystuffyouhadtopayfor. Therefore, you may wish to either use a service/plugin, or add some simple logic to your app to "protect" any private or restricted routes.

I was so confuseded when someone said I should use "route protection"; I guess it's just one of those things that are so obvious to the people who know it that they don't see it as worth explaining. 

Also, I started writing this post many months ago. I should have just finished it when I started it! Anyway, here it is now. I hope it helps.  
