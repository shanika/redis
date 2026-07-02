Optional dynamic libraries loaded into the server at startup or runtime to add commands and data types beyond the core.

## Responsibilities
- Register new commands, data types, and keyspace hooks through the C module API
- Ship engines such as RedisBloom, RediSearch, RedisJSON, RedisTimeSeries, and vector-sets

## Tech Stack
- C shared objects (`.so`) built against `src/redismodule.h`; sources vendored under `modules/`
