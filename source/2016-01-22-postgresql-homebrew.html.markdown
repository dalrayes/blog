---
title: PostgreSQL & Homebrew
date: 2016-01-22 00:50 UTC
tags: postgresql, rails, homebrew
published: true	
---


I think my battery died last night on my Macbook and when I started the rails app I was working on this morning I saw this error:


	PG::ConnectionBad (could not connect to server: No such file or directory
	Is the server running locally and accepting
	connections on Unix domain socket "/tmp/.s.PGSQL.5432"?

None of my rake commands were working and I couldn't load my site on localhost.

I looked up the error and found these links the most useful:

1. <a href="https://kkob.us/2016/01/09/homebrew-and-postgresql-9-5/" target="_blank">Keita's Blog</a>
2. <a href="https://coderwall.com/p/zf-fww/postgres-on-osx-with-homebrew-not-running-after-osx-crash" target="_blank">Coderwall</a>
3. <a href="http://stackoverflow.com/questions/19828385/pgconnectionbad-could-not-connect-to-server-connection-refused" target=_"blank">Stack Overflow</a>

I ended up finding that somehow, I had two versions of postgres under `usr/local/var` (postgres) and (postgres9.5)

After much frustration, I did the following:

	brew uninstall postgresql

and then:
		
	gem uninstall pg

I then deleted all postgres files in `usr/local/var`

Double checked there was no postgresql by running `brew list`, then ran:

	brew install postgresql

I ran `pg_ctl -D /usr/local/var/postgres -l /usr/local/var/postgres/server.log start`

I then checked that the server was running: 

```ruby
	$ ps x | grep postgres
	 5141   ??  Ss     0:00.00 postgres: checkpointer process
	 5142   ??  Ss     0:00.01 postgres: writer process
	 5143   ??  Ss     0:00.00 postgres: wal writer process
	 5144   ??  Ss     0:00.00 postgres: autovacuum launcher process
	 5145   ??  Ss     0:00.00 postgres: stats collector process
	 5133 s000  S      0:00.03 /usr/local/Cellar/postgresql/9.5.0/bin/postgres -D /usr/local/var/postgres
	 5161 s000  U+     0:00.00 grep postgres

```

Lastly, bundle install & Reboot rails!!


Side note: 
A fellow DBC alum recommended this command as well - you might find it useful:
`pg_ctl restart -D /usr/local/var/postgres -m fast `

This command kills postgres entirely, then restarts it…!


