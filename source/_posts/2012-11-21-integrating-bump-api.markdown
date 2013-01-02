---
layout: post
title: "Integrating Bump API in iOS"
date: 2012-11-21 15:36
comments: true
categories: [programming, iphone]
---

Stop waiting for NFC to pickup, coz its not. with the announcement of iPhone 5 and iOS 6 passbook, Apple has made it clear they are not interested in NFC. which means we need to find alternative solutions for iPhone. Bump which transfers contacts between two phones just by bumping them together has an API. below are the steps to integrate that into your app.


Download the api from
{% blockquote %}
https://github.com/bumptech/bump-api-ios.git
{% endblockquote %}

now to integrate it it just takes two steps

1. add the lib and .h into your project
2. include CFNetwork, Core Location 

that's it.

the basic sample mentioned by bump is as good as it gets.

{% codeblock lang:objc %}
- (void) configureBump {
    [BumpClient configureWithAPIKey:@"b8283fabcf1345559951c466d387432c" andUserID:[[UIDevice currentDevice] name]];

    [[BumpClient sharedClient] setMatchBlock:^(BumpChannelID channel) { 
        NSLog(@"Matched with user: %@", [[BumpClient sharedClient] userIDForChannel:channel]); 
        [[BumpClient sharedClient] confirmMatch:YES onChannel:channel];
    }];

    [[BumpClient sharedClient] setChannelConfirmedBlock:^(BumpChannelID channel) {
        NSLog(@"Channel with %@ confirmed.", [[BumpClient sharedClient] userIDForChannel:channel]);
        [[BumpClient sharedClient] sendData:[[NSString stringWithFormat:@"Hello, world!"] dataUsingEncoding:NSUTF8StringEncoding]
            toChannel:channel];
    }];

    [[BumpClient sharedClient] setDataReceivedBlock:^(BumpChannelID channel, NSData *data) {
        NSLog(@"Data received from %@: %@", 
                [[BumpClient sharedClient] userIDForChannel:channel], 
                [NSString stringWithCString:[data bytes] encoding:NSUTF8StringEncoding]);
    }];

    [[BumpClient sharedClient] setConnectionStateChangedBlock:^(BOOL connected) {
        if (connected) {
            NSLog(@"Bump connected...");
        } else {
            NSLog(@"Bump disconnected...");
        }
    }];

    [[BumpClient sharedClient] setBumpEventBlock:^(bump_event event) {
        switch(event) {
            case BUMP_EVENT_BUMP:
                NSLog(@"Bump detected.");
                break;
            case BUMP_EVENT_NO_MATCH:
                NSLog(@"No match.");
                break;
        }
    }];
}
{% endcodeblock %}


You just need to understand the basic usage of this. Inorder to use the API's your device needs to be connected to Bump. Once each device is connected to bump, say DeviceA and DeviceB then both of them need to bump together. if they simultaneously bump together, the bump api will match its location and establish a connection. Once connected any one device may send data to the other. 

That's it.

I have created a sample github repo with a sample app. check it out.

!(BumpApp Github ProjectPage)[https://github.com/darcwader/BumpApp]



