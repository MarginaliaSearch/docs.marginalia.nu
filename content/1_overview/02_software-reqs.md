---
title: '1.2 Software Requirements'
published: '2024-01-16'
---

The software requirements for running the Marginalia Search Engine are:

* Linux, probably doesn't matter which distro, the instructions assume something Debian or Ubuntu-like.  If you're running something else, especially something other than Linux, it may or may not work.
* Docker ([install guides](https://docs.docker.com/engine/install/))
* Docker Compose ([install guides](https://docs.docker.com/compose/install/))
* Java (use [sdkman](https://sdkman.io/) to install):
* * JDK 22 for the latest released version of the system
* * JDK 23 for the head of the git repository
* Tailwindcss via NPM

To install tailwindcss

1. Install NPM
2. Run `npm install tailwindcss @tailwindcss/cli`

Note:  The JS ecosystem is quite prone to churn, and how to install this seems to change every once in a while.  If you're having npm related issues, consult https://tailwindcss.com/docs/installation/tailwind-cli
