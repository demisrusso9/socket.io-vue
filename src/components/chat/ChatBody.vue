<script lang="ts" setup>
	import { defineProps } from 'vue'
	import { Message } from '../../types/message'

	interface ChatBody {
		contactMessage: string
		myMessage: string
		username: string
		list: Message[]
	}

	const props = defineProps<ChatBody>()
</script>

<template>
	<div class="bg-gray-100 overflow-auto h-96 w-full flex flex-col">
		<section
			v-for="(message, i) in list"
			:key="i"
			:class="message.username === props.username ? 'you' : 'other'"
			class="flex flex-col"
		>
			<div
				:class="message.username === props.username ? 'bg-green-300' : 'bg-blue-300'"
				class="m-2 p-2 rounded-lg shadow-lg w-min"
			>
				<div>{{ message.message }}</div>

				<small class="text-xs">{{ message.time }}</small>
			</div>
		</section>
	</div>
</template>
