<script lang="ts">
	import { onMount } from 'svelte';

	import type { RealtimeChannel } from '@supabase/supabase-js';
	import type { Message } from '@/types/message';

	import ChatInput from './chat-input.svelte';
	import Messages from './messages.svelte';

	export let data;
	$: ({ supabase, session } = data);

	let channel: RealtimeChannel;

	let messages: Message[] = [];

	onMount(() => {
		channel = supabase.channel('default');

		channel
			.on('broadcast', { event: 'message' }, ({ event, payload, type }) => {
				messages = [...messages, payload];
			})
			.subscribe();

		return () => {
			supabase.removeChannel(channel);
		};
	});

	async function onSubmit(input: string) {
		if (!session?.user) return;

		const message: Message = {
			userId: session.user.id,
			content: input
		};

		messages = [...messages, message];

		// const { data, error } = await supabase.from('messages').insert({
		// 	user_id: message.userId,
		// 	content: message.content
		// });

		// if (error) {
		// 	console.error(error);
		// }

		channel.send({
			type: 'broadcast',
			event: 'message',
			payload: message
		});
	}
</script>

<div class="container flex h-dvh max-w-[1024px] flex-col">
	<div class="flex-1">
		<Messages {messages} />
	</div>
	<ChatInput {onSubmit} />
</div>
