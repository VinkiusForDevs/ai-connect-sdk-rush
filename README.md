<div align="center">

# Vinkius Connect — Rush monorepo

### The Vinkius AI Connect SDK, built with Rush.

The same `@vinkius/connect` SDK you get from npm, developed as a Rush monorepo
for teams that build with [Rush](https://rushjs.io).

[![Connectors](https://api.vinkius.com/badges/connectors.svg)](https://vinkius.com)
[![AI Capabilities](https://api.vinkius.com/badges/capabilities.svg)](https://vinkius.com)

[Documentation](https://vinkius.com/learn/en/connect-sdk) · [Vinkius Cloud](https://cloud.vinkius.com) · [Site](https://vinkius.com)

</div>

---

## What is this repo?

This repository hosts **`@vinkius/connect`** — the Vinkius AI Connectivity
Platform SDK — inside a Rush workspace. The published npm package is built
from `packages/connect` and is consumed by any Node.js project exactly the
same way:

```bash
npm install @vinkius/connect
```

This monorepo is for the maintainers and for teams that standardize on Rush:
versioned installs (`rush update`), parallel builds (`rush build`), bulk
tests (`rush test`), change files, and lock-step publishing (`rush publish`).

```
ai-connect-sdk-rush/
├── packages/connect          # @vinkius/connect — the SDK (src, tests, adapters)
├── common/config/rush/       # Rush configuration (pnpm, versioning, policies)
└── rush.json                 # workspace manifest
```

## Prerequisites

- Node.js **20+**
- `@microsoft/rush` **5.181.0** (pinned by the repo)

## Getting started

```bash
npm install -g @microsoft/rush@5.181.0
rush update      # install all project dependencies
rush build       # build @vinkius/connect (ESM + CJS + d.ts via tsup)
rush test        # run the vitest suites (18 test files, fully offline)
rush lint        # eslint across the workspace
```

## Using the SDK

The fluent API — a lazy, zero-request handle per end user:

```ts
import { Vinkius } from '@vinkius/connect';

const vinkius = new Vinkius({
  appId: 'vk_app_...',
  apiKey: 'vk_app_sk_...',
});

const user = vinkius.user('alice_123');
const caps = await user.capabilities();
const result = await caps.findCapability('github__create_issue')?.execute({
  owner: 'acme',
  repo: 'product',
  title: 'Ship it',
});
```

Framework adapters (each subpath is a separate module — install only what
you need; adapters never require their target framework to convert):

```ts
import { toOpenAITools, runOpenAIToolCall } from '@vinkius/connect/openai';
import { toAnthropicTools } from '@vinkius/connect/anthropic';
import { toGeminiTools } from '@vinkius/connect/gemini';
import { toJSONSchemaTools } from '@vinkius/connect/json-schema';
```

Full documentation, examples, and the domain model (connectors, connections,
capabilities, tokens) live in the [SDK documentation](https://vinkius.com/learn/en/connect-sdk)
and in [`packages/connect/README.md`](packages/connect/README.md).

## Versioning and publishing

The workspace uses lock-step versioning via the `connect-sdk` version policy
(`common/config/rush/version-policies.json`):

```bash
rush change     # record the change type (patch/minor/major)
rush version    # bump versions + write CHANGELOG
rush publish    # publish to the npm registry
```

## License

[Apache License 2.0](./LICENSE), Copyright Vinkius.