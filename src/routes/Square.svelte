<script lang="ts">
	import { send } from './transition';
	import { get_twemoji_url } from './utils';

	export let emoji: string;
	export let selected: boolean;
	export let found: boolean;
	export let group: 'a' | 'b';
</script>

<div class="square" class:flipped={selected || found}>
	<button on:click />

	<div class="background" />

	{#if !found}
		<img out:send={{ key: `${emoji}:${group}` }} alt={emoji} src={get_twemoji_url(emoji)} />
	{/if}
</div>

<style>
	.square {
		display: flex;
		justify-content: center;
		align-items: center;
		transform-style: preserve-3d;
		transition: transform 0.4s;
	}

	.flipped {
		transform: rotateY(180deg);
	}

	.background {
		position: absolute;
		width: 100%;
		height: 100%;
		background: white;
		border: 0.5em solid #eee;
		border-radius: 1em;
		transform: rotateY(180deg);
		backface-visibility: hidden;
	}

	button {
		position: absolute;
		width: 100%;
		height: 100%;
		background: #eee;
		border: none;
		border-radius: 1em;
		backface-visibility: hidden;
		font-size: inherit;
	}

	img {
		width: 6em;
		height: 6em;
		pointer-events: none;
		transform: rotateY(180deg);
		backface-visibility: hidden;
	}
</style>
