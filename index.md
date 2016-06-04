---
layout: default
title: Home
---

## Howdy, friend.

My name is Randall. I am a 20 year old student from a small town in Texas. A
sinner saved by grace. I currently reside in the Seattle area and work as a
Software Developer Intern for Microsoft.

During the academic year, I am a full-time student at Texas A&M University
in College Station, Texas, where I am majoring in Computer Science and
minoring in Music.

Professional interest areas: distributed computing, web-scale systems,
human-computer interaction, educational games, interactive/responsive media,
and music technology.

Noteworthy pastimes: music (listening,
performing, and arranging), coding, GNU/Linux, writing, video games (playing
and developing), and tabletop roleplaying.

## Developing software is what I do.

When I do, it typically ends up on my [Github](https://github.com/takmo).
I cannot promise greatness, but every project there brought me here. Everything
is licensed openly, so you can build on something if you want.

## I take pride in my work.

Today I work as a Software Developer Intern at Microsoft, helping to develop
enterprise business solution platforms in [#TheCloud]
(https://azure.microsoft.com).

Last summer I worked as a Software Developer Intern at [Epoch Labs]
(http://epochlabs.com), contributing to a high-performance distributed
NoSQL database with a whimsical love of [boats](https://raft.github.io/).

For the last four semesters, I have worked as a Student Technician
for the Engineering IT group at Texas A&M University, where I interact daily
with students, staff, and faculty to resolve technical mishaps and maintain
equipment.

## Sometimes, I write things.

I am not one for blogging, but sometimes I have something to say. Here are a
few of my latest posts.

<ul>
    {% for post in site.posts limit: 5 %}
        <li>{{ post.date | date_to_string }} &raquo;
        <a href="{{ site.url }}{{ post.url }}">{{ post.title }}</a></li>
    {% endfor %}
</ul>

[(Or you can click here to view all of my posts in order.)]({{site.url}}/posts.html)

## Want to chat with me?

Feel free to reach out to me on [Twitter] (https://twitter.com/imtakmo) or
send an email to my name at this domain.

