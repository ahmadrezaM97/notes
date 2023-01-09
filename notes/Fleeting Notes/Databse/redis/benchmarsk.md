

what happend i redis runs out of memoery


redis will start to reply with an error to write commands

It's not very frequent that CPU becomes your bottleneck with Redis, as usually Redis is either memory or network bound. For instance, when using pipelining a Redis instance running on an average Linux system can deliver 1 million requests per second



Redis can handle up to 2^32 keys, and was tested in practice to handle at least 250 million keys per instance.



Every hash, list, set, and sorted set, can hold 2^32 elements.