<script lang="ts">
	import * as Command from '$lib/components/ui/command/index.js';
	import { Button } from '$lib/components/ui/button/index.js';
	import data from '../../data/data.json';

	let shortcuts = $derived(data);

	let open: boolean = $state(false);

	function handleClickCommandItem(index: string) {
		open = false;
		const el = document.querySelector(`[data-action='${index}'`);
		el?.scrollIntoView({ behavior: 'smooth' });
		el?.classList.add('highlight');
		setTimeout(() => {
			el?.classList.remove('highlight');
		}, 2000); // Match the animation duration
	}
</script>

<Button
	variant="ghost"
	class="border border-accent text-muted-foreground "
	onclick={() => (open = !open)}>
	Search shortcuts...
</Button>

<Command.Dialog bind:open>
	<Command.Input placeholder="Search shortcuts..." />
	<Command.List class="custom-scroll">
		<Command.Empty>No results found.</Command.Empty>
		{#each Object.entries(shortcuts) as [category, actions], indexGroup}
			<Command.Group heading={category}>
				{#each actions as action, indexAction}
					<Command.Item
						onSelect={() => handleClickCommandItem(`${indexGroup}-${indexAction}`)}
						class="cursor-pointer">
						{action.action}
					</Command.Item>
				{/each}
			</Command.Group>
			<Command.Separator />
		{/each}
	</Command.List>
</Command.Dialog>

<style lang="postcss">
	:global(.custom-scroll::-webkit-scrollbar) {
		@apply h-2.5 w-2.5;
	}

	:global(.custom-scroll::-webkit-scrollbar-track) {
		@apply bg-accent;
	}
	:global(.custom-scroll::-webkit-scrollbar-thumb) {
		@apply rounded-full border-[1px] border-solid border-transparent bg-primary bg-clip-padding;
	}

	:global(.highlight) {
		border: 1px solid hsl(var(--destructive));
	}
</style>
