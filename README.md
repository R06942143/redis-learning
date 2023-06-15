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
