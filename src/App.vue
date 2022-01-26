<script lang="ts" setup>
	import { ref } from 'vue'
	import { io } from 'socket.io-client'
	import Input from './components/ui/Input.vue'
	import Button from './components/ui/Button.vue'
	import Card from './components/ui/Card.vue'
	import Container from './components/ui/Container.vue'
	import Title from './components/ui/Title.vue'
	import Chat from './components/chat/Chat.vue'

	const loggedIn = ref(false)
	const username = ref('')
	const room = ref('')

	const socket = io('127.0.0.1:3000')

	socket.on('connect', () => {
		console.log(`Client connected: ${socket.id}`)
	})

	const connectToRoom = () => {
		if (!username.value || !room.value) return

		socket.emit('test_room', room.value)
		loggedIn.value = true
	}
</script>

<template>
	<Container>
		<Title v-show="!loggedIn" class="text-4xl mb-5">Socket IO</Title>

		<Card v-show="!loggedIn">
			<p class="text-white">Username: {{ username }}</p>
			<p class="text-white">Room: {{ room }}</p>

			<div class="flex gap-4 my-4">
				<Input v-model="username" placeholder="Username..." />
				<Input v-model="room" placeholder="Room..." />
			</div>

			<Button @click="connectToRoom">Enter Room</Button>
		</Card>

		<Chat v-show="loggedIn" :socket="socket" :username="username" :room="room" />
	</Container>
</template>
