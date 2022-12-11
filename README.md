# Modal Svelte

Features:

- Autofocus
- Focus trapping
- Returns focus after closing
- Form mode

## Install

```
npm install color-picker-svelte
```

## Usage

```svelte
<script>
  import Modal from 'color-picker-svelte'
</script>

<Modal
  title="Hello world!"
  onCancel={() => (open = false)}
>
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
  let:focus
>
  <p>Content</p>
  <input />

  <!-- If you want to bring focus to a specific element -->
  <input use:focus />

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
    npm publish ./package
    ```
6. Commit with a tag in format "v#.#.#"
7. Create GitHub release with release notes
