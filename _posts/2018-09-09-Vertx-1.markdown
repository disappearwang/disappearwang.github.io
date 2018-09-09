---
layout: post
title: "Reading Notes - Vert.x - 1 - en"
date: 2018-09-09 17:35:00 +0200
categories: [architecture, book]
---
[A gentle guide to asynchronous programming with Eclipse Vert.x for Java developers](https://vertx.io/docs/guide-for-java-devs/guide-for-java-devs.pdf)
_Julien Ponge, Thomas Segismont and Julien Viet
version 1.3.0, June 5th 2018_

# Introduction

Vert.x (Eclipse Vert.x) is a toolkit for building reactive applications on the JVM. Which means, that it support various languages based on JVM like JAVA, Groovy, Scala, Kotlin...etc

Vert.x aime to deal with more concurrent netword connections with less threads than synchronous APIs such as Java servletx or java.et socket classes. So the principle limite to concept application is the resource of the server.

Vert.x is _polyglot_. Note that when we talk about "support a language" is not juste to prived acces to the APIs, but also to make sure that eh language-specific  APIs are idiomatic in each target language (e.g., using Scala futures in place of Vert.x futures).

# Core Vert.x concepts
There are 2 key concepts to learn in Vert.x :
1. what a `verticle` is
2. how the `event bus` allows verticles to communicate

## Verticle
The unit of deplyment in Vert.x is called a _verticle_. A verticle processes inconming events over a _event-loop_, where events can be anything like receiving network buffers, timing events, or messages sent by other verticles. Event-loops are typical in asynchronous programming models :
 ![Event-loops](/_image/vertx/event-loop-model.PNG)

 Each event shall be processed in a reasonable amount of time to not block the event loop. This means that thread blocking operations shall not be performed while executed on the event loop.

 In any case Vert.x emits warnings in logs when the event loop has been processing an event for _too long_.

Every event loop is attached to a thread. By default Vert.x attaches 2 event loops per CPU core thread.

A verticle can be passed some configuration (e.g., credentials, network addresses, etc) and a verticle can be deployed several times :
![verticle-deplyment](/_image/vertx/verticle-deploy-model.PNG)

When a verticle opens a network server and is deployed more than once, then the events are being distibuted to the verticle instances in a round-robin fashion which is very useful for maximizing CPU usage with lots of concurrent networked requests. Verticles have a simple start/stop life-cycle, and verticles can deploy other verticles.

## Event bus
An exemple :
![event bus](/_image/vertx/Event-bus-model.PNG)

The event-bus allows passing any kind of data. Message can be sent to destinations which are free-form strings. The event bus supports the following communication patterns:
1. point-to-point messaging
2. request-response messaging
3. publish/subscribe for broadcasting messages

The event bus allows verticles to transparently communicate not just within the same JVM process :
- when network clustering is activated, the event bus is _distributed_ so that messages can be sent to verticles running on other application nodes
- the event-bus can be accesed through a simple TCP protocol for third-party applications to communicate
- the event-bus can also be exposed over general-purpose messaging bridges (e.g., AMQP, Stomp)
- a SockJS bridge allows web applications to seamlessly communicate over the event bus from JavaScript running in the browser by receiving and publishing messages just like any verticle would do
