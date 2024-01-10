# Modal Svelte

Modal component for Svelte

[![NPM](https://img.shields.io/npm/v/modal-svelte.svg)](https://npmjs.com/package/modal-svelte)
[![License](https://img.shields.io/npm/l/modal-svelte.svg)](LICENSE)
[![NPM Downloads](https://img.shields.io/npm/dm/modal-svelte.svg)](https://npmjs.com/package/modal-svelte)
[![Test](https://github.com/probablykasper/modal-svelte/actions/workflows/test.yml/badge.svg)](https://github.com/probablykasper/modal-svelte/actions/workflows/test.yml)

Features:

- Autofocus
- Focus trapping
- Returns focus after closing
- Form mode

[Demo](https://modal-svelte.kasper.space/)

## Install

```
npm install modal-svelte
```

## Usage

```svelte
<script>
	import Modal from 'modal-svelte'
</script>

<Modal title="Hello world!" onCancel={() => (open = false)}>
	<p>Content</p>
</Modal>
```

### Functionality

This shows all available functionality

```svelte
<Modal
	title="Hello world!"
	onCancel={() => (open = false)}
	form={() => submitMyForm()}
	noEscapeHandling
	noCloseIcon
	class="w-full"
>
	<p>Content</p>
	<input />

	<!-- If you want to bring focus to a specific element -->
	<input autofocus />

	<div slot="buttons">
		<button type="submit">Submit</button>
	</div>

</Modal>
```

```css
:root {
	--modal-bg: #fff;
	color: #000;
}
```

## Dev instructions

### Get started

1. Install Node.js
2. Run `npm install`

### Commands

- `npm run dev`: Start in dev mode
- `npm run build`: Build
- `npm run lint`: Lint
- `npm run format`: Format

### Publish new version

1. Update `CHANGELOG.md`
2. Check for errors
	```
	npm run lint
	```
3. Bump the version number
	```
	npm version --no-git-tag <version>
	```
4. Generate the package
	```
	npm run build:package
	```
5. Publish the package
	```
	npm publish
	```
6. Commit with a tag in format "v#.#.#"
7. Create GitHub release with release notes
