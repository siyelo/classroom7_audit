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




= Rails Deployment Audit

  * http://blog.siyelo.com/rails-deployment-audit

== Extract sensitive data

  All sensitive data like passwords, API keys & tokens that are in the application are extracted as environment variables outside of the source code repository. If using rbevn for managing rubies, there is a nice plugin for setting up environment variables rbenv-vars.

  * https://github.com/sstephenson/rbenv
  * https://github.com/sstephenson/rbenv-vars

== Continuous Integration & Deployment

  There are many great open source and commercial tools out there for Continuous Integration. We are using Jenkins CI to run our test suite and handle Continuous Deployment (if the tests are green) for our staging branch. For production releases, we suggest doing releases manually at a scheduled time with the whole team ready and available.

  * http://jenkins-ci.org/
  * https://www.ruby-toolbox.com/categories/continuous_integration

== Deployment tools

  Checkout gitploy - bare minimum, git-based tool for deployment, it is very simple & interesting

  * https://github.com/brentd/gitploy

== Server security

  * http://library.linode.com/securing-your-server

== Start clean on boot

  All the services that are being used by the application need to start cleanly when the server boots up - there should be no need for manual intervention. If you are deploying to Ubuntu, update-rc.d is the tool to use. You can setup the init script links for a target script /etc/init.d/name with "sudo update-rc.d -f name defaults" and you can use foreman gem to manage Procfile-based process and export them to upstart easily for monitoring.

  * http://manpages.ubuntu.com/manpages/precise/man8/update-rc.d.8.html
  * http://michaelvanrooijen.com/articles/2011/06/08-managing-and-monitoring-your-ruby-application-with-foreman-and-upstart/

  - Export from foreman to upstart with:
    foreman export upstart /etc/init -a myapp -u deployer

  - Start upstart script automatically on boot (add the following to the top of /etc/init/myapp.conf file)
    start on runlevel [2345]

  - If you want to automate with capistrano you can use sed:
    sudo sed -i \"1 i start on runlevel [2345]\" /etc/init/myapp.conf"

  - Setup unicorn init script https://gist.github.com/504875 to start on boot up

== Failover

  Test recovery procedure if server crashes

== Application performance & Monitoring

  New Relic have Availability monitoring similar to pingdom that will email you when server is down

  * http://newrelic.com/features/availability-monitoring
  * http://www.pingdom.com/

== Data security

  Setup SSL
