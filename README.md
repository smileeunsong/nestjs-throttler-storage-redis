# NestJS Throttler Redis Storage

![Tests status](https://img.shields.io/github/actions/workflow/status/kkoomen/nestjs-throttler-storage-redis/tests.yml?label=tests&branch=master)
[![npm](https://img.shields.io/npm/v/nestjs-throttler-storage-redis)](https://www.npmjs.com/package/nestjs-throttler-storage-redis)

Redis storage provider for the [@nestjs/throttler](https://github.com/nestjs/throttler) package.

# Installation

### Yarn

- `yarn add nestjs-throttler-storage-redis ioredis`

### NPM

- `npm install --save nestjs-throttler-storage-redis ioredis`

# Usage

Basic usage:

```ts
import { ThrottlerModule, seconds } from '@nestjs/throttler';
import { ThrottlerStorageRedisService } from 'nestjs-throttler-storage-redis';
import Redis from 'ioredis';

@Module({
  imports: [
    ThrottlerModule.forRoot({
      throttlers: [{ limit: 5, ttl: seconds(60) }],

      // Below are possible options on how to configure the storage service.

      // default config (host = localhost, port = 6379)
      storage: new ThrottlerStorageRedisService(),

      // connection url
      storage: new ThrottlerStorageRedisService('redis://'),

      // redis object
      storage: new ThrottlerStorageRedisService(new Redis()),

      // redis clusters
      storage: new ThrottlerStorageRedisService(new Redis.Cluster(nodes, options)),
    }),
  ],
})
export class AppModule {}
```

Inject another config module and service:

```ts
import { ThrottlerModule } from '@nestjs/throttler';
import { ThrottlerStorageRedisService } from 'nestjs-throttler-storage-redis';

@Module({
  imports: [
    ThrottlerModule.forRootAsync({
      imports: [ConfigModule],
      inject: [ConfigService],
      useFactory: (config: ConfigService) => ({
        throttlers: [
          {
            ttl: config.get('THROTTLE_TTL'),
            limit: config.get('THROTTLE_LIMIT'),
          },
        ],
        storage: new ThrottlerStorageRedisService(new Redis()),
      }),
    }),
  ],
})
export class AppModule {}
```

# Issues

Bugs and features related to the redis implementation are welcome in this
repository.

# License

NestJS Throttler Redis Storage is licensed under the MIT license.
