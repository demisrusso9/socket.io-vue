<script lang="ts" setup>
	import { ref, defineEmits } from 'vue'

	const emit = defineEmits(['update:modelValue', 'sendMessageClear'])

	const input = ref('')

	const clear = (e: KeyboardEvent) => {
		if (e.key !== 'Enter') return

		emit('sendMessageClear')
		input.value = ''
	}
</script>

<template>
	<input
		class="rounded-md shadow-md outline-none p-2"
		type="text"
		v-model="input"
		:onkeypress="clear"
		@input="$emit('update:modelValue', ($event.target as HTMLInputElement).value)"
	/>
</template>
