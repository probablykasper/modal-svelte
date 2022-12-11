<script lang="ts">
	import Modal from '$lib/Modal.svelte'

	let open = false

	let title = 'Text color'
	let noCloseIcon: boolean
	let noEscapeHandling: boolean
</script>

<div class="center">
	<nav>
		<a href="https://github.com/probablykasper/modal-svelte">GitHub</a>
		<a href="https://npmjs.com/package/modal-svelte">NPM</a>
	</nav>

	<h1>Modal Svelte</h1>

	<button class="show" on:click={() => (open = true)}>Show modal</button>

	<div>
		<div class="row">
			<span>title</span>
			<input type="text" bind:value={title} />
		</div>
		<div class="row">
			<span>noCloseIcon</span>
			<input type="checkbox" bind:checked={noCloseIcon} />
		</div>
	</div>
	<div class="row">
		<span>disableEscapeHandling</span>
		<input type="checkbox" bind:checked={noEscapeHandling} />
	</div>
</div>

{#if open}
	<Modal onCancel={() => (open = false)} {noCloseIcon} {title} {noEscapeHandling}>
		<p>
			Aute irure est exercitation eiusmod consequat tempor. Id sint eu velit. Cillum deserunt
			consequat Lorem pariatur commodo ad labore ex irure proident laboris laboris in laboris.
			Officia ex sunt voluptate eu cillum.
		</p>
	</Modal>
{/if}

<style lang="sass">
  :global(html, body)
    margin: 0px
    color: #ffffff
    font-family: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif
  .center
    display: flex
    flex-direction: column
    align-items: center
    min-height: 100vh
    padding: 20px 0px
    box-sizing: border-box
    font-size: 1rem
    background-color: #111318
  nav
    cursor: default
    a
      padding: 4px 2px
      margin: 0px 2px
      font-size: 16px
      text-decoration: none
      color: hsla(0, 100%, 100%, 0.75)
      &:hover
        color: #ffffff
  button.show
    margin-bottom: 20px
  .row
    margin-top: 8px
    display: flex
    align-items: center
    height: 24px
  span
    margin-right: 12px
  input
    margin-left: auto
  p
    max-width: 500px
    margin: 0px
</style>
