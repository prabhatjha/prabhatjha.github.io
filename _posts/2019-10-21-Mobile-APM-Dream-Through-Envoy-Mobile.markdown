---
layout: post
title:  "A mobile APM dream and Envoy Mobile"
tags: [mobile]

---

I recently came across a new open source project [Envoy Mobile](https://eng.lyft.com/announcing-envoy-mobile-5c2067d9ade0) from the makers of [Envoy Proxy](https://www.envoyproxy.io/). When I read about the problems Envoy Mobile is designed to solve it took me down memory lane to my first startup. 

Back in 2011-2012 mobile apps were getting hot, B2C was the primary use cases but B2B and enterprise apps were starting to gain tractions. I was working for JBoss middleware as part of [Red Hat](https://redhat.com) then. One of my colleague introduced me to Alan Ho. Alan previously worked at Amazon Web Services (AWS) and had just moved to Austin,TX to work for Dell. When we met for the first time we soon realized that we both wanted to create a startup. 

We took a pragmatic approach -- what problem could we solve given our strengths? He had seen enough at AWS to be a cloud expert and I had seen enough at Red Hat to know what it takes to build and operate high quality enterprise server side applications. After few iterations and dabbling through several SaaS ideas we realized that we should find something that intersects mobile and cloud. These are still hot areas to explore but back then if you put mobile and cloud in the same sentence you would get some eye balls like you do these days when you put machine learning and Kubernetes together. ;-) We joked initially but as we dug deeper we realized that we had found something worth solving for. InstaOps was born. Cool name, no?

Our insight was that for a web based app providers who now  need to deliver similar capabilities through mobile apps, how your customers consume your offerings have changed but underlying operational qualities and user experieces don't change. Customers who are used to good qualities of your web based offerings would expect similar qualities for your mobile based offerings. So as a provider of these web and mobile apps you would need to analyze the logs from mobiles apps, network performance metrics (latency, error etc) from mobile device persepctives. There were startups such as Mixpanel but they were focused on user and behavorial analytics. APM tools for server side monitoring and debugging at the time such as New Relic, Splunk, App Dynamics were useless to say the least. 

For network performance we realized that it was more multi faceted than server side web based deployments. For example, network perforamnce of API calls from mobile devices would depend on several factors that were not applicable to server side deployments:

- Carriers such as AT&T, Verizon, T-Mobile
- Network types such as 3G, 4G, Wifi
- Mobile OS such as iOS, Android and Windows Mobile
- Mobile OS versions
- Device makers such as Apple, Samsung, LG
- App versions

Here is a initial prototype of our dashboard

![InstaOps Network Prototype](/assets/instaops-network.png)

It was early for the major OS providers to provide these APM services out of the box. An interesting idea was to work with mobile OS providers to bundle our network agent/proxy in the OS in a way that the proxy would be "optionally" available to all apps runing on a device. It was even naive of us to  entertain this thought but we secretely hoped that if we were to get traction may be we would have enough data points to back our case. In hindsight you would think if these OS providers were to build something like this they would build themselves instead of incorporating a 3rd party software for such a critical part of the OS. So we decided to build a network wrapper/proxy for two major OS namely iOS and Android to be bundled as part of an app. For iOS we leveraged "swizzling" to automatically capture network traffic without single line of code change to. All you had to do was bundle our SDK. We were never sure if our approach vioated Apple's policy but we did not hear any complaints so we assumed it was all right. Such "swizzling" approach was not possible for Android so we ended up creating a wrapper for the most commonly used Apache HTTP Client but it did require some code changes to existing applications. These instrumentation allowed us to capture performance metrics and break it down across different dimensions that I mentioned above. 


We were not only able to help you see metrics across these dimensions but we also provided some control planes to configure network caching, timeout parameters, log levels. For example, if you noticed that latency was higher on 3G on Verizon then it made sense to increase timeout for these users.

Before I talk about Envoy Mobile I will simply mention that we got acquired by [Apigee](https://apigee.com) where our vision was to provide end to end API monitoring where InstaOps monitoring would complement Apigee's API monitoring. Our biggest success came when our SDK was bundled with Walgreens mobile app. It was interesting to observe spikes on network latency during lunch time. :-) 


I believe that Envoy Mobile will see  success. Core contributors and community behind this project understand networking nuances very well. It will have good adoption when it's bundled with mobile app itself. It would be amazing if Envoy Mobile gets deployed as a sidecar similar to how Envoy Proxy is deployed in Kubernetes and managed by a central control plane. But I think that will be almost impossible, at least in the next few years for the Mobile device ecosystem is more complicated and there are more players involved in supply chain such as OS maker, device makers, upgrade frequency. And most importantly there are data privacy and data sharing concerns.

Here is wishing the team and community all the best in their journey. It's a goal worth trying.