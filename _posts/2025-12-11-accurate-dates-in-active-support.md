---
layout: post
title: Accurate Dates in ActiveSupport
date:   2025-12-11
categories: programming ruby
---
Why is dealing with dates and times so hard in programming? Well, one reason is because it's hard for us as humans too! Of course, across peoples and cultures we have many time, date and calendar systems, and there's one in particular I'd like to focus on in this article. 
The [Gregorian Calendar](https://en.wikipedia.org/wiki/Gregorian_calendar) went into effect in October of 1582, and it is still in use today. It replaced the Julian Calendar, and one of the major enhancements was a more accurate counting of leap years. Since leap years weren't being observed correctly, to keep continuity with the previous calendar, the dates of October 5th-14th, 1582 were skipped.

A lesser language might gloss over a *minor* detail like this. Ruby, or more accurately ActiveSupport, on the other hand, actually takes [this into consideration!](https://guides.rubyonrails.org/active_support_core_extensions.html#extensions-to-date-calculations)

{% highlight ruby %}
Date.new(1582, 10, 4).tomorrow
# => Fri, 15 Oct 1582
{% endhighlight %}

{% highlight ruby %}
Date.new(1582, 10, 5)
# Date::Error: invalid date
{% endhighlight %}

Just one of the many reasons ruby is such a pleasure to use.
