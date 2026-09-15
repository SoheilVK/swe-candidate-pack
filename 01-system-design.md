# System Design: External API Integration

Design a system to efficiently fetch and update event ticket data from a third-party supplier's API while respecting strict rate limits and handling large data volumes.

## Problem

We regularly synchronize ticket listings from a third-party tickets provider into our internal system. Maximize update throughput while staying inside the API's constraints and keeping data reasonably fresh across thousands of events.

## Technical constraints

### API limitations

- **Rate limit:** 500 requests per minute (global across all operations)
- **Pagination:** each request returns one page of results
- **Page size:** maximum 200 tickets per page
- **Shared budget:** the rate limit is global: all request types count toward the same pool. You cannot bypass it with parallel clients or extra API keys unless you explicitly assume the supplier allows that (default: they do not).



### Data volume

- **Events:** thousands of events need regular updates
- **Popular events:** 4,000+ tickets is common (20+ requests per full refresh)
- **Freshness:** ticket data should stay reasonably fresh; popular/on-sale events matter more than quiet ones



## What to produce

1. **Architecture**: components, how work is scheduled, where state lives
2. **Rate-limit strategy**: how you stay under 500 rpm without idle waste
3. **Prioritization**: how you decide which events to refresh first when the budget is tight
4. **Failure handling**: 429s, 5xx, partial page failures, stuck workers
5. **Data model**: what you store so the product can serve “current” listings

