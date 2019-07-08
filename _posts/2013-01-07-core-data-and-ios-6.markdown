---
layout: post
title: "Core Data and iOS 6"
date: 2013-01-07 23:17
comments: true
categories: [programming, ios]
---

Core data is one of the technologies which elevates the performance and forms an essential part of any iOS app. 

### Advantages

* Performance - easy to fine tune.
* Model - often overlooked core data awesomeness. a model ties together the data and the view, which gives better consistency while dealing with webservices. 
* Faults - efficient usage of memory . loads only the necessary content. 
* Caching - uniform way to fetch different kinds of data offline. convinient saving APIs.
* Undo - easily undo grouped changes

### Basics

{% img /images/posts/2013-01-07/stack.png 'stack' %}

* Persistent Store

   Persistent store is the file format used to store the data, like SQLite.

* Managed Object Context

   The main context used to create all objects. different contexts have different state of objects. Eventually all contexts sync. Threads should have separate contexts.

* Managed Object

   Actual object which needs to be displayed or saved. Each object type in the model is called Entity.
    
* Fetch Request

   Fetch the managed objects from a context.

### Modeling Entities

Entities should be modeled close to the views and not the data. The tables should be normalized and denormalized to give the best performance.

consider the following model, here we model cities and state relation ship. We need to search both together, instead of joining both results and searching independently, we will create another entity where we store either a state or city but constitutes as a global search entity.

{% img /images/posts/2012-12-18/cityschema1.png "city schema 1" %}

### Fetching Data

* Create Fetch Request with Entity Name
* Exectute Fetch

{% codeblock lang:objc %}
NSFetchRequest *fetchRequest = [[NSFetchRequest alloc] initWithEntityName:@"SearchEntry"];

NSError *error;
NSArray *res = [self.managedObjectContext executeFetchRequest:fetchRequest error:&error];
NSLog(@"fetched result %d", [res count]);

{% endcodeblock %}

The fetch result returns an array of objects. these objects maybe contain data or maybe faults. Faults are lazy loaded objects which get fetched when we try to access its data.

#### What if I want to fetch a particular object?

set a predicate before executing the fetch

{% codeblock lang:objc %}
NSPredicate *predicate = [NSPredicate predicateWithFormat:@"self == %@", targetObject];
[request setPredicate:predicate];
{% endcodeblock %}

Expressions such as less than greater than values, specific range of dates can also be done.

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

Easiest of them all.
{% codeblock  lang:objc %}

[self.managedObjectContext deleteObject:myObject];

{% endcodeblock %}
                    
Core data owns the lifecycle of Managed Object.So we have to make sure the object references are not used anywhere after deleing. If we try to access this object details after it has been deleted,it results in an exception.

## Creating a Managed Object Context

Create a separate managed object context for each thread and share a single persistent store coordinator.
Usually, there is only one persistent store coordinator which is set to multiple managed object contexts.

{% codeblock lang:objc %}

@interface NSManagedObjecContext
- (id)initWithConcurrencyType:(NSManagedObjectContextConcurrencyType)ct
@end

enum {
  NSConfinementConcurrencyType        = 0x00,
  NSPrivateQueueConcurrencyType       = 0x01,
  NSMainQueueConcurrencyType          = 0x02
};
typedef NSUInteger NSManagedObjectContextConcurrencyType;

{% endcodeblock %}

Confinement type would be used where each thread has its own context. Main Queue type is used if the context is used in the main thread.

you should not pass managed objects or managed object contexts between threads. Pass its `objectId` which is used to fetch the object in other context.

Track changes with  `NSManagedObjectContextDidSaveNotification` notification. merge it in to different contexts if required.

## Core Data Models

### Music Library

* Have some Songs
* Songs belong to an album
* Song created by an artist

{% img /images/posts/2013-01-07/model-music.png %}

### Image Feed

List of images shown in a table

* Have a feed with image
* Have Image Id
* Have Image caption

{% img /images/posts/2013-01-07/model-photo1.png %}

When 10 items are fetched, all 10 item images are loaded into memory. even if it is just a list showing thumbnails.

*Improvements*

* Create Thumbnail
* Move out large data cunk into separate entity

{% img /images/posts/2013-01-07/model-photo2.png %}

## Profiling 

{% img left /images/posts/2013-01-07/profile.png %}

The most important part about using core data is its ability to profile. Run core data analysis through instruments to find out all bottlenecks and improve the model to get the best performance. This convenience greatly helps in identifying performance issues. Ultimately resulting in delivering a better user experience. 

*Delivering 10% better user experience can easily double the app sales!*
