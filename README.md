# API Fields Middlware
[![Licensed under MPL 2.0](https://img.shields.io/badge/license-MPL_2.0-green.svg)](https://www.mozilla.org/en-US/MPL/2.0/)
[![Build Status](https://github.com/launchcodedev/api-fields-middleware/workflows/CI/badge.svg)](https://github.com/launchcodedev/api-fields-middleware/actions)
[![npm](https://img.shields.io/npm/v/@lcdev/api-fields-middleware.svg)](https://www.npmjs.com/package/@lcdev/api-fields-middleware)

Use API Fields easily in Koa.

```bash
yarn add @lcdev/api-fields-middleware@0.1
```

```typescript
import { ApiField } from '@lcdev/api-fields';

// somewhere, where your model is defined
class EntityA {
  @ApiField()
  publicFieldA: any;

  @ApiField()
  publicFieldB: any;

  privateFieldA: any;
  privateFieldB: any;
}

import { extractApiFieldsMiddleware } from '@lcdev/api-fields-middleware';

// somewhere, where you have a koa router
app
  .get('/foo/bar', extractApiFieldsMiddleware(EntityA), async (ctx, next) => {
    // the body ends up as an object that is, or looks like an 'EntityA'
    ctx.body = new EntityA();

    return next();
  });
```

This will perform the `extract` with `ApiFields` of `EntityA`, after your router
middleware has run. This will only apply **if the response is a 200**.

Features:
- Supports `extractApiFieldsMiddleware([EntityA])` to imply that the api response is an array of EntityA
- Supports `extractApiFieldsMiddleware({ ... }, true)` to pass a "raw" `Extraction` object to use (you might use `getApiFields` with an extension here)
