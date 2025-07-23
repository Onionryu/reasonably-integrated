---
title: Environments
---
# Local Environment (local)
This is an environment that is local for the developer, meaning that it is not actually deployed to any remote environments like the rest.
Typically not available for a lot of integration solutions in Azure so this is more in use for Functions and some Logic Apps.

# Development Environment (dev)
sandbox for testing out solutions, meant to be for integration developers
typically used for unit testing

# Test Environment (test)
environment for consumer testing and has a degree of control on how to publish things
typically used for end-to-end testing

# Production Environment (prod)
highly controlled environment

> BONUS: Additional Environments

# UAT/QA Environment
You can add additional environments if needed, these are not hard rules that you should limit yourself to, but for a good reason these are industry standards. One reason to add another environment besides test is that if consumer has specific testing needs or things are done in longer sprints. This would allow you to have a more finely tuned control on what goes to which environment and allows you to continue development and release to test in the meanwhile.

# Problems that might arise if you don't have these:
In some cases you might only be able to access a test or even just a prod environment. This is obviously not ideal when it comes to developing solutions because things have a habit of not working on the first try.

# TLDR
Three environments: dev, test and prod. 
- Dev environment as a developer sandbox, meant for solutions that can break. 
- Test environment can either be dev specifications or the same as prod, used for consumer testing and deployed through IaC to make sure things work in prod. 
- Prod environment is where only configuration changes and hotfixes are made, ideally through test side if possible, no other manual changes. Good idea to have some kind of consumer control over publishing.

- does this need to be in a process format?