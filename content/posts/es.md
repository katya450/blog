+++
title = 'Lesson learned with Elasticsearch'
date = 2025-10-03
draft = false
+++

I made a bang, and completely crashed one of our test environments. It was surprisingly easy!

What I did, was make a rather simple database migration which touched 900+ Datomic entities, which each could have have 0-n entities under them, and those could also
have so many too many entities under them.<br>

Migration itself and the Datomic transactor: no problem, everything went well and got updated as planned.<br>

After that, ES started its work and began indexing all of these new changes. And then runs out of heap memory, big time.<br>

Then the server crashes and restarts as it's told to do, but what does it do when it restarts? It wants to index the ES stuff...<br>

What happens when it wants to index all the stuff it run OOM with the last time it tried to do that?
It crashes. And restarts. And crashes XD

---
Lesson learned:<br>
Whenever making a migration that *feels slightly bigger* that just touching one or two rows: consider whether it really needs to be indexed in ES right away - or maybe not?
