---
layout: project-post
title: "Learning Auth With Golang: Introduction"
slug: intro
description: "Let's Roll Our Own Auth"
published_date: 2020-05-17
author: Franco Posa
order_number: 1
---

As an engineer developing web applications, I find myself to be particularly curious about the parts of our stack that we are often encouraged to just accept as part of the magic we all work with - the frameworks, tools, and protocols developed and perfected over decades by experts for smarter and more dedicated than ourselves. 
"Don't reinvent the wheel!" Reach for a tried and true library. Or, as the top SEO-optimized search results will say, _buy_ a tried and true solution!

This may be good advice when it comes to something as sensitive as authentication and authorization, but  I personally do not learn very well this way. I often need to take a system apart and put it back together, or -- even better -- build the system myself from scratch in order to really, _really_ get it. 

So for my own education and that of any other experiential learners that may come across this project, I am setting out to roll my own authentication and authorization provider from the ground up and document it all. While there is no intent to turn this project into anything serious or production-ready, I will not kneecap the experience by intentionally making a simplified, toy implementation. 

Everything will be open source and I welcome feedback on the code, its documentation, and these posts.

Let's see how far we can take it!

## Our Current System Architecture

For the purposes of demonstration, we are going to to adopt an architecture that makes the various roles in our auth system as clear as possible. It's a bit disjointed, but the mix of components represent a situation many companies find themselves in.

We divide the components of our system up according to the [OAuth2 Roles](https://tools.ietf.org/html/rfc6749#section-1.1):

##### Resource Servers:
**1. The monolith**  - Reports of its death have been greatly exaggerated. While the monolith is no longer the new shiny tech that everyone in the company wants to work on, it is not going away anytime soon. The monolith:
1. Serves some of the pages in our customer portal as well as our internal admin portal via class server-side HTML templating.
2. Provides a partial API backend to our new single-page app, which makes up the other half of the customer portal.

**2. API Services** - New, cool, and small-but-not-micro, our services can be deployed and scaled on their own, but have complicated our authentication and authorization schemes. These services are called directly by both the monolith app server and by our single-page app via the browser.

##### Client Applications:
**1. The Monolith** - The monolith is a private client of the other API services, depending on the services for some routes of the the monolith API endpoints as well as to hydrate data into some of the templates for the customer and admin portals.

**2. The Single-Page App (SPA)** - The SPA is a public client of both the Monolith APIs and well as the other service APIs.

**3. The Future: 3rd-Party API Clients** - As a tech company in 2020, we really want to offer a public API so our users and business partners can build cool integrations and functionality on top of our core competencies. We are not quite there yet, but our new Auth system should enable this without much extra work.

## What We Are Building

Somehow our product has made it this far without even the most basic authentication. Not to worry, this means we are not tied to any legacy technologies! We have the opportunity to create our own authentication and authorization servers compliant with OAuth2, OpenID Connect, and other relevant current best practices.

##### Authentication (Authn) Server
The Authn Server fulfills a few core functionalities
* User signup - Registering new users with their usernames, emails, and passwords
* User login - Authenticating users via standard username/email/password login
* User permissions - for now, the clients requesting permissions on behalf of our users are our own clients. We know 





