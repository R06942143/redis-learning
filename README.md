# redis-learning
---
## Data Structure
1. String
2. hashes
3. lists
4. sets
5. sorted sets with range queries
6. bitmaps
7. hyperloglogs
8. geospatial indexes
9. streams

---
## Atomic operations
1. Append:
    1. String append is O(1) time complexity.
2. HINCRBY:
    1. Hash increment is O(1) time complexity.
    2. Increments the number stored at field in the hash stored at key by increment. If key does not exist, a new key holding a hash is created. If field does not exist the value is set to 0 before the operation is performed.

---
## Getting Started
1. Install redis on mac
```bash
brew install redis
```
2. Start redis server
```bash
redis-server
```
3. start redis service
```bash
brew services start redis
```
4. stop redis service
```bash
brew services stop redis
```
5. check redis service status
```bash
brew services info redis
```
6. connect to redis server
```bash
redis-cli
```
---
## Redis data types tutorial
### keys
Redis keys are binary safe
- Very long keys are not a good idea. For instance a key of 1024 bytes is a bad idea not only memory-wise, but also because the lookup of the key in the dataset may require several costly key-comparisons. Even when the task at hand is to match the existence of a large value, hashing it (for example with SHA1) is a better idea, especially from the perspective of memory and bandwidth.
- Very short keys are often not a good idea. There is little point in writing "u1000flw" as a key if you can instead write "user:1000:followers". The latter is more readable and the added space is minor compared to the space used by the key object itself and the value object. While short keys will obviously consume a bit less memory, your job is to find the right balance.
- Try to stick with a schema. For instance "object-type:id" is a good idea, as in "user:1000". Dots or dashes are often used for multi-word fields, as in "comment:4321:reply.to" or "comment:4321:reply-to".
- The maximum allowed key size is 512 MB.

