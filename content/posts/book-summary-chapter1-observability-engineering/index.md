---
title: "[Book Summary - Observability Engineering] Chapter 1 - What is Observability?"
date: 2023-02-13T10:07:17+11:00
tags: ["notes", "book", "SRE", "observability", "observability_engineering"]
draft: false
---

_Observability Engineering By Charity Majors, Liz Fong-Jones, and George Miranda_

# Chapter 1: Introduction to Observability Engineering
## The Challenge - The Why "observability" and Not just "monitoring"?
The chapter opens with a discussion of the growing complexity of modern software systems, which often consist of many interconnected components and services that run on cloud infrastructure. The author argues that this complexity has made it increasingly difficult for developers to understand and debug their systems when things go wrong. Traditional monitoring tools, such as system health checks and basic logging, are no longer sufficient to provide developers with the insights they need to identify and resolve issues.

To address this challenge, the author introduces the concept of observability engineering. Observability is defined as the ability to understand a system's behavior by observing its outputs, such as logs, metrics, and traces. The author argues that observability is necessary for building and maintaining reliable and scalable systems, and that it is an important complement to traditional monitoring. With observability, developers can gain a much deeper understanding of how their systems are behaving, even under conditions of high complexity and variability.

## Observability Concept - How?
The chapter then provides an overview of the three pillars of observability: logs, metrics, and traces. Logs are textual records of events that occur within a system, such as requests, errors, and warnings. Logs are useful for providing context and detailed information about specific events, making them a powerful tool for debugging complex systems. Metrics, on the other hand, are quantitative measures of a system's behavior, such as CPU usage, request latency, and error rate. Metrics provide a high-level view of how a system is behaving over time, making it easier to identify trends and spot anomalies. Finally, traces are records of the interactions between different components of a system. Traces can be used to reconstruct the path of a request or transaction, making them useful for understanding the performance and behavior of complex systems.

## Embracing Failure
The chapter emphasizes the importance of embracing failure and learning from incidents. The author argues that failures are inevitable in complex systems, and that developers should aim to minimize their impact by designing for resiliency and practicing incident response. The author encourages developers to use incidents as an opportunity to learn and improve their systems, rather than as a source of blame or shame.

## Conclusion
Overall, the chapter provides a comprehensive overview of the challenges posed by complex modern software systems and the need for observability to address these challenges. The three pillars of observability, logs, metrics, and traces, are introduced and their respective strengths are explained. Finally, the author emphasizes the importance of embracing failure and learning from incidents as a way to improve system reliability and resiliency.