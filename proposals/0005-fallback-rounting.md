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

Model provider APIs are external resources that **could be unavailable** from time to time. 
When your applications use those API in the business-critical workflows, this becomes a problem that could cause revenue lose.

Model API could be unavailable in the following ways:
- API returns `50X` errors that indicate internal service issues
- API returns `429` errors indicating rate or quota limits. API is healthy in this case, but given credentials are not going to work for specified period of time
- API could answer for too long a.k.a. **timeout**. Increase in timeout rate means availability issues on the provider side.

At the same time, routing requests to a healthy model provider could be just one of requirements 
the application may have to pick the next model to server the request. Other considerations may be helpful, too, like:
- **Weighted Routing (a.k.a Weighted Round Robin or WRR)** e.g. let 30% of requests be processes by Cohere model, 20% by Anthropic, and remaining 50% by OpenAI.
- **Priority-based Routing** e.g. OpenAI should serve all requests if available. If not, then Anthropic, and finally, Google Bard
- **Latency-based Routing** e.g. the next request should be served by the fastest available model in the pool. This is useful for real-time applications where latency is important.
- **Cost-based Routing** e.g. the next request should be served by the cheapest available model in the pool
- **Performance-based Routing** e.g. the next request should be served by the model that performs the best for my request (see GEP003).
- etc.

WRR could be useful to perform **A/B testing** of model pool setup to find the best model/params/prompt combination for given task.

### Requirements

- R1: We MUST fallback to a healthy model if any in a pool doing our best to fulfill routing strategy specified in configs
- R2: Fallback MUST be as quick as possible. No redundant retrying or waiting
- 

## Design

The trivial strategy to tackle this problem is redundancy. So we have some backup models to talk to in case the main one is down.
The mechanism of switching models in case of failures is called fallbacking.

## Testing

TBU

## Alternatives Considered

[TBU, what other solutions were considered and why they were rejected]

## Future Work

- Cost- and performance-based routing is out of scope for Glides MVP, but could be brought on later on
- Streaming model responses & model that accept streaming input (e.g. STT models) are out of scope right now, but will be considered in the future 
