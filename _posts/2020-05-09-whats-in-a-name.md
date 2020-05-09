---
layout: post
title: What's in a name?
description: hud - What's in a name?
summary: CICD pipeline names are a powerful way of making your pipeline code dynamic and for reducing repo clutter
comments: true
tags: [cicd, jenkins, groovy, naming, convention, pipeline]
---

Naming conventions go a long way in mainting a clean environment for your developers and infra team. All CICD tools (Jenkins, bamboo, ADO etc.) publish environment variables that are available for use within your pipeline code. The variable names vary between tools but you can manipulate them in the same way. One such example is the name of your pipeline, normally something like `BUILD_NAME`.

These names are a powerful way of making your pipeline code dynamic and for reducing repo clutter. By defining a naming convention for your pipelines and engineering your code around this convention you can save your business time and reduce the inherent risk in deploying changes.
<br>

Lets make the assumptions that we are using <a href="https://www.jenkins.io" target="_blank">Jenkins</a> as our CICD tool and we are developing our pipeline code with <a href="https://groovy-lang.org" target="_blank">groovy</a>
<br>

Consider this simple naming convention for a jenkins job that deploys docker containers:
1. `deploymentEnvironment_containerName_hostPort_containerPort`
1. `deploymentEnvironment` must be a `String` with length between 1 and 5 characters
1. `containerName` must be a `String` with length between 1 and 10 characters
1. `hostPort` must be an `int` between 1 and 65535 (this would be refined in a production environment)
1. `containerPort` must be an `int` between 1 and 65535 (this would be refined in a production environment)
<br>

And this as an example of *implementing* that naming convention:
> `test_apache_4000_443`
<br>

You may have intuited that in this example we're using '_' as a delimiter and that we're going to `split` our pipeline name environment variable, which in jenkins is declared as `JOB_NAME`. To achieve this in groovy is trivial:
```
String[] jobInfo     = env.JOB_NAME.split('_')
String deployEnv     = jobInfo[0]
String imgName       = jobInfo[1]
int hostPort         = jobInfo[2].toInteger()
int containerPort    = jobInfo[3].toInteger()
```

With the `deployEnv`, `imgName`, `hostPort` and `containerPort` variables defined, we can interpolate them where we need throughout the rest of our deployment code in order to achieve the desired result.

A major benefit to developing your pipeline code like this is that you can simply create an exact copy of your pipeline, give it a new name and your work is done, i.e:
> `prod_nginx_4433_443`
<br>

**Keep this information in mind when defining naming conventions:**
- Pipeline names can hold a lot of vauable information.
- Naming conventions should be *code* friendly i.e. no whitespace, no case, no special characters.
- Naming conventions should be *human* readable i.e. succint and logical.
- Avoid randomisation and iterations for the sake of uniqueness as this wastes real estate, i.e `pipeline0`, `pipeline1`.
- What if I want to use my code outside a pipeline? Consider writing in some cmd line flags so that you can input the vars you need instead of hardcoding pipeline env vars within your script.
- Once you come up with a convention you need to adhere to it so work with your team, document your solution and if you feel crazy enough, write a compliance test for your pipeline name.

Example test regex:

`(\S{1,5})_(\S{1,10})_(6553[0-5]|655[0-2][0-9]\d|65[0-4](\d){2}|6[0-4](\d){3}|[1-5](\d){4}|[1-9](\d){0,3})_(6553[0-5]|655[0-2][0-9]\d|65[0-4](\d){2}|6[0-4](\d){3}|[1-5](\d){4}|[1-9](\d){0,3})`

Example test cases:

<span style="color:green">test_apache_4000_443</span><br>
<span style="color:green">dev_nginx_80_80</span><br>
<span style="color:green">dev_nginx_1_65535</span><br>
<span style="color:red">dev_nginx_1_65536</span><br>
<span style="color:red">dev_nginx_0_65535</span><br>
<span style="color:red">pipeline_0</span><br>
<span style="color:red">pipeline1</span><br>
<span style="color:red">prod_jenkins_4000_</span><br>
<span style="color:red">qa-rabbitmq_8080_8000</span><br>
