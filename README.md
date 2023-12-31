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

### Strings
Since Redis keys are strings, when we use the string type as a value too, we are mapping a string to another string. The string data type is useful for a number of use cases, like caching HTML fragments or pages.

#### SET/GET
```Redis
> SET name "Peter"
OK
> GET name
"Peter"
```
#### SET NX/XX (NX: only set the key if it does not already exist, XX: only set the key if it already exist)
```Redis
> SET name Peter
OK
> GET name
"Peter"
> SET name "Conrad" nx // only set the key if it does not already exist
nil

> SET name "Conrad" xx // only set the key if it already exist
OK
> GET name
"Conrad"
```

#### GETSET (set the new value and return the old value)
```Redis
> SET counter 1000
OK
> GET counter
"1000"
> GETSET counter 2000 // set the new value and return the old value
"1000"
> GET counter
"2000"
```

#### MGET/MSET (set multiple keys and get multiple keys)
```Redis
> MSET key1 "Hello" key2 "World"
OK
> MGET key1 key2
1) "Hello"
2) "World"
```

#### EXIST (check if the key exists)
```Redis
> SET name "Peter"
OK
> EXISTS name
(integer) 1
> EXISTS age
(integer) 0
```

#### DEL (delete the key)
```Redis
> SET name "Peter"
OK
> DEL name
(integer) 1
> EXISTS name
(integer) 0
```

#### TYPE (check the type of the key)
```Redis
> SET name "Peter"
OK
> TYPE name
string
> del name
(integer) 1
> TYPE name
none
```

#### EXPIRE (Key expiration)
A few important notes about key expiration:

- They can be set both using seconds or milliseconds precision.
- However the expire time resolution is always 1 millisecond.
- Information about expires are replicated and persisted on disk, the time virtually passes when your Redis server remains stopped (this means that Redis saves the date at which a key will expire).
```Redis
> SET resource:lock "Redis Demo"
OK
> EXPIRE resource:lock 5
(integer) 1
> TTL resource:lock
(integer) 4
> TTL resource:lock
(integer) 3
> TTL resource:lock
(integer) 2
> TTL resource:lock
(integer) 1
> TTL resource:lock
(integer) 0
> TTL resource:lock // the key has expired
(integer) -2
> GET resource:lock
(nil)

> SET resource:lock "Redis Demo" EX 5
OK
> TTL resource:lock
(integer) 4
> PERIST resource:lock // remove the expiration
(integer) 1
> TTL resource:lock // the key will never expire
(integer) -1

> SET resource:lock "Redis Demo" PX 10000
OK
> TTL resource:lock
(integer) 9
> PTTL resource:lock // get the time to live in milliseconds
(integer) 8997
```