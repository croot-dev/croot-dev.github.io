---
layout: post
title: "Nuxt3(Nitro)환경에 Redis, FileStorage 연동"
created: 2024-01-15
edited: 2024-08-03
category: [기술]
tags: [nuxt:rgb(245 224 233):rgb(76 35 55),nitro:rgb(211 229 239):rgb(24 51 71),redis:rgb(250 222 201):rgb(73 41 14)]
---


NoSQL 기반 목업 API 구축을 위해 Nitro를 이용하여 Redis, FileSystem 을 연동해보았다.


Redis, FileSystem 이 외에도 azure, cloudflare, mongoDB 등등 다양한 드라이버를 제공하여 간단하게 연동할 수 있다.


Nuxt3에서는 `~/server/plugins/` 폴더 내 파일들을 자동으로 호출하여 주어 편리하다.


## redis

1. DB 정보 환경 설정

	```yaml
	# .env
	
	REDIS_HOST = "127.0.0.1"
	REDIS_PORT = 6379
	REDIS_DB = 0
	REDIS_USERNAME = [USERNAME]
	REDIS_PASSWORD = [PASSWORD]
	```

2. Redis Driver를 Nitro에 등록

	```javascript
	// ~/server/plugins/redis.ts
	
	import redisDriver from 'unstorage/drivers/redis';
	
	export default defineNitroPlugin(() => {
	  const storage = useStorage();
	
	  // Dynamically pass in credentials from runtime configuration, or other sources
	  const driver = redisDriver({
	    base: '',
	    host: useRuntimeConfig().redis.host,
	    port: parseInt(useRuntimeConfig().redis.port) as number,
	    username: process.env.REDIS_USERNAME,
	    password: process.env.REDIS_PASSWORD,
	    db: (process.env.REDIS_DB || 0) as number
	  });
	
	  // Mount driver
	  storage.mount('redis', driver);
	});
	```

1. 실제 사용 시에는 nitroPack에서 자동 로드 된 useStorage를 이용하여 DB 핸들링을 해주면 된다.

	```javascript
	// ~/server/api/index.ts
	
	export default defineEventHandler(async (event) => {
	  const storage = useStorage('redis');
		await storage.setItem('dashboard', 'item');
	});
	```


## Filesystem

1. DB 정보 환경 설정

	```yaml
	# .env
	
	FS_BASE = "./server/db"
	```

2. FS Driver 를 Nitro에 등록

	```javascript
	// ~/server/plugins/storage.ts
	
	import fsDriver from 'unstorage/drivers/fs';
	
	export default defineNitroPlugin(() => {
	  const storage = useStorage();
	  const driver = fsDriver({
	    base: process.env.FS_BASE,
	    watchOptions: {}
	  });
	
	  // Mount driver
	  storage.mount('fs', driver);
	});
	```

3. 실제 사용 시에는 nitroPack에서 자동 로드 된 useStorage를 이용하여 DB 핸들링을 해주면 된다.

	```javascript
	export default defineEventHandler(async (event) => {
	  const storage = useStorage('fs');
	  const data = await storage.getItem('dashboard');
	  return data;
	});
	```

4. 실제 `~/server/db` 를 접근하면 파일이 변경되는 것을 확인 할 수 있다.
