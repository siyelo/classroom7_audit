= Classroom7 - Devops

== General Architecture

=== Rails (App)

Rails v3.2
Linode (London)
Talks to Couch(Cloudant) over HTTP (not SSL)


=== Linode

- 1 user
 - not root
 - does everything


=== Amazon S3 (backups)
  -

=== Postgres (Persistence)

For Relational models
Users/Organizations/Courses/Lessons


=== CouchDB (persistence)

For documents - eg. Questions/Answers/Progress

Hosted on Cloudant (free up to 250mb)
* hosted in Holland
* 22ms response time (from London Linode)

Append-only (file): Admin has to (manually) compact it.
* Admin concern: Disk space!


=== IrisCouch (Couch backups)

Initialized from Linode


=== Cron (backup app for Couch)

* A cron (rake) job - 2am - replicate
* capistrano recreates
* connects to IrisCouch
* mails via Resque Mailer


=== Ruby backup (Postgres and Files)

* backup gem
* whenever gem
* S3 buckets for staging/prod
* backs up logs
* integrated into rails app

=== Exception Notifier (app monitoring)

* email

=== Unicorn

* 4 workers
* nginx -> app

=== nginx (routing)


=== Resque-mailer

- App mailer
- cron job mailer
- Connects to MandrillApp (via SMTP)

=== Linode (Hosting)

=== Linode Backup Service (full instance backup)

* daily/weekly/monthly

=== NewRelic (Monitoring)

=== Scout (monitoring)

* client app served on Linode
* installed via whenever
* per-minute snapshots
* 5-minute curl (but only email notifications)
* email notifications on memory/disk/os issues

=== LogRotate

Installed

=== <?> (Clean restart)

* should use upstart (via foreman)

=== <?> (App status)

=== <?> (DNS balancer)

=== <?> (Continuous Deployment)

=== DNSimple (DNS)

* low (1hr) timeout (good refreshes if things go to shit)


=== Password security

* S3
* Postgres
* Cloudant/Couch
- in Rails source code!
! consider Rbenv.vars /etc/environment


=== Ideas

statsd from etsy;
 - graphite
 - 37s did one (w/ redis)

=== Ruby
 - straight 1.9.3 compiled on Linode

rbenv advantages?

== Staging

Current URL: actionlight.gravitylabs.io

Originally built with chef, then clone taken for prod.

== Production

URL: www.classroom7.com

* is a snapshot of production
* (tried with chef - but had issues and ran out of time)
* need updated Postgres recipe
*


== To Deploy

*

