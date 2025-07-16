---
title: Home
---

# A starting point to all things Azure.
This is my collection of thoughts regarding the things I have done in Azure for work and free time. This is just a single viewpoint into how I do things so don't take everything in here as set rules, and that is also why I have outlined in many places my thought process into making these solutions. I also don't know everything so there might be many faults in my thinking but I will try to update this as I go along and learn more about Azure. Pages have the starting point of a thought process and it goes more into detail on the challenges and decisions made after that. There is also a TLDR (Too Long Didn't Read) section for people who want just the latest solution in a more high-level approach.

# What are integrations?
Integrations are a specific term used in the data side of system solutions. I like to think of it as an automated transfer of operational data. This differs a little bit from data analytics where information is displayed more on a complete data set. These are typically either shorter event type messages where you handle a single event happening, like an order. The other type is batch type data, where there are multiple events in a single message. The role of integrations is to translate the messages from one system to another so that they can communicate effectively.
> More on details like batches etc. in another page and what you should do in integrations

# Why integrations?
To answer this we need to take a look at how systems have been built. Previously the way was to try to build one system that does everything you need. On paper this sounds amazing because there is no need to go beyond the system to do anything. The reality is that the system often needs a lot of complex development to suit the business needs. This means that the system created serves only the current business and might be useless to other businesses so this will always need to be a customized solution.

After a certain period the system grows so massive that making changes to it becomes more difficult due to it having so much internal business logic and the testing of the whole solution after changes takes a lot of time and effort. Adding specific features might not be possible due to the complexity or the limitations set by the system or any decisions done. You additionally cannot get help from anywhere else because the solution that has been made is unique to each business.
So the solution to this one is to start breaking the system into smaller components and hand over that one to different teams. Maybe you now have a standard solution for handling clients and a custom one for handling the orders that suit your specific business needs. This also allows you to use different technologies that suit the needs better, each team can use specifically the technologies and languages that they need. You have a lot easier time to test solutions because you can contain the changes to a smaller part of the whole. This is called moving from monolithic systems to microservices.

This of course has it's downsides, one major thing is that the more systems and technologies you have, the more likely it is that they are not able to communicate to each other natively. This is where specifically integration work comes into play, the main idea is to enable your systems to work seamlessly and keep giving the development teams the chance to use the tools that they want to.

# Why specifically integration developers?
As these systems continue to grow into more specialized and decoupled systems, the need to learn all other systems and technologies will detract from the actual work of developing the business solution. In a more traditional world integrations were done by system specialists or people who happened to have a needed skillset, these are typically called point-to-point integrations and were done with varying methods and technologies. When these solutions widened we ended up what we typically call 'integration spaghetti' where everything is more or less a mess and hard to make out what everything does.

You are better off with specializing a person or team to handle integration matters. This solves the issue of having a hard to read system environment and collects all the little threads into more of manageable solution. The first step is to collect the integrations into their own system, likewise as the system developers, you allow the integration developers to choose a technology that is suited to their specific needs. This first step is to get the spaghetti onto a plate so it's less messy, meaning a single integration system that for all intents and purposes is another business system like any other. This allows the integration developers to focus on a single technology for their work and most importantly have overview and monitoring to integration solutions.

Integration work can sometimes be a bit difficult to justify. Think of it this way, like electricity and light switches in rooms, you rarely think about them but it would be annoying if you weren't able to access the lights in a room you're in. The same goes for data, you should try to keep a modernized version of integrations in business like any other systems. It has the same rules as other systems, including requiring occasional refactoring and maintenance work. It is often easier to see afterwards if the solution could be better, maybe in terms of architecture, scalability or security.

# Data access and visibility
Quite recently the integration side has broadened into also data access and visiblity. What this means is that things like API Management solutions create a proxy between systems. This way you can control who has access to where by giving consumers conditional access to business systems. Because this is handled in a single system, you can also have the visibility of these API endpoints available to system developers.

# TLDR
- These pages are a collection of ideas that have the "solution" at the end.
- Integrations specifically move operational data, they rarely store it anywhere.
- Integrations solve the issue of having separate systems having to communicate to each other.
- Integration developers because they can focus on the specific systems for their needs.
- Integration solutions need occasional refactoring and maintenance work.
- Expose the endpoints of systems safely with API Management solutions, creating a separate gateway.