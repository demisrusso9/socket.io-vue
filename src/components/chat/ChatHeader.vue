<script lang="ts" setup>
import { ref, reactive, defineProps, computed } from 'vue'

const props = defineProps({
	contact: String
})

const initials = computed(() => props.contact?.slice(0, 2))
</script>

<template>
	<section class="my-1 flex justify-start items-center bg-blue-400 p-2 gap-x-2 teste">
		<h1 class="flex justify-center items-center rounded-full h-10 w-10 bg-white">
			{{ initials }}
		</h1>

		<span>{{ contact }}</span>
	</section>
</template>
