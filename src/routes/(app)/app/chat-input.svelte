<script lang="ts">
	import { autosize } from '@/components/actions';

	export let onSubmit: ((input: string) => void) | undefined = undefined;

	let input: string = '';

	async function handleKeydown(event: KeyboardEvent) {
		if (event.key === 'Enter' && !event.shiftKey) {
			event.preventDefault();

			if (onSubmit) {
				await onSubmit(input);
			}

			input = '';
		}
	}
</script>

<div class="py-6">
	<div
		class="m-auto flex w-full max-w-lg items-center rounded-lg border bg-secondary p-2 shadow-sm"
	>
		<textarea
			rows="1"
			class="h-full w-full resize-none bg-transparent outline-none"
			placeholder="Write your message..."
			bind:value={input}
			use:autosize={input}
			on:keydown={handleKeydown}
		></textarea>
	</div>
</div>
