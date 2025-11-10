redis-cli

redis-cli monitor

[https://aioredis.readthedocs.io/en/latest/getting-started/](https://aioredis.readthedocs.io/en/latest/getting-started/)

ACL: Redis access control list

Data types

- Strings: Redis Strings are binary safe, this means that a Redis string can contain any kind of data, for instance a JPEG image or a serialized Ruby object. A String value can be at max 512 Megabytes in length.

- Use Strings as atomic counters using commands in the INCR family: [INCR](https://redis.io/commands/incr), [DECR](https://redis.io/commands/decr), [INCRBY](https://redis.io/commands/incrby).
- Append to strings with the [APPEND](https://redis.io/commands/append) command.
- Use Strings as a random access vectors with [GETRANGE](https://redis.io/commands/getrange) and [SETRANGE](https://redis.io/commands/setrange).
- Encode a lot of data in little space, or create a Redis backed Bloom Filter using [GETBIT](https://redis.io/commands/getbit) and [SETBIT](https://redis.io/commands/setbit).

- Lists: lists of strings, sorted by insertion order. It is possible to add elements to a Redis List pushing new elements on the head (on the left) or on the tail (on the right) of the list. The [LPUSH](https://redis.io/commands/lpush) command inserts a new element on the head, while [RPUSH](https://redis.io/commands/rpush) inserts a new element on the tail. The main features of Redis Lists from the point of view of time complexity are the support for constant time insertion and deletion of elements near the head and tail, even with many millions of inserted items. Accessing elements is very fast near the extremes of the list but is slow if you try accessing the middle of a very big list, as it is an O(N) operation.
- Sets:  are an unordered collection of Strings. It is possible to add, remove, and test for existence of members in O(1) (constant time regardless of the number of elements contained inside the Set).
- Hashes: are maps between string fields and string values, so they are the perfect data type to represent objects
- Sorted sets: every member of a Sorted Set is associated with a score, that is used keep the Sorted Set in order, from the smallest to the greatest score. While members are unique, scores may be repeated.
- Bitmaps:
- Streams: is a data structure that acts like an append-only log. Streams are useful for recording events in the order they occur.
- Geospatial indexes: provides geospatial indexes, which are useful for finding locations within a given geographic radius

client-side caching

Solve

A problem with the above pattern is how to invalidate the information that the application is holding, in order to avoid presenting stale data to the user.

 Tracking - Redis client-side caching support. Has two modes

- default mode: the server remembers what keys a given client accessed, and sends invalidation messages when the same keys are modified. This costs memory in the server side, but sends invalidation messages only for the set of keys that the client might have in memory.
- broadcasting mode: the server does not attempt to remember what keys a given client accessed, so this mode costs no memory at all in the server side. Instead clients subscribe to key prefixes such as object: or user:, and receive a notification message every time a key matching a subscribed prefix is touched.