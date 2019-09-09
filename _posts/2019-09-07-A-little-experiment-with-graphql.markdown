---
layout: post
title:  "A little experiment with GraphQL"
tags: [experiments]

---

### A Super High Level Description ###

When `GraphQL` was first announced as OSS software from Facebook in 2015 I remember briefly looking at it and thought it made sense for Facebook use case and similar other use cases. To expand on this a bit more, here is a concrete example. When I am browsing through my friend's list I am proably interested in snapshot of each friend as follow :

```javascript
{
	name
	location
	profile_pic_url
	birthday
}

```

If I were to use pure `REST` approach to this problem, I would have to to make following few API calls 
-  `/friends` to get a list of my friends' ids
- `/friends/id` to get a detail of each friend such as profile_pic_url, birthday
- `/locations/id` to get details of locations.

and more than likely data these APIs return will have more attribues than I need because for a given entity `friend` the server has lots more attributes. So a logical optimization would be to introduce query parameters such as `view=list`, `view=details`, `include_location=true`, `include_friends=true` As you can imagine this opens lots of permuations and combinations on entities and their relationships. In a nutshell a client is at mercy of server to figure out how to get what it needs.

On the other hand, the `GraphQL` approach would be that client knows what it wants so server should give exactly that -- no more, no less. In essence, you construct your request by defining the resources you want. You send this via a `POST` to a server, and the response matches the format of your request. It's server's responsibilities to stitch different entities together regardless of how data are stored in its datastore. This sounds super exciting from a API consumer perspective. Clients do not have to know how different entities are tied together, don't have to make 10s of API calls just for one view.

### A tiny project to look under the hood ###

Recenlty I dug a little bit more to understand pros and cons of `GraphQL` and what it would take to expose this service on top of existing data infrastructure. I have created a small project, [graphql-rails](https://github.com/prabhatjha/graphql-rails/) using [GraphQL-Ruby project](https://graphql-ruby.org/). The `graphql` ruby gem saved me from adding a bunch of boiler plate code you see at [https://github.com/prabhatjha/graphql-rails/tree/master/app/graphql](https://github.com/prabhatjha/graphql-rails/tree/master/app/graphql) but I still had to describe my ojects/models in GraphQL's type system as you can see [here](https://github.com/prabhatjha/graphql-rails/blob/master/app/graphql/types/query_type.rb) . 

### Pros and Cons ###

I then played with the UI tool called `GraphiQL` and following pros and cons stick out:

- Because you have to describe types, [see `end_users`,](https://github.com/prabhatjha/graphql-rails/blob/master/app/graphql/types/end_user_type.rb) you get the documentation for "free" and more importantly documentation is always consistent with actual implementation. 
- The HTTP status code server returns is either `200` or `500`.  To return more granualar status codes and error messages you would have to implement that yourself. It's possible that there are tools and wrappers which make it easier for implementation but IMHO it's a big gap, for it means that most of API monitoring tools you rely on currently are useless in this context. 
- From a client perspective there is one `URL` to hit. From a server perspective any advanced API analytics tools which give you a view of resources usage breakdown will be of no value. 
- URL based routing that you often see on load balancers and API Gateway also are of no use. 
- Well understood concepts such as **Authorization (eg OAuth)**, **Paginations** are harder to implement.

I have not done lots of research on tools and frameworks in this space but it seems that it's an evolving space. Few prominent offerings are listed [here](https://graphql.org/code/#services). 

In essence, you should approach using GraphQL for the use case it's built for, not just to look cool and to ditch REST apis. Picking a new framework is more than about just the framework itself. Some well known use cases are listed [here](https://www.graphql.com/case-studies/). I think the most pragmatic approach to expose this capability at your organzation would be to first expose it as a wrapper atop your existing REST apis instead of starting from scratch.  For a brand new project I suggest to build a small project with complete SDLC which includes CI/CD process and monitoring so that you identify gaps in all different dimensions namely people, product, process and infrastructure.



