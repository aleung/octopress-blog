---
layout: post
title: "Text to UML diagram"
date: 2014-01-20 22:10
comments: true
tags: 
- SoftwareDev
- UML
---
Usually I used [Astah](http://astah.net/) to draw UML diagram to put into design document in MS Word or PowerPoint format. The separation of the source and generated file making management of the UML model file a problem. And also it's annoying to adjust a large amount of elements by dragging with mouse whenever you add something in the diagram. For a coder, typing on keyboard is faster than drawing with mouse.
 
Now I switch to [PlantUML](http://plantuml.com/). Just write UML in its [DSL](http://en.wikipedia.org/wiki/Domain-specific_language) and PlantUML generates the diagram. It supports most of the frequent used UML diagrams, which I use most are sequence diagram, class diagram and state diagram. Then I paste the generated diagram into slides (design document) and keep the "source" in note. No need to wonder where to find original UML model file when I need to update the diagram.
 
The core of PlantUML is simply a jar file. It parses the text input and depends on _dot_ tool from [Graphviz](http://www.graphviz.org/) for graph generation (except sequence diagram). 

There are [a bunch of ways](http://www.plantuml.com/running.html) to run PlantUML. I highlight some of them I prefer:

- Online editor. Best choice if you're too lazy to install anything on your computer.
- Desktop (Windows): [PlantUML File Watcher](https://code.google.com/p/plant-uml-file-watcher/)
- Embedded into Octopress blog platform. The below examples are using it.
- IDE plugin. Easy way to keep UML together with source code.

The syntax of PlantUML's DSL is quite intuitional, you can start to use it by following example.

```
package ConsentObject <<Rect>> {
  Consent : id
  Scope : key
}
 
Client - User
(Client, User) . Consent
 
Consent - "*" Scope
 
Consent "1" -- "1..*" AccessToken
```

The generated diagram:

{% plantuml %} 
package ConsentObject <<Rect>> {
  Consent : id
  Scope : key
}
 
Client - User
(Client, User) . Consent
 
Consent - "*" Scope
 
Consent "1" -- "1..*" AccessToken
{% endplantuml %} 

A sequence diagram example:

```
title OAuth 2.0 Authorization Code Grant

actor    UserAgent       as UA
participant Application     as APP
participant OAuthServer     as OAuth
participant ResourceServer  as RS

== Application requests authorization from user ==
UA->APP: 
APP-->UA:redirect to OAuth Server
UA->OAuth: GET /authorize?response_type=code
ref over UA, OAuth: user authentication\nuser confirms reqest
OAuth-->UA: redirect to App with authorization_code
UA->APP: authorization_code

== Application retrieve access token ==
APP->OAuth: POST /token with authorization_code
ref over APP, OAuth: client authentication
APP<--OAuth: access_token

== Application access protected resource ==
APP->RS: getResource with access_token
RS->OAuth: validateToken(access_token)
RS-->APP: result
```

{% plantuml %}
title OAuth 2.0 Authorization Code Grant

actor       UserAgent       as UA
participant Application     as APP
participant OAuthServer     as OAuth
participant ResourceServer  as RS

== Application requests authorization from user ==
UA->APP: 
APP-->UA:redirect to OAuth Server
UA->OAuth: GET /authorize?response_type=code
ref over UA, OAuth: user authentication\nuser confirms reqest
OAuth-->UA: redirect to App with authorization_code
UA->APP: authorization_code

== Application retrieve access token ==
APP->OAuth: POST /token with authorization_code
ref over APP, OAuth: client authentication
APP<--OAuth: access_token
== Application access protected resource ==
APP->RS: getResource with access_token
RS->OAuth: validateToken(access_token)
RS-->APP: result
{% endplantuml %}
