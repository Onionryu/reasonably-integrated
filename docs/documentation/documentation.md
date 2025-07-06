---
title: Documentation
---
# Intro
What is needed in documentation about specific things and who should handle those?
Think about the consumer groups who will be looking through your documentation and try to answer their needs.

# Integration Developers
The main thing that your documentation should answer is: How does this solution work on a technical level? What goes in, what happens inside and what comes out? This will already give you an idea on how to do the solution even without seeing it and give other developers a concrete example of how to use the solution. I would suggest putting example messages of both in and out and explanation of what the data is.
One other thing is to list dependencies to other resources, including where the solution is hosted and what it needs to work. You should also list at least the basics of other things needed when deploying the solution, maybe the solution needs a username + password combination to another system or a storage table named in a specific way.

# System Specialists
Mainly meant for people who are not that familiar with the technology that the solution uses, either super users of systems that the solution is connected to

# The dreaded business side
Now this is something that is sometimes difficult to do. Try to have an overview of the solution without many solution specific knowledge, assume that the people who read this are not developers or system specialists. They should still get a clear idea on what this does and who to contact.
- 

# Troubleshooters
A lot of times you might create a system that you are not going to be upkeeping personnally, sometimes it's a team you are part of or another team entirely. This is also a good idea to have a detailed section because often times these solutions will live on for years and you might not be working at the same place anymore the next time something happens.
- known issues

# TLDR
How to put this all together? I would suggest a top-down approach where you list more general information first and people can find more detailed items when they scroll further. This way anyone who needs a quick check on some information can get it faster, maybe it is contact information or troubleshooting 

Integration documentation
- High level items (catalogue style)
  - Contact information
- Troubleshooting guide and known issues
- Integration workflow (high level explanation)
- Integration details (low level explanation on how things work)