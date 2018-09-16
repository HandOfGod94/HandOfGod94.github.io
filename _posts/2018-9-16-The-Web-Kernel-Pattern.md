# The Web Kernel Pattern

## History

The traditional web application development has evolved a lot in past few years. Right from 
gigantic monolithic applications written in cobol or java to dividing it into SPAs for 
frontend and microservices backend which can individually be deployed on any machine.

## Context of the problem

It is often seen that when a company starts, it'll just be one application. Say for e.g. 
Google, initial application was just Search Engine, and as they move forward they started 
providing other services such as Gmail, Google News, Google keep and so on. It's just one 
example that I have chosen, since this is what everybody knows and its public too. But 
there could be many. Any product based software company goes thought lots of 
transformation during its lifetime, the mergers acquisitions and what not? The best 
scenarios can be seen in ERP systems.

## Problem

With all these mergers and acquisitions comes the technical overhead of maintaining the 
current application architecture and somehow combine it with the new application came into 
picture at later stage developed completely in different framework.
**This is the challenge I'm going to address today.**

## Solution

If we look at it the design pattern, it's still the same. It's good for just a single 
domain specific application which is only trying to solve a domain specific problem. All 
the techniques that are currently being used such as SPAs, PWAs, microservices etc. scale 
out for these kind of applications but people will still struggle to integrate or tie 
several different application under one umbrella.

> What if we develop something similar to what Operating Systems has been doing
> it for years?

Lets see what an OS provides.
* Any software can be developed using the specific set of APIs provided as part of sdk 
* toolkit and will give the same look and feel.
* The main business logic differs for all the applications inside it. For e.g. Media Player 
  is completely different from a text editor, but both can be installed on same OS with 
  same set of APIs.

So what I'm suggesting is develop a **core** module for web application and the deploy actual business application on top of it. But how do we do that?

If we really look at any web application it has 4 basic components:
1. Business Logic (probably the backend)
2. UI (Look and feel of the application)
3. Database (To store everything)
4. Transformation/Customization [Optional] (The data coming in from outside
   world needs some kind of filtering or transformation).

But if we look at some of the companies, they are already doing it or using the similar 
technique then how is this different. Generally these are giant monolithic architecture 
developed for legacy systems. So they are now unable to leverage the new technology. 
Picture will become more clear as we dwell into the detail of the description of core 
module.

### Core Module:

Lets talk about the first component.
1. Business Logic: Basically it should contain important stuff required for backend to 
   operate. Some of them include **Auth, NavigationLinks, User Management logic, User 
   preferences (Locale, region) etc.**
   We can use what ever framework we like. Django, Flask for python of Spring, Struts for 
   Java, or expressjs for node ecosystem.
   > The important part to remember here is that it should be exposed over `REST` endpoints
   > OR Something similar, like `json-rpc`.
   
2. UI: It should be responsible for look and feel of the whole applications. Some of the 
   includes **UI styling, I18Nized texts, TimeZone conversions, NavLinks Display, UI 
   Components, Application state management** 
   This module can be an SPAs or PWA developed in any of the front end framework such as 
   `ReactJS`, `VueJS`, `Angular` and so on.
   > The important part here is that all the communication to backend will happen over
   > `REST` or protocols like `json-rpc`

3. Database:
   This is probably the straightforward implementation, as to where you want to store the 
   core module's data. As discussed above it could be **user store, auth helper tables, 
   preferences table, Master Data management tables ...**

4. Transformation/Customization:
   It can be used for variety of purposes.
   Generally speaking, we often face a scenario where we want to integrate our existing
   application with an external application. The primary purpose of this module will be to
   covert the data in which the the business logic needs it. This way we are separating the
   data filtering part from the business logic and both will become independent.

Phew!! too much thing to start with. But once you have the initial core ready, you can 
write modules in the same format as you have written core. We have two approaches at our 
disposal,(which I could think of)

#### Approach 1: Pluggable Modules

Typical Pluggable Module structure will be similar to core
* Backend
* UI
* Database
* Transformation

This approach will require an additional component. **Installer**
>Installer: Responsible for plugging in/out the module 

Along with these, a **configuration file** will be required which will tell the `core` and 
`Installer` on what to pick and where to place it from the module. (Useful for adding 
navlinks on the fly for newly added modules).

**In Reality, this is very much similar how VSCode works. It allows creating and installing
some plugins written in any language, following certain specs and then can be integrated
with the existing VSCode to provide more feature.**

#### Approach 2: Extend the Core

Follow the same structure as the core but instead of plugging in the components extend the 
core components itself. This is good for SAAS application but you will loose the ability to 
customize things and add new features on the fly. But can be easy to start with and is good 
if you have only 1-2 modules.

Personally I'm would prefer `Approach 1` as it more OS-like then a Giant Web Application.

> This is just a high level imaginative hypothesis, which will require
> lot of work if it needs reality.