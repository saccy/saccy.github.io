---
layout: post
title: Docker volume backups
description: Docker volume backups
summary: Bash and powershell scripts for backing up and restoring docker volumes
comments: true
tags: [cicd, docker, backup, bash, powershell]
---

*The docker volume backup GitHub repo can be found <a href="https://github.com/saccy/docker_vol_backup" target="_blank">here</a>*

I've recently been switching between mac and windows for a project I've been developing. This wasn't an issue in itself because the code is being developed inside a containerised environment, however there *was* an issue in keeping the persistent docker volumes backed up and shared between the two machines.

At the moment the solution will only backup to and restore from local storage, however I intend to add s3 as a storage option.

Feel free to check it out, contribute or raise an issue.
