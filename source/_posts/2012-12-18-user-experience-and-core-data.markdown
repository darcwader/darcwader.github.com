---
layout: post
title: "User Experience and Core Data"
date: 2012-12-18 15:32
comments: true
external-url: 
categories: [Programming] 
---

## Web Trends

Generally if you see the browser downloads. we see that last two years we see a gradual decline. Users are preferring to consume through mobile devices rather than browsers.  

{% img /images/posts/2012-12-18/trend.png "Google Browser Trends" %}

## Native

Facebook released a new native iOS app which uses native controls instead of html5 controls. The user experience improvement was significant and even made zuckerberg admit it was a [mistake not to go Native!][1]

Google maps is another example of why native gives much better user experience than html5. The number of downloads skyrocketed to more than [10million][2] . There are lot of others like flickr, instagram, path which are all native apps.

Wunderlist 2 [goes native][3], the titanium based hybrid app ditched the html5 hybrid approach to give better experience.

User Experience is highly driven by performance. The smoother the app performs the better the user experience will be. No surprise all these apps are ditching hybrid and html5 for Native approach. 

## Core Data

Core data is one of the technologies which elevates the performance and forms an essential part of any iOS app. 

### Advantages

* Performance - easy to fine tune
* Model - often overlooked core data awesomeness. a model ties together the data and the view, which gives better consistency while dealing with webservices. 
* Faults - efficient usage of memory . loads only the necessary content. 
* Caching - uniform way to fetch different kinds of data offline. convinient saving APIs.
* Undo - easily undo grouped changes


### Basics

* Persistent Store & Managed Object Model
* Managed Object Context
* Managed Object
* Fetch Request


### Modeling Entities

Entities should be modeled close to the views and not the data. The tables should be normalized and denormalized to give the best performance.

consider the following model, here we model cities and state relation ship. We need to search both together, instead of joining both results and searching independently, we will create another entity where we store either a state or city but constitutes as a global search entity.

{% img /images/posts/2012-12-18/cityschema1.png "city schema 1" %}

### Fetching Data

* Create Fetch Request
* Attach Entity
* Exectute Fetch

{% codeblock lang:objc %}
NSFetchRequest *fetchRequest = [[NSFetchRequest alloc] initWithEntityName:@"SearchEntry"];

NSSortDescriptor *sortDescriptor = [[NSSortDescriptor alloc] initWithKey:@"name" ascending:YES];
NSArray *sortDescriptors = [[NSArray alloc] initWithObjects:sortDescriptor, nil];
[fetchRequest setSortDescriptors:sortDescriptors];

NSError *error;
NSArray *res = [self.managedObjectContext executeFetchRequest:fetchRequest error:&error];
NSLog(@"fetched result %d", [res count]);

{% endcodeblock %}

The fetch result returns an array of objects. these objects maybe contain data or maybe faults. Faults are lazy loaded objects which get fetched when we try to access its data.


### Insert Object

* Create object through entity
* Set Properties with KVO
* Save Context

{% codeblock lang:objc %}

NSManagedObject *state=[NSEntityDescription insertNewObjectForEntityForName:@"State" inManagedObjectContext:self.managedObjectContext];
[state setValue:stateString  forKey:@"name"];
[self.managedObjectContext save:&error];

{% endcodeblock %}

### Setting Relationship

Setting relationship is as easy as setting any property.

{% codeblock lang:objc %}

NSManagedObject *searchEntry=[NSEntityDescription insertNewObjectForEntityForName:@"SearchEntry" inManagedObjectContext:self.managedObjectContext];

[stateObj setValue:searchEntry forKey:@"searchEntry"];
[self.managedObjectContext save:&error];


{% endcodeblock %}


### Deleting object

Easiest of them all, but make sure the object references are not used anywhere. If we try to access this object details after it has been deleted results in an exception.

{% codeblock  lang:objc %}

[self.managedObjectContext deleteObject:myObject];

{% endcodeblock %}


The most important part about using core data is its ability to profile. Run core data analysis through instruments to find out all bottlenecks and improve the model to get the best performance. This convenience greatly helps in identifying performance issues. Ultimately resulting in delivering a better user experience. 

*Delivering 10% better user experience can easily double the app sales!*


[1]:http://techcrunch.com/2012/09/11/mark-zuckerberg-our-biggest-mistake-with-mobile-was-betting-too-much-on-html5/
[2]:http://techcrunch.com/2012/12/17/google-maps-for-ios-was-downloaded-over-10m-times-in-its-first-48-hours/
[3]:http://www.tuaw.com/2012/12/18/wunderlist-2-goes-native-adds-many-new-features-to-beautiful-f/

