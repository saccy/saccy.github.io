---
layout: post
title: Azure DevOps build agent image
description: Azure DevOps build agent image
summary: Containerised ADO build agent
comments: true
tags: [docker, ADO, Azure, DevOps, alpine]
---

*The ADO build agent image GitHub repo can be found <a href="https://github.com/saccy/ado_build_agent_image" target="_blank">here</a>*<br>
*The ADO build agent image on docker hub can be found <a href="https://hub.docker.com/r/saccy/ado-build-agent" target="_blank">here</a>*

Containers are a natural fit as build agents and I had a need for a CentOS 8 based Azure DevOps (pipelines?) build agent.

I plan on creating an alpine based ADO agent image in the future as the CentOS based image is quite heavy.

Feel free to check it out, contribute or raise an issue.

Here's an example of how to pull and run it:
```
docker pull saccy/ado-build-agent

docker run \
    --rm \
    -d \
    saccy/ado-build-agent \
        https://dev.azure.com/${your_project} \
        $your_PAT \
        $your_agent_pool \
        $name_for_your_agent
```
