---
MEP: 5
Title: Fallback & Load Balancing
Discussion: Link
Implementation: Link
---

# Fallback & Routing

## Abstract

In this GEP, we talk about fallback mechanism, the key strategy to cope with model provider availability issues, 
and load balancing which is somewhat related topic.

## Motivation

Model provider APIs are external resources that could be unavailable from time to time. 
When your applications use those API in the business-critical workflows, this becomes a problem that could cause revenue lose.

At the same time, routing requests to a healthy model provider could be just one of requirements 
the application may have to pick the next model to server the request. Other considerations may be helpful, too, like:
- **Weighted Routing (a.k.a Weighted Round Robin or WRR)** e.g. let 30% of requests be processes by Cohere model, 20% by Anthropic, and remaining 50% by OpenAI.
- **Priority-based Routing** e.g. OpenAI should serve all requests if available. If not, then Anthropic, and finally, Google Bard
- **Latency-based Routing** e.g. the next request should be served by the fastest available model in the pool
- **Cost-based Routing** e.g. the next request should be served by the cheapest available model in the pool
- **Performance-based Routing** e.g. the next request should be served by the model that performs the best for my request (see GEP003).
- etc.

WRR could be useful to perform A/B testing of model pool setup to find the best model/params/prompt combination for given task.

### Requirements

[TBU, list all key requirements to keep in mind, use https://datatracker.ietf.org/doc/html/rfc2119 verbs]
- TBU 

## Design

The trivial strategy to tackle this problem is redundancy. So we have some backup models to talk to in case the main one is down.
The mechanism of switching models in case of failures is called fallbacking.

## Testing

TBU

## Alternatives Considered

[TBU, what other solutions were considered and why they were rejected]

## Future Work

[TBU, what we put outside of the equation now, but may want to consider in the future]
