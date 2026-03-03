<script lang="ts">
	import CollapsibleSection from '@/ui/settings/svelte/sections/CollapsibleSection.svelte';
	import { createEventDispatcher } from 'svelte';
	import { settingsStore } from '@/ui/settings/settingsstore';

	export let open = false;
	export let plugin;

	const dispatch = createEventDispatcher();

	let hideNoteLinksInTickTick = false;
	let filterTickTickTagOnSync = false;

	$: hideNoteLinksInTickTick = $settingsStore.hideNoteLinksInTickTick ?? false;
	$: filterTickTickTagOnSync = $settingsStore.filterTickTickTagOnSync ?? false;

	function handleHeaderClick() {
		dispatch('toggle');
	}

	async function handleHideNoteLinksChange(checked: boolean) {
		settingsStore.update((s) => ({ ...s, hideNoteLinksInTickTick: checked }));
		await plugin.saveSettings();
	}

	async function handleFilterTagChange(checked: boolean) {
		settingsStore.update((s) => ({ ...s, filterTickTickTagOnSync: checked }));
		await plugin.saveSettings();
	}
</script>

<CollapsibleSection
	title="TickTick behavior"
	shortDesc="Outbound sync options"
	open={open}
	on:headerClick={handleHeaderClick}
>
	<div class="setting-item">
		<div class="setting-item-info">
			<div class="setting-item-name">Hide note links in TickTick</div>
			<div class="setting-item-description">
				Do not include the Obsidian file link in TickTick task description/content when link destination is set to Description.
			</div>
		</div>
		<div class="setting-item-control">
			<label class="toggle-switch">
				<input
					type="checkbox"
					bind:checked={hideNoteLinksInTickTick}
					on:change={async (e) => {
						await handleHideNoteLinksChange((e.target as HTMLInputElement).checked);
					}}
				/>
				<span class="slider"></span>
			</label>
		</div>
	</div>

	<div class="setting-item">
		<div class="setting-item-info">
			<div class="setting-item-name">Filter out #ticktick tag when syncing to TickTick</div>
			<div class="setting-item-description">
				Exclude the internal #ticktick marker from tags sent to TickTick.
			</div>
		</div>
		<div class="setting-item-control">
			<label class="toggle-switch">
				<input
					type="checkbox"
					bind:checked={filterTickTickTagOnSync}
					on:change={async (e) => {
						await handleFilterTagChange((e.target as HTMLInputElement).checked);
					}}
				/>
				<span class="slider"></span>
			</label>
		</div>
	</div>
</CollapsibleSection>
