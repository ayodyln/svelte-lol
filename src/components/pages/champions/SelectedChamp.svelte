<script lang="ts">
	import { build, buildChampion } from '$lib/stores'
	import Stats from './Stats.svelte'
	import { colorBadgeHandler } from '$lib/league-of-legends/champions'
	import Items from './Items.svelte'
	import ChampionSplashArt from './ChampionSplashArt.svelte'
	import { onMount } from 'svelte'

	export let selectedChampion: any, items: any

	const addChampToBuild = (champion: any) => {
		$build = JSON.stringify(buildChampion)
		const buildObject = JSON.parse($build)
		buildObject.champion = champion
		$build = JSON.stringify(buildObject)
	}

	onMount(() => {})
</script>

<ChampionSplashArt {selectedChampion} />

<section class="p-2">
	<div class="flex justify-between items-center">
		<div class="flex gap-3 items-end">
			<span>
				<h1 class="text-3xl font-bold">{selectedChampion.name}</h1>
				<h3 class="text-xs font-extralight capitalize">{selectedChampion.title}</h3>
			</span>

			{#if !JSON.parse($build).champion.name || JSON.parse($build).champion.name !== selectedChampion.name}
				<button class="btn btn-primary" on:click={() => addChampToBuild(selectedChampion)}
					>Add to Build</button>
			{:else}
				<button class="btn btn-disabled" on:click={() => addChampToBuild(selectedChampion)}
					>Added</button>
			{/if}
		</div>

		<span class="flex gap-3">
			{#each selectedChampion.tags as tag}
				<div class="badge badge-outline badge-lg {colorBadgeHandler(tag)}">{tag}</div>
			{/each}
		</span>
	</div>

	<div class="divider m-0" />

	<div class="flex flex-col gap-4">
		<section id="baseStats" class="flex w-full justify-between">
			<Stats {selectedChampion} />
		</section>

		<section id="abilities" class="flex flex-wrap w-full gap-2">
			<h2 class="text-lg font-bold">Abilities</h2>
			<div class="w-full h-24 bg-neutral-focus rounded-lg flex">
				<div class="h-full rounded overflow-hidden">
					<img
						src="https://ddragon.leagueoflegends.com/cdn/13.8.1/img/passive/{selectedChampion
							.passive.image.full}"
						alt="Tailwind-CSS-Avatar-component"
						class="h-full" />
				</div>

				<div class="p-2 w-3/4">
					<p>Passive - {selectedChampion.passive.name}</p>

					<p class="text-xs">{selectedChampion.passive.description}</p>
				</div>
			</div>
			{#each selectedChampion.spells as spell}
				<div class="w-full h-24 bg-neutral-focus rounded-lg flex">
					<div class="h-full rounded overflow-hidden">
						<img
							src="https://ddragon.leagueoflegends.com/cdn/13.8.1/img/spell/{spell.image.full}"
							alt="Tailwind-CSS-Avatar-component"
							class="h-full" />
					</div>

					<div class="p-2 w-3/4">
						<p class="capitalize">{spell.id.slice(-1)} - {spell.name}</p>

						<p class="text-xs">{spell.description}</p>
					</div>
				</div>
			{/each}
		</section>

		<div id="lore" class="h-auto italic">
			<div class="divider h-0" />
			{selectedChampion.lore}
			<div class="divider h-0" />
		</div>

		<section id="tips" class="flex gap-8">
			<div id="ally" class="flex flex-col gap-2 w-1/2">
				<h2 class="text-success">Ally Tips</h2>
				{#each selectedChampion.allytips as tip}
					<span class="bg-base-100 rounded-lg p-3 border border-success">{tip}</span>
				{/each}
			</div>

			<div id="enemy" class="flex flex-col gap-2 w-1/2">
				<h2 class="text-error">Enemy Tips</h2>
				{#each selectedChampion.enemytips as tip}
					<span class="bg-base-100 rounded-lg p-3 border border-error">{tip}</span>
				{/each}
			</div>
		</section>

		<div class="divider h-0 m-0" />

		<Items {items} {selectedChampion} />
	</div>
</section>
