---
title: Making sense of Sentry messages
author: bart
type: post
date: -001-11-30T00:00:00+00:00
draft: true
url: /?p=70
materiaal:
  - hout
hoeveelheid_leuningen:
  - 2
categories:
  - Uncategorized

---
I really like the error logging tool Sentry &#8211; a lot. It&#8217;s a life saver, in many ways. If we only didn&#8217;t have so many different projects to tend to&#8230; with a lot of traffic. That means that if something goes wrong in a very rare occasion &#8211; those are still quite some occurrences in our case.

Properly analysing your logs across projects sets you off in a bit of a treasurehunt. I&#8217;ve put together a set of selectionqueries that will give quite some insight in message occurence.

### Above all: keep it clean!

If you&#8217;re not very carefull, you might end up with gigabytes of data spanning several months. Sentry is generous enough to help you clean it up by means of a management command. Keep it cronned!

<pre>0 2 * * * /path/to/python /path/to/sentry --config=/path/to/settings.py cleanup --days=7</pre>

## Getting a little more detail in your overview

### Messages per project

Whenever you&#8217;ve left Sentry for a while, or you&#8217;re cleaning up stuff, at a certain point you&#8217;ll want to see how many messages you have per project in order to see which project might need your most immediate attention:

<pre>SELECT 
    SUM(T1.`times_seen`) AS count, 
    T2.name, 
    T2.id
FROM
    `sentry_groupedmessage` T1,
    `sentry_project` T2
WHERE
     T1.`project_id` = T2.`id`
GROUP BY T1.project_id
ORDER BY count desc</pre>

### Amount of messages for a project, grouped by day

<figure id="attachment_73" aria-describedby="caption-attachment-73" style="width: 983px" class="wp-caption alignnone"><img decoding="async" loading="lazy" class="wp-image-73 size-full" src="http://www.bartstroeken.nl/wp-content/uploads/2015/04/Screen-Shot-2015-04-17-at-14.12.24.png" alt="The number of Sentry messages over time, on a team level." width="983" height="190" srcset="https://www.bartstroeken.nl/wp-content/uploads/2015/04/Screen-Shot-2015-04-17-at-14.12.24.png 983w, https://www.bartstroeken.nl/wp-content/uploads/2015/04/Screen-Shot-2015-04-17-at-14.12.24-300x58.png 300w" sizes="(max-width: 983px) 100vw, 983px" /><figcaption id="caption-attachment-73" class="wp-caption-text">The number of Sentry messages over time, on a team level.</figcaption></figure>

When in team view, you might see a certain raise in events on a certain moment. But&#8230; in which project is that? It would be awesome to have that graph on project level. Until that time, try this one. It&#8217;s veryvery slow, but it&#8217;ll work:

<pre>select 
   count(id), 
   `level`,
   cast(datetime as DATE) AS seen_at 
from 
   `sentry_message`
WHERE 
    project_id = 3
GROUP BY 
    `level`,
    `seen_at`
ORDER BY 
   seen_at DESC, 
   level DESC</pre>

&nbsp;