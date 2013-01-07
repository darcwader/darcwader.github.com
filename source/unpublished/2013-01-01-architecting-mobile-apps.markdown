---
layout: post
title: "Architecting Mobile Apps"
date: 2013-01-01 23:54
comments: true
external-url: 
categories: [ Management ]
---

In this fast paced mobile world, a mobile app has to react and adapt to customers in a very short duration. It isn't a surprise that most successful apps are created by small startups. Darwinian principle in the app world is *survival of the fastest*.

### Customers First

Every successful app has solved something that a customer needs. 90% of startups fail because they do not understand what the customer wants. Even the path of most successful companies was not the route which they started out with. Microsoft when it started it wanted to make money by selling programs, but IBM's direction showed it a better way. The only way to make sure what the customer wants is to create a prototype and get their feedback.

The choice of which platform to reach first hence is essential to gathering the customer requirements. iOS users will generate different set of feedback centered on design than android centered on stability. The development of a quick prototype to gather is data is also faster as the Objective-C platform is more mature and apple provides better tools at developers disposal.

### Wireframes

Which is the most important part of waterfall model in any project development? its no surprise if you said *Requirement Gathering*. In reality the requirements evolve but the ideas remain the same. Its important that every project member understands the ideas behind the projects. 

The way I see it, the product owner projects his view of ideas to the designer. designer puts his view of ideas along with developers view to create the project requirements. The best representation of the project requirement is the product backlog. The backlog is the single point of reference of ideas for designer, developer and product owner.

### Design

With hundreds and thousands of apps in the AppStore, it is very hard to get good visibility. The mere functioning of app isn't sufficient anymore. The app needs to be unique and intriguing. It has to be very easy to use. A 10% increase in usability can easily double the sales.

### Engineering

A company that gets software written faster and better will, all other things being equal, pu tits competitors out of bussiness. Time to market is a very important factor which makes or breaks a company. All mobile devices nowadays are changing every year to newer form factors , newer SDK's. If an app is in development too long, it runs the risk of being outdated before it is even released. 

Application development has to happen on two parts. The requirements and The language.

Each requirement translates into separate module which is build independently. Every module is separate and has interfaces defined with which it communicates with other modules. These modules are generally app specific like views, buttons, tabs.

The other part is developing on the language to create higher abstractions. These tend to bridge the gap between the language and modules being developed. These generally tend to be reusable across other projects and greatly reduce the development time and cost. Thes are like network management, image cache, file management etc.

One overlooked part of development is being compatible. The SDK's usually evolve to provide significant performance boosts but do not provide backwards compatibility. There is a trade off to target newer devices faster, the older devices cannot be supported. The newer SDKs not only offer newer APIs but offer newer language paradigms which greatly enhance development. Generally the tradoff is often worthwhile as newer SDK adoption in mobile is very high (think 90%'s). 

There is a bigger hurdle here that the entire development team doesnt become aware of the newer technology overnight. Looking from a dev who does not know newer SDK features, it would as if the newer SDK might not help greatly. But loooking from dev who knows the newer technology, it would seem like the newer technology can save significant time. This loss of perspective keeps most dev teams to use older tehnology. It takes a very focused group of devs to always stay ahead of the industry and follow the advancement of technology.

### Quality

Is a simple equation

Quality = Requirements x Design x Engineering

Good quality product is a result of perfect execution in wireframing, designing and engineering. Sometimes we make mistakes and we need to fix them. This where a qa team helps. QA in mobile app development is essential throughout the lifecycle. Idetify issues in requirements, designs or engineering. Lets update the equation with this to get

Quality Product = (Requirements x Design x Engineering) / QA



