# Azure Service Bus queues
In the event-driven pattern, we discussed services' publish and subscribe events. We used an event manager to manage all the events.
In this post, we will see how Azure Service Bus manages events and provides the facility to work with microservices.

## What's Azure Service Bus?
Azure Service Bus is nothing but an information delivery service. It is used to make communication easier between two or more components/services. In our case, whenever services need to exchange information, they will communicate using this. Azure Service Bus plays an important role here. There are two main types of services provided by Azure Service Bus:

    * Brokered communication – This service can also be called hired service. It works in the similar way as postal services work in our real world. Whenever a person wants to send message/information, he/she can send a letter to another person. This way, one can send various types of messages in the form of letters, packages, gifts, and so on. This type of messaging service ensures delivery of a message even when both the sender and receiver are not online at the same time. This is a messaging platform with components such as queues, topics and subscriptions, and so on.

    making a phone call. In this, the caller (sender) calls a person (receiver) without any confirmation indicating whether he/she will pick the call or not. In this, the sender sends information and it purely depends upon the receiver to receive the communication and pass the message back to the sender.

## A multi-tenant cloud service
Service Bus is a multi-tenant cloud service, which means that the service is shared by multiple users. Each user, such as an application developer, creates a namespace, then defines the communication mechanisms she needs within that namespace

## Services Available
We have the following services available on Azure Service Bus:
    * Queues – These allow one-directional communication and act as brokers.
    * Topics – These provide one-directional communication where a single topic can have multiple subscriptions.
    * Relays – These provide bi-directional communication. They do not store messages (as queues and topics do). Relays pass messages to the destination application.

# Implementation of an Azure Service Bus queue
Let's see the actual implementation of an Azure Service Bus queue by creating the following:

    A Service Bus namespace
    A Service Bus messaging queue
    A console application to send a message
    A console application to receive a message

