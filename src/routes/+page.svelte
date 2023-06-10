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
			<p>You {state} the game! play again?</p>
		{:else if state === 'paused'}
			<p>game paused</p>
		{:else if state === 'waiting'}
			<p>Choose a level:</p>
		{/if}

		<div class="buttons">
			{#if state === 'paused'}
				<button on:click={() => game.resume()}>resume</button>

				<button
					on:click={() => {
						state = 'waiting';
					}}>quit</button
				>
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
		text-align: center;
		font-size: 2em;
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

	.buttons {
		display: flex;
		flex-wrap: wrap;
		justify-content: space-around;
	}

	.buttons button {
		font-family: 'Grandstander';
		align-items: center;
		background-color: #fee6e3;
		border: 2px solid #111;
		border-radius: 8px;
		box-sizing: border-box;
		color: #111;
		cursor: pointer;
		display: flex;
		font-size: 16px;
		height: 48px;
		justify-content: center;
		line-height: 24px;
		max-width: 100%;
		padding: 0 25px;
		position: relative;
		text-align: center;
		text-decoration: none;
		user-select: none;
		-webkit-user-select: none;
		touch-action: manipulation;
	}

	.buttons button:after {
		background-color: #111;
		border-radius: 8px;
		content: '';
		display: block;
		height: 48px;
		left: 0;
		width: 100%;
		position: absolute;
		top: -2px;
		transform: translate(8px, 8px);
		transition: transform 0.2s ease-out;
		z-index: -1;
	}

	.buttons button:hover::after {
		transform: translate(0, 0);
	}

	.buttons button:active {
		background-color: #ffdeda;
		outline: 0;
	}

	.buttons button:main,
	.buttons button:focus {
		user-select: auto;
	}

	.buttons button:hover {
		outline: 0;
	}
</style>
