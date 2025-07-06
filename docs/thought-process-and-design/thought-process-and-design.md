A random collection of other ideas:

- UI VS API
- Developer vs Architect
- Workflow vs data flow

Building backends with logic apps and inline code/functions? Leverage connectors and other ideas and use code for the heavy lifting
functions inside vnet and only logic apps connecting to them, isolate logic apps and functions
use managed identity and dont allow direct access to functions
orchestration layer
do as much as you can in your orchestration layer, move to using other helpers when it becomes unfeasible to do so. There are a few guidelines but no hard rules so your experience will be key to deciding when to do what. A lot of times simple solutions become mroe complex so don't be afraid to try out new things.
A checklist for things to look out for and solutions to those:
- Scalability and performance, avoid complex loops and large amounts, use liquid transformations and Azure/inline Functions to handle. If you run into actually big amounts, it might be worth it to check out Data Factory.
- Event vs batch

test and fail fast approach, I have often found it to be easiest to develop alongside a subject matter expert and a system specialist.
Working in a team: If you are working in a team where 100% of your time is allocated to a single project then a regular schedule along other people is propably fine. Often the reality is that there are multiple projects going on at the same time where things are progressing at varying degrees of success. In these cases I would recommend discussing with project managers, if instead of being in regular meetings where there is not much to discuss, it would be possible to create another smaller team for updates specifically into integration matters. There could include all the people working directly with the integrations, including subject matter experts and system specialist. This way you can have a more focused session to discuss matters that are directly related to the work at hand. This saves calendar time for a lot of participants.

Should you know what you are integrating? Yes and no is the simple answer. More often than not it is impossible in the beginning to know what the data that you are transferring does in each system. Also the sheer amount of knowledge needed to know everything about everything will be simply too much. But over time if you have long enough projects you can start making educated guesses on how the data should look like and what it does.