<script lang="ts" setup>
	import { ref, toRefs, defineProps, computed, watchEffect, reactive } from 'vue'
	import { Socket } from 'socket.io-client'
	import { Message } from '../../types/message'
	import Input from '../ui/Input.vue'
	import Button from '../ui/Button.vue'
	import ChatBody from './ChatBody.vue'
	import ChatHeader from './ChatHeader.vue'
	import ChatFooter from './ChatFooter.vue'

	const props = defineProps<{
		socket: Socket
		username: string
		room: string
	}>()

	const { socket, room, username } = toRefs(props)

	const messageList = reactive([] as Message[])
	const message = ref('')
	const contact = reactive({
		name: '',
		message: '',
		time: ''
	})

	const datetime = computed(
		() =>
			new Date().getHours().toString().padStart(2, '0') +
			':' +
			new Date().getMinutes().toString().padStart(2, '0')
	)

	const sendMessage = () => {
		if (!message.value) return

		const messageData = {
			username: username?.value,
			room: room?.value,
			message: message.value,
			time: datetime.value
		}

		socket?.value?.emit('send_message', messageData)
		messageList.push(messageData as Message)
	}

	watchEffect(() => {
		socket?.value?.on('receive_message', (data: Message) => {
			messageList.push(data)
			contact.name = data.username
			contact.message = data.message
			contact.time = data.time
		})
	})
</script>

<template>
	<div class="flex flex-col flex-1">
		<ChatHeader :contact="contact.name" />

		<ChatBody
			:contact-message="contact.message"
			:my-message="message"
			:list="messageList"
			:username="username"
		/>

		<ChatFooter>
			<Input v-model="message" @sendMessageClear="sendMessage" />
			<Button @click="sendMessage">Send</Button>
		</ChatFooter>
	</div>
</template>
