---
layout: post
title: "Adventures with Mozilla Con't"
tags:
- code
- Mozilla
---

"Con't" is how I used to abbreviate "continued" when I took notes in HS/uni. The good old days. I could never use my laptop even in university (ie when I had one) to take notes in class because the instant I would get even remotely disinterested, I would be totally gone and only tune back in to be hella confused. Dis bad. 

Anyway, since deciding that continuing to explore & contribute to Mozilla would be better than trying to (frantically, because that's apparently the _only_ way) apply for other jobs, I have found myself much happier.

Today I set up [Docker](https://opensource.com/resources/what-docker)! <!--more--> Holy smokes finally the time has come! It always gets mentioned - even when I told Stanley I wanted to work on Blaggregator (RC's internal blog aggregator...don't you love that name!), he said:

> Oh, talk about a piece of cake! I added a Dockerfile when I was contributing so now it should be super easy to set up!

Bah humbug, I thought. Well, that's not quite true. I was just surprised that he thought that made it easier. I thought that made it harder - now I had to set up Docker _and_ the actual code. Docker just always seemed like this magical, elusive technology that advanced devs used, and I couldn't justify taking time to explore it myself because I never really _needed_ it.

Also, I think I like to do things the hard way. Sometimes. Sometimes I am trigger happy and I `apt-get install` like a..whatever, similies are hard. But then I balance that out by sometimes doing everything I can to avoid installing dependencies. For example, I would like to check out BMO, or bugzilla.mozilla.org, Mozilla's bug tracker. I first tried just getting it running. Then I relented and turned to Docker, though still resisted using [docker-compose](https://docs.docker.com/compose/overview/), a Docker container management tool. I sleuthed enough to figure out that the `docker-compose.yml` file contained some tidbits of tasty info, of which I (perhaps erroneously? I still don't know) decided the only important bits were this:


```yml
    ports:
      - "80:80"
      - "5900:5900"
```

Ah! This means I must bind the port on my machine (host!) to that on the container. I also decided 5900 was the only important one to bind...probably not true, in retrospect. However, the way I tried to get around this was to amend my command to run the container:

```bash
# Original:
(sudo) docker run bmo-dev

# New attempt:
(sudo) docker run -p=127.0.0.1:5900:5900 bmo-dev
```

I tried a few other syntaxes (syntacies?) but nothing seemed to be available in my browser :'( so I resorted to installing docker-compose.

Also, I aliased `docker` to `sudo docker` because it always needs to be run with root privledges and ain't nobody got time to type sudo every time. I decided this is sufficiently innocuous. Please educate me if I am wrong. Also by "time to type" I mean "brain power to remember to type" and "experience to understand that the error message has nothing to do with debugging docker and everything to do with needing MOAR POWER.".


Things I'm learning:

- In Perl, `$` identifies a scalar variable (string, num) while `@` identifies an array
- `127.0.0.1` is the magical built-in-to-my-computer Apache server!
- Patience.

Tune in next time to find out moar things about Mozilla and Outreachy! And maybe life! I talked to a couple mentors lately about their projects and it is making me super excited at the prospect of being fully involved in them! :))
