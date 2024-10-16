<script lang="ts">
	import type { Message } from '@/types/message';

	export let messages: Message[];
</script>

<div class="m-auto max-w-lg">
	{#each messages as message}
		<div>
			{message.content}
		</div>
	{/each}
</div>
