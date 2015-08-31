---
title: Using Ansible to Rule the World! (well, the DevOps world--maybe, I guess)
tags: [ansible, techops, devops, mike-klass]
categories: []
name: [Mike Klass]
author: [mike-klass]
snippet: [Lorem ipsum dolor sit amet, consectetur adipisicing elit. Blanditiis doloremque cumque neque architecto asperiores consequuntur repellat voluptate ut. A ipsam culpa voluptates inventore voluptas quia vel ipsa quis laudantium? Nulla?]
---

### Intro
So my first blog post is about my learning experience with [Ansible](https://en.wiktionary.org/wiki/ansible) because I really wanted to learn how Ansible works and how we could leverage it here at WayBetter to help make our migration process easier. So far I really love how easy it has been to set up and configure. It seems like it is definitely the way to go for our migration, and I am going to spend a lot more time learning about it's strengths (and weaknesses) so we can leverage it properly!

### Why I Like Puppet
[Puppet](https://puppetlabs.com/puppet/what-is-puppet) is awesome because it lets you script monotonous jobs that you usually have to spend hours doing over and over. It saves time, saves brain cycles, and saves you from having to do so much boring command-line crap. It also reduces the chance of misconfiguration, missed steps, and fat-fingering, all of which happen on a regular basis and can prevent true system parity.

### What a DevOps Engineer Wants, What a DevOps Engineer Needs, Whatever Makes a DevOps Engineer Happy Sets You Free
When I first heard about a tool that would allow you to [script building](https://en.wikipedia.org/wiki/Build_automation) environments WITHOUT needing to use any external protocols (a.k.a. everything is done over [SSH](http://searchsecurity.techtarget.com/definition/Secure-Shell)! U wot m8?) I was not only intrigued, but excited. Also, knowing Ansible mainly works with [YAML](https://en.wikipedia.org/wiki/YAML) (much like Puppet/Heira), it made the learning curve basically disappear. New commands needed to be learned, sure... but the basic syntax and how things run when you step through the playbooks is very similar. I mainly wanted to see if they fixed most of my major gripes with Puppet/[Heira](http://docs.puppetlabs.com/hiera/1/).

### Why I Dislike Puppet
One of the major gripes I had with Puppet was the lack of true [modular design](https://en.wikipedia.org/wiki/Modular_programming). In order to create modular systems that would only install certain configs based on hosts, you had to do an extreme amount of pre-configuration in Heira and set up complicated chains of nested files in subfolders, scripts that were prone to breaking your configs, and difficult to track down when you needed to debug. Many people have come up with decent modular Puppet configs on their own, but I wanted to see if something existed already that could be made to work in a modular fashion without a whole lot of work up front (to save time and brain cycles... we don;t have a whole lot of time for R&D on this so we want to hit the ground running).

### So, What's So Great About Ansible
Ansible works by connecting to each of your [endpoints](https://github.com/Mach-II/Mach-II-Framework/wiki/Introduction-to-REST-Endpoints) (that you define) and pushing out small programs, called “Ansible Modules” to them. Ansible then executes these modules (over SSH) and removes them when finished. No traces are left and this means there are no security vulnerabilities, which is great!

Your playbooks can reside on any machine, and there are no servers, daemons, or databases required.

### Ansible Tower
We are using [Ansible Tower](http://www.ansible.com/tower) (in limited trial) which does require a DB [(PostgreSQL)](http://www.postgresql.org/)
You work with Ansible using a Terminal, a text editor, and Git to keep track of changes. It's as simple as that. No complicated "Master/Slave" setups and no required modules.

One of the best things about Ansible, though, for me... is that it just seems to make a lot more sense. When I wrote puppet configs, I found it difficult to wrap my head around all of the things that were going on, and things just seemed convoluted and more complicated than necessary. Ansible fixes most of these issues for me. I just inherently understand Ansible a lot more. Grok++

### Where Do We Go From Here
Most of what I have learned so far is around [Roles](http://random-notes-chris.blogspot.com/2014/02/git-submodules-for-ansible-roles.html), which are sorta like sub-modules. In our Git repo [Rocannon's Lunch Box](https://github.com/waybetterdev/rocannons-lunch-box) I have written several roles which encompass main program groups on our endpoints. Roles can be enabled or disabled based on specific hosts, so if you say you don't need MySQL installed on a Production box, Ansible won't run the MySQL role on the production cluster. This makes things modular in a way that was much more difficult to script in Puppet without writing your own complicated host-recognition code (which could be done based on hostname, but we all know that hostnames tend to stray from naming conventions in many cases).

I would like to continue learning about how we can leverage Ansible moving forward. Things such as [Role Scaffolding](http://probablyfine.co.uk/2014/03/27/how-to-write-an-ansible-role-for-ansible-galaxy/), which allow you to build out roles based on specific host requirements, seems like something we could leverage. I would also like to build out the requirements definitions, and learn how those can be used for best practices. I would also like to learn more about task delegation, so we can use this to reduce the load on our Ansible Tower server when building out large parts of the infrastructure.

Repo: [Rocannons Lunch Box](https://github.com/waybetterdev/rocannons-lunch-box)

### Introduction to My First Ansible Playbook
Rocannon's Lunch Box is a playbook to set up a basic LAMP (Linux, Apache, MySQL, PHP) server. This is set up for [CentOS 6.6](https://en.wikipedia.org/wiki/CentOS).
In it's entirety, it will install Apache 2.4.12, PHP 5.6.10, MySQL Community server 5.7.7 and Varnish Cache 4.0.3.
You can exclude certain roles if these are not necessary for the box you are building.
I set the playbook up using a ["vagrant"](https://en.wikipedia.org/wiki/Vagrant_(software)) user account as the base SSH action account, so it will allow you to run the playbook on a standard vagrant instance of CentOS 6.6.

### Roles
I have set up the following Roles that break out the setup of individual components. This should allow for disabling certain components when they are not required in a build (for instance, there is no need for local MySQL on a Production server since the MySQL database will be hosted in [AWS](https://aws.amazon.com/)).
####epel
Adds the [EPEL](https://fedoraproject.org/wiki/EPEL/FAQ#What_is_EPEL.3F) repo to CentOS. EPEL contains many updated libraries and is required for builds.
common
This Role updates everything to the latest versions and installs required packages for CentOS so it can be provisioned by Ansible. In addition it will create groups, users and directories for compiling purposes. We compile PHP, Apache, etc from source to gain more control of how the builds happen and maintain version parity across all servers in the future. This does not limit us, the version of PHP can be updated by us in the Ansible Playbook manually when we see fit, and then we just need to run the Playbook to update those specific Roles.
####firewall
This sets up [iptables](https://en.wikipedia.org/wiki/Iptables) to listen on the required ports.
####ssh
Configures  the [SSH daemon](http://linux.die.net/man/8/sshd) and improves SSH security by locking down default CentOS security vulnerabilities. 
####httpd
[Apache server 2.4.12](http://httpd.apache.org/). Apache will be built from source and installed in local system. It contains configuration files, basic hardening, logrotate and /etc/init.d script. In addition it installs the Apache ModSecurity module and OWASP rules.
####OWASP
[OWASP](https://en.wikipedia.org/wiki/OWASP) is the Open Web Application Security Protocol, and it defines certain best practices for setting up Apache for web applications. We use many of the rules defined by OWASP to set up Apache for better security.
####php
This installs [PHP 5.6.10](http://php.net/manual/en/intro-whatis.php). PHP will be built from source. It contains configuration files, PHP-FPM setup, OPCache setup, basic OWASP hardening and creates the /etc/init.d script for PHP-FPM.
####PHP-FPM
[PHP-FPM](http://php-fpm.org/) (FastCGI Process Manager) is an alternative PHP FastCGI implementation with some additional features useful for sites of any size, especially busier sites. It spawns a separate process for each PHP handler, making rogue processes easy to track down and find more info on. It also lets you kill those specific processes when they cause the server to become overloaded, while keeping the rest active.
####mysql
Installs [MySQL](https://en.wikipedia.org/wiki/MySQL) community server in latest version (5.7.7). It will be installed from official MySQL repository. Contains configuration files and basic OWASP hardening. This will be commented out in most situations, but is here for when we need to build locals.
#### varnish
Installs [Varnish 4.0.3](https://www.varnish-cache.org/docs/4.0/tutorial/introduction.html). Contains configuration and a basic [VCL](https://www.varnish-cache.org/trac/wiki/VCL), and includes security fixes for CentOS 6.6. Once we figure out if we are going to use Varnish in production, we will also probably need to modify the default VCL here to tune Varnish. This comes later down the road.

###How to run Playbooks
Running a [playbook](http://docs.ansible.com/ansible/playbooks.html) requires Ansible to be set up on the machine you wish to run the Playbook on, so lets get that installed first. You'll need to grab EPEL to install Ansible, so grab that first, then grab Ansible. This assumes you are root--if not, [sudo](https://en.wikipedia.org/wiki/Sudo). This also assumes you are running CentoOS 6.x, and using the 64-bit distribution. If you are using 32-bit, go back and get 64-bit because we use 64-bit on the servers we build.

	$ wget http://download.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm
	$ rpm -ivh epel-release-6-8.noarch.rpm
	$ yum install ansible

Once Ansible is installed, you can run a playbook simply by using the following command:

	$ ansible-playbook playbook_name.yml

So in this case, if you want to run the Rocannon's Lunch Box playbook, you would clone the Git repo onto your local (or whichever server), then run in the base level of the repo:

	$ ansible-playbook webserver.yml

And then Magic happens!