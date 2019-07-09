---
title: "Parallel Image Download"
date: 2013-02-12T11:08:04+05:30
comments: true
categories: [ios, programming]
---

Photo download is one of the most common feature in iPhone app. Just as Download Manager speeds up due image downloads, we can speed up the image download using multiple requests.

## Is it really gonna speed up the download ?

Usually single large image loads take considerable time due two 2 factors. 

* Server bandwidth - the bandwidth might be fixed bandwidth for each request 
* Client bandwidth - the speed of internet connection

Client bandwidth's effective utilization requires carefully crafted architecture in backend and front end. Server bandwidth on the other hand is much easier to optimize.

## My server would get loaded in parallel, is it a good thing?

Nowadays, we don't generally store images in our own server. we store it into s3 buckets and other servers which have load balancers and scaling capabilities. So, yes it is a good thing if you are using such service.


## On to the code

not so fast! first lets understand the flow 

1. get *image contentsize* by sending a HEAD request for the resource
1. divide into 3 parts
   1. download bytes `0` to `contentsize/3`
   1. download bytes `contentsize/3+1` to `2*contentsize/3`
   1. download bytest `2*contentsize/3` to `contentsize`
1. join all the parts

## Now on to the code

### Getting the image content size

``` objc
NSMutableURLRequest *headRequest = [NSMutableURLRequest requestWithURL:imgurl];

[headRequest setHTTPMethod:@"HEAD"];
[headRequest setValue:@"bytes" forHTTPHeaderField:@"If-Ranges"];
[headRequest setValue:@"bytes=0-1/1" forHTTPHeaderField:@"Range"];

```

``` objc
NSInteger contentLength = [[resp.allHeaderFields objectForKey:@"Content-Length"] integerValue];
```

### Now create multiple requests diving the content length

``` objc
NSString *rangeString = [NSString stringWithFormat:@"bytes=%i-%@", chunkPosition, requestSizeEnd];

NSMutableURLRequest *downloadRequest = [NSMutableURLRequest requestWithURL:imgurl];
[downloadRequest setValue:rangeString forHTTPHeaderField:@"Range"];
```

### Download more data and merge

``` objc
[downloadedData insertObject:data2 atIndex:index];
```

### Join all parts

``` objc
[compiledData appendData:partialData];
```

## Beware of the Cache

NSURL will cache response and return you the same data for first , second and third parts as it doesnt care about header length fields. So better disable the caching to avoid sleepless nights.

{{< highlight objc >}}
-(NSCachedURLResponse *)connection:(NSURLConnection *)connection

                 willCacheResponse:(NSCachedURLResponse *)cachedResponse

{
    return nil;
}
{{< /highlight >}}

All credit to Vivek Rajanna for this idea and implementation.





