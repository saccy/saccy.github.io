---
layout: post
title: Automation as software
description: Automation as software
summary: Automation code is software that often lacks attention to detail
comments: true
tags: [configuration, management, chef, puppet, ansible, sysadmin]
---

## Correctly configuring computers
### Conundrum or crapshoot?
Configuration management is really an essential part of system administration, but that's not to imply that it should be relegated to sysadmins to have exclusive control of it. In fact config management is the perfect poster child for DevOps. What better way to bring development and operations together?

You probably know it doesn’t quite work that way in practice in many organisations. You’ll get sysadmins who are intimidated or disinterested in the as-code aspect and devs that are intimidated or disinterested in the infra aspect. From this melting pot of socially awkward outrage was born the DevOps engineer, a most peculiar and confused breed indeed. Whether you’re one who challenges the legitimacy of DevOps engineers or not, the fact remains that they are a not so new normal, and the task of developing your orgs config management code is theirs.

### Why is config mgmt hard?
Learning curve
When I think back to when someone said `you have to use ansible` and I said `wot` I liken the experience to learning a web development framework like react or angular in terms of complexity. The initial WTF moment felt roughly the same for me both with web dev and config mgmt. To say it another way; config mgmt is the more obscure concept to grasp, but the rabbit hole goes deeper with web dev frameworks.
Development and testing
If you don’t have experience writing code then you’re going to struggle with config mgmt more than you would writing a normal python script. Actually, even if you do have experience it’s going to seem weird at first, but that’s the nature of declarative programming. 
Testing can be a royal pain when you’re dealing with config mgmt. All the spinning up and destroying of fresh infra can be hugely time consuming. Things like dokken can make this easier but they don’t cover everything, and you can be stuck waiting on a clean instance to finish building only so you can mangle it with your slick config mgmt code that itself took 10 minutes to run. Rinse and repeat.
Errors can seem totally bizarre. Trust that you’re using a computer and there is always some logical explanation to your error, even if the message doesn’t make sense to you. That or you found a bug.
Bug bears
Directory structure: This can cause headaches when you’re starting out. Some of the directory structure is rigid, some is loose. I always follow recommended best practice here. Personally I prefer when product developers dictate a single way in which something should be done, but often with config mgmt tools you’ll be able to do the same thing in a multitude of ways.
Logic: Simple things like looping can be quite weird in declarative code and you’ll find them wanting. It’s part of the trade off and to be fair most of the time the DSL’s are adequate.

### Stick with it
My career certainly wouldn’t be where it is without having learned configuration management and it’s still a niche skill in some places. It works really well when you develop and maintain your codebase properly, but many organisations come unstuck here. See my [automation as software](link to blog) post for what I mean.

### Dynamic example 
Let’s look at a practical example of some config management code. I’ll use `puppet` but the same principle applies to the likes of `chef` and `ansible` amongst others.

If you use one of these tools professionally then you’ll be getting requests to install agents on the servers in your fleet frequently, and more often than not it’s the same pattern:
Download the agent
Install/configure the agent
Ensure the agent service is running

There will be different things that need to happen per agent - think ports, registry keys - but let’s keep it simple for now.

Please dont copy and paste this into a production environment, it is untested and meant for use as an example only.

In this example we’re defining a reusable data pattern for `RHEL`/`CentOS` servers and RPM packages. Here is what the dir structure looks like. I always recommend using [in module hiera data](https://puppet.com/docs/puppet/latest/hiera_migrate.html#adding_hiera_data_to_a_module) when working with puppet.

`/example/manifests/init.pp`
```
#Installs a fake example service
class example (
  $dirs,
  $files,
  $source,
  $service,
) {

  file { $dirs:
    ensure => 'directory',
  }

  $files.each |$key, $value| {
    file { $value['name']:
      path   => "${value['path']}/${value['name']}",
      source => "${source}/${value['name']}",
    }
  }

  package { split($files['package']['name'], '[-]')[0]:
    source   => "${files['package']['path']}/${files['package']['name']}",
    provider => 'rpm',
    notify   => Service[$service],
  }

  service { $service:
    ensure => 'running',
  }

}
```

`example/data/common.yaml`
```
---
#Example parameters
example::service: 'exampleservice'
example::source: 'puppet:///installer_files/example/example-1.0.0-1.x86_64.rpm'
example::dirs:
 - '/var/opt'
 - '/var/opt/example'
 - '/var/opt/example/example_dir'
example::files:
 config:
   name: 'example.conf'
   path: '/var/opt/example/example_dir'
 package:
   name: 'example-1.0.0-1.x86_64.rpm'
   path: '/tmp'
```

