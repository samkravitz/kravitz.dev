---
layout: post
title: An oddity in ActiveRecord
date:   2025-12-10
categories: programming ruby
---
A colleague of mine recently came across an interesting and hard to track bug in our application. It had to do with some oddities in how ActiveRecord deals with associations among objects. Unlike other languages, I find oddities and quirks in ruby to be charming and exciting. Let's dive into the issue.

It starts with this class and association:

{% highlight ruby %}
class Report < ApplicationRecord
  belongs_to :report_file
end

class ReportFile < ApplicationRecord
  has_one :report
end
{% endhighlight %}

Simple enough, right? The unusual behavior came with this bit of code:

{% highlight ruby %}
report_file = ReportFile.create

ActiveRecord::Base.transaction do
  Report.create(report_file: report_file)
  raise ActiveRecord::Rollback
end

report_file.update(...)
{% endhighlight %}

With the transaction raising a rollback, one may expect that the `Report` is destroyed after the `raise`. We certainly did. However, we noticed that this is not the case! The `Report` persists because the update to `report_file` resurrects it since the `ReportFile` maintains it in memory and needs it to exist to satisfy the association. The way to make it behave as we *expected* is to use `ids` instead of objects:

{% highlight ruby %}
report_file = ReportFile.create

ActiveRecord::Base.transaction do
  Report.create(report_file_id: report_file.id)
  raise ActiveRecord::Rollback
end

report_file.update(...)
{% endhighlight %}

When written like this, the `Report` is not ressurected after the transaction and destroyed, as the association is not set when giving the `id` instead of an object. The more you know!
