<script lang="ts" context="module">
	import { writable } from 'svelte/store'

	export const modalCount = writable(0)
</script>

<script lang="ts">
	import { onDestroy } from 'svelte'
	import { checkShortcut } from './shortcuts'
	import { fade, scale } from 'svelte/transition'
	import { cubicOut } from 'svelte/easing'

	export let onCancel: () => void
	export let noEscapeHandling = false
	export let form: (() => void) | undefined = undefined
	export let noCloseIcon = true
	$: tag = form === undefined ? 'div' : 'form'
	export let title: string | null = null

	$modalCount += 1
	onDestroy(() => {
		$modalCount -= 1
	})

	let lastActiveElement: Element | null = null

	function focus(el: HTMLElement) {
		if (lastActiveElement === null) {
			lastActiveElement = document.activeElement
		}
		el.focus()
	}
	function focusDefault(el: HTMLDivElement) {
		if (!el.contains(document.activeElement)) {
			focus(el)
		}
	}

	function focusTrap(el: HTMLElement) {
		function getFocusElements() {
			return el.querySelectorAll(
				'button, [href], input, select, textarea, [tabindex]:not([tabindex="-1"])'
			)
		}

		if (lastActiveElement === null) {
			lastActiveElement = document.activeElement || document.body
			el.focus()
		}

		function handleKeydown(e: KeyboardEvent) {
			if (checkShortcut(e, 'Tab', { shift: true })) {
				const focusElements = getFocusElements()
				const lastFocusElement = focusElements[focusElements.length - 1]
				if (focusElements.length === 0) {
					e.preventDefault()
				} else if (
					document.activeElement?.isSameNode(focusElements[0]) &&
					lastFocusElement instanceof HTMLElement
				) {
					lastFocusElement.focus()
					e.preventDefault()
				}
			} else if (checkShortcut(e, 'Tab')) {
				const focusElements = getFocusElements()
				const lastFocusElement = focusElements[focusElements.length - 1]
				if (focusElements.length === 0) {
					e.preventDefault()
				} else if (
					document.activeElement?.isSameNode(lastFocusElement) &&
					focusElements[0] instanceof HTMLElement
				) {
					focusElements[0].focus()
					e.preventDefault()
				}
			} else if (checkShortcut(e, 'Escape') && !noEscapeHandling) {
				onCancel()
			}
		}
		el.addEventListener('keydown', handleKeydown)
		return {
			destroy() {
				el.removeEventListener('keydown', handleKeydown)
				if (lastActiveElement instanceof HTMLElement) {
					lastActiveElement.focus()
				}
			},
		}
	}

	function boxKeydown(e: KeyboardEvent) {
		if (form && checkShortcut(e, 'Enter')) {
			form()
			e.preventDefault()
		}
	}
</script>

<svelte:element this={tag} class="modal overlay" on:submit|preventDefault={form} on:keydown>
	<!-- svelte-ignore a11y-click-events-have-key-events -->
	<div
		class="backdrop overlay"
		on:click={onCancel}
		on:mousedown|preventDefault
		transition:fade={{ duration: 200, easing: cubicOut }}
	/>
	<div
		class="box"
		use:focusTrap
		use:focusDefault
		tabindex="-1"
		on:keydown|self={boxKeydown}
		transition:scale={{ duration: 200, start: 0.9, opacity: 0, easing: cubicOut }}
	>
		{#if noCloseIcon}
			<!-- svelte-ignore a11y-click-events-have-key-events -->
			<svg
				on:click={() => onCancel()}
				fill="currentColor"
				xmlns="http://www.w3.org/2000/svg"
				width="12"
				height="12"
				viewBox="0 0 24 24"
				><path
					d="M23.954 21.03l-9.184-9.095 9.092-9.174-2.832-2.807-9.09 9.179-9.176-9.088-2.81 2.81 9.186 9.105-9.095 9.184 2.81 2.81 9.112-9.192 9.18 9.1z"
				/></svg
			>
		{/if}
		{#if title !== null}
			<h2>{title}</h2>
		{/if}
		<slot {focus} />
		{#if $$slots.buttons}
			<div class="buttons">
				<slot name="buttons" />
			</div>
		{/if}
	</div>
</svelte:element>

<style lang="sass">
	.modal
		position: relative
		display: flex
		align-items: center
		justify-content: center
		user-select: text
		z-index: 90
	.overlay
		position: fixed
		top: 0
		left: 0
		bottom: 0
		right: 0
		padding: 1.25rem
		box-sizing: border-box
	.backdrop
		background-color: hsla(0, 0%, 0%, 0.5)
	.box
		background-color: var(--modal-bg, hsl(220, 18%, 11%))
		position: relative
		border: 1px solid hsla(0, 0%, 100%, 0.15)
		max-width: 100%
		max-height: 100%
		padding: 1.5rem
		box-sizing: border-box
		border-radius: 8px
		box-shadow: 0px 0px 30px 0px rgba(#000000, 0.5)
		overflow: auto
		z-index: 100
		outline: none
	svg
		position: absolute
		right: 0.75rem
		top: 0.75rem
		padding: 0.4rem
		&:hover
			opacity: 0.7
	h2
		margin-top: -0.5rem
	.buttons
		display: flex
		justify-content: flex-end
</style>
