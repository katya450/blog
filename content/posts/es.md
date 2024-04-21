+++
title = 'Lesson learned with Elasticsearch'
date = 2025-10-03
draft = false
+++

I made a bang, and completely crashed one of our test environments.

It was surprisingly easy!

I made a database migration which touched 900+ Datomic entities, which each could have have 0-n entities under them, and those could also
have so many too many entities under them.<br>
Migration and Datomic transactor, no problem, everything went well and got updated as planned.<br>
After that, ES started to work and index all of these new changes. Runs out of heap memory, big time.
<br>Server crashes and restarts as it's told to do, but what does it do when it restarts? It wants to index the ES stuff...
<br>What happens when it wants to index all the stuff it run OOM last time it tried to do that? <br>
It crashes. And restarts. And crashes XD

Lesson learned:<br>
Whenever making a migration that feels slightly bigger that just touching one or two rows: consider whether it really needs to be indexed in ES right away or maybe not?
