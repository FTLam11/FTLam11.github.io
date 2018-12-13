---
layout: post
title:  Yooooo String#scan pretty nice
date:   2018-12-13
tags: [ruby]
---
String#scan, Enumerable#sort_by, and reggies (regexps) are fuckin OP as shit. Was trying
to properly order a few hundred strings containing outline numbers and writing them to
a file.

{% highlight ruby %}

[[7, nil, "1 - PROJECT MANAGEMENT"],
[17, 7, "1.1 - Project Initiation"],
[22, 17, "1.1.1 - Client kick-off meeting - preparation and participation"],
[27, 7, "1.2 - Project Plan"],
[37, 27, "1.2.1 - Project management plan development"],
[42, 27, "1.2.2 - Project management plan maintenance (Please refer to C: Optional Service)"],
[47, 27, "1.2.3 - Communication plan development"],
[52, 27, "1.2.4 - Risk management and contingents plan development"],
[57, 27, "1.2.5 - Monitoring manual development"],
[62, 27, "1.2.6 - Patient recruitment / retention plan development"],
[32, 7, "1.3 - Client Communication"],
[262, 7, "1.4 - Status Reporting"],
[267, 7, "1.5 - Trip Visits"],
[272, 7, "1.6 - Training"],
[277, 7, "1.7 - Vendor Management"],
[282, 7, "1.8 - Investigational Product Management"],
[287, 7, "1.9 - Project Communication & Risk Mitigation"],
[127, nil, "10 - REGULATORY INSPECTION (Not including)"],
[447, 127, "10.1 - TFDA Inspection "],
[657, 447, "10.1.1 - Pre-inspection meeting with sponsor"],
[662, 447, "10.1.2 - Pilot run before TFDA inspection"]]

{% endhighlight %}

String comparisons wouldn't work here, and I experimented using sort_by
with some real gnarly reggies until I looked up String#scan.

{% highlight ruby %}
open("#{Rails.root}/temp.txt", 'w') do |f|
  ServiceItemMold.pluck(:id, :parent_id, :name).sort_by do |data|
    data[-1].scan(/(\d+)/).flatten.map(&:to_i).each do |line|
      f << line; f << ','; f << "\n"
    end
  end
end
{% endhighlight %}

Real clean yeaaaaaaaaa
