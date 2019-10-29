---
title: Heap
---

[Heap](https://heapanalytics.com/?utm_source=segmentio&utm_medium=docs&utm_campaign=partners) automatically captures every user interaction with no extra code. This includes clicks, taps, gestures, form submissions, page views, and more. The {{integration.name}} Destination is open-source. You can browse the code [on GitHub](https://github.com/segment-integrations/analytics.js-integration-heap).

This document was last updated on January 29, 2018. If you notice any gaps, out-dated information or simply want to leave some feedback to help us improve our documentation, please let us know!

## Getting Started

{% include content/connection-modes.md %}

1. From your Segment UI’s Destinations page click on "Add Destination".
2. Search for "Heap" within the Destinations Catalog and confirm the Source you’d like to connect to.
3. Drop in your Heap "App ID" into the connection settings.
4. If you are using Heap via Segment's client-side `analytics.js` library, we asynchronously load Heap's Javascript library onto the page. As such, all native functionality of Heap, including auto-capturing of all events.

## Identify

If you haven't had a chance to review our spec, please take a look to understand what the [Identify method](https://segment.com/docs/spec/identify/) does. An example call would look like:

```javascript
analytics.identify('userId123');
```

When you call `identify` we call [Heap's identify method](https://heapanalytics.com/docs/custom-api#identify) with the `userId` and `traits` you provide.

If one of your `traits` is of the date property type, we will convert it into an ISO string.

## Track

If you haven't had a chance to review our spec, please take a look to understand what the [Track method](https://segment.com/docs/spec/track/) does. An example call would look like:

```javascript
analytics.track('Clicked Button');
```

When you call `track` from `analytics.js`, we call Heap's [track function](https://docs.heapanalytics.com/reference#track) with exactly the same parameters. Calling `track` from one of our mobile SDKs or server-side sources records a [Heap Custom Event](https://docs.heapanalytics.com/reference#track-1) with the same event name and properties.

If one of your `properties` is of the date property type, we will convert it into an ISO string.

## Troubleshooting

### Anonymous traffic not showing up from server-side source
Ensure that you are specifying a `user_id` in your event calls. The Heap API will reject any server-side events without a `user_id`.

### Nested Objects and Arrays
Heap does not accept nested properties, so we will flatten and stringify them before sending to Heap. For example:

 ```javascript
 analytics.track('Signed Up', {
   foo: {
     bar: {
       cheese: 'american',
       prop: [1, 2, 3],
       products: [{"A": "Jello"}, {"B": "Peanut"}]
     }
   }
 });
 ```

The properties would be sent as:

```javascript
foo.bar.cheese: 'american'
foo.bar.prop: '[1,2,3]'
foo.bar.products: "[{'A': 'Jello'},{'B': 'Peanut'}]"
```