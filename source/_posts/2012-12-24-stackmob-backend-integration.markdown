---
layout: post
title: "Stackmob Backend Integration"
date: 2012-12-24 13:47
comments: true
external-url: 
categories: [Programming]
---

There are a lot of backend services available, with Parse as one of the more popular ones. Windows azure also provides a good BaaS. Stackmob though directly provies integration with Core Data which makes development a whole lot faster and easier.

## Add Sdk

Adding sdk is straight forward, download from (Stack Mob)[https://developer.stackmob.com] and add to your project.

## Managed Object Context

Need to get the managed object context from the SMCoreDataStore

{% codeblock %}
self.client = [[SMClient alloc] initWithAPIVersion:@"0" publicKey:@"YOUR_KEY"];
SMCoreDataStore *coreDataStore = [self.client coreDataStoreWithManagedObjectModel:self.managedObjectModel];
NSManagedObjectContext = [coreDataStore managedObjectContext];
{% endcodeblock %}


## Inserting an entry

Before inserting all entries have to have a primary key field.

{% codeblock %}


{% endcodeblock %}


