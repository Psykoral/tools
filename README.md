# @flippydisk/tools

Small, framework-independent JavaScript utilities for browsers and Node.js. Version 2 uses native platform APIs, publishes ESM and CommonJS, and has no runtime dependencies.

## Requirements

- Node.js 22 or newer for Node consumers and development
- A modern browser with ES modules, `URLSearchParams`, and Fetch for browser use

The library has no React, Vue, Svelte, Next.js, or bundler dependency. Those tools consume the same package exports as plain JavaScript.

## Install

```sh
npm install @flippydisk/tools
```

## Debug

```js
import { Debug } from '@flippydisk/tools';

const debug = new Debug({
	debug: true,
	control: 'Application',
});

debug.log('Started');
debug.info('Ready');
debug.warn('Slow response');
debug.error('Request failed');
debug.debug({ state: 'active' });
debug.table([{ name: 'item' }]);
debug.trace('Trace point');
```

The direct ESM and CommonJS entry points preserve the established constructor shape:

```js
import Debug from '@flippydisk/tools/debug';
const { Debug } = require('@flippydisk/tools');
const DebugDirect = require('@flippydisk/tools/debug');
```

Set `debug: false` and activate a control from the browser URL:

```text
?debug=Application
?debug=All
?debug=Application&debug=AnotherControl
```

Explicit debugging works in Node.js and server-rendered applications without browser globals.

## Disable

`Disable` provides a small URL-controlled feature switch and is safe to construct in Node.js:

```js
import { Disable } from '@flippydisk/tools';

const feature = new Disable({ control: 'ExperimentalPanel' });
if (!feature.isDisabled()) {
	// Run the feature.
}
```

Use `?disable=ExperimentalPanel` to disable it in a browser, or pass `disable: true` explicitly.

## Fetch helper

`ajax` uses the platform Fetch implementation. It forwards request options and `AbortSignal`, parses JSON responses by content type, returns text for other content types, and returns `null` for empty responses.

```js
import { ajax } from '@flippydisk/tools/json';

const controller = new AbortController();
const data = await ajax('/api/items', undefined, {
	method: 'GET',
	signal: controller.signal,
});
```

HTTP failures reject with an `HTTPError` containing `status`, `statusText`, `url`, `response`, and parsed `data`. Network, abort, and malformed JSON errors also reject normally. An optional callback remains supported and its return value becomes the resolved result.

## Language utilities

```js
import * as Lang from '@flippydisk/tools/lang';

Lang.getPropertySafely({ user: { name: 'Ada' } }, 'user.name');
Lang.isNullish(value);
Lang.isValidObject(value);
```

`filterHTMLTags` strips tag-shaped text. It is not an HTML sanitizer and must not be used as a security boundary for untrusted markup.

## Native browser modules

Serve the ESM distribution from a valid module URL. Browsers do not resolve bare npm package names without an import map or bundler.

```html
<script type="module">
	import Debug from '/node_modules/@flippydisk/tools/dist/esm/debug/debug.js';

	const debug = new Debug({ debug: true, control: 'BrowserApp' });
	debug.log('Native browser module loaded');
</script>
```

Vite, Next.js, Rollup, webpack, React, Vue, and Svelte can use the package-name imports shown above.

## Development

```sh
npm run lint
npm run lint:fix
npm test
npm run build
npm run docs
npm run check
```

`npm run check` runs lint, tests, the build, package export tests, and an npm package dry run. Builds copy native ESM and use esbuild only for CommonJS compatibility.

See [MIGRATION.md](./MIGRATION.md) when upgrading from 1.x and [CHANGELOG.md](./CHANGELOG.md) for release changes.
