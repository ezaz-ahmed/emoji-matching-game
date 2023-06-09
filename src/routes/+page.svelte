<script lang="ts">
	import { confetti } from '@neoconfetti/svelte';

	import Game from './Game.svelte';
	import '../style.css';
	import Modal from './Modal.svelte';
	import { levels } from './levels';

	let state: 'waiting' | 'playing' | 'paused' | 'won' | 'lost' = 'waiting';

	let game: Game;
</script>

<Game
	bind:this={game}
	on:play={() => {
		state = 'playing';
	}}
	on:pause={() => {
		state = 'paused';
	}}
	on:win={() => {
		state = 'won';
	}}
	on:lose={() => {
		state = 'lost';
	}}
/>

{#if state !== 'playing'}
	<Modal>
		<header>
			<h1><span>Emoji</span> Matching Game</h1>
		</header>

		{#if state === 'won' || state === 'lost'}
			<p>You {state} the game!</p>
		{:else if state === 'paused'}
			<p>game paused</p>
		{:else if state === 'waiting'}
			<p>Choose a level:</p>
		{/if}

		<div class="buttons">
			{#if state === 'paused'}
				<button> resume </button>

				<button> quit </button>
			{:else}
				{#each levels as level}
					<button on:click={() => game.start(level)}>{level.label}</button>
				{/each}
			{/if}
		</div>
	</Modal>
{/if}

{#if state === 'won'}
	<div
		class="confetti"
		use:confetti={{
			stageWidth: innerWidth,
			stageHeight: innerHeight
		}}
	/>
{/if}

<style>
	h1 {
		font-size: 3em;
	}

	h1 span {
		color: purple;
	}

	p {
		font-family: 'Grandstander';
	}

	.confetti {
		position: fixed;
		width: 100%;
		height: 100%;
		left: 50%;
		top: 30%;
		pointer-events: none;
	}
</style>
