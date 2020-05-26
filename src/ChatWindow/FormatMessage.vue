<template>
	<div>
		<div v-if="textFormatting">
			<template v-for="(message, i) in linkifiedMessage">
				<component
					:is="message.types.indexOf('url') !== -1 ? 'a' : 'span'"
					:key="i"
					:class="{
						'text-bold': message.types.indexOf('bold') !== -1,
						'text-italic': message.types.indexOf('italic') !== -1,
						'text-deleted': deleted,
						'text-strike': message.types.indexOf('strike') !== -1,
						'text-underline': message.types.indexOf('underline') !== -1
					}"
					:href="message.href"
					target="_blank"
					>{{ message.value }}</component
				>
			</template>
		</div>
		<div v-else>{{ content }}</div>
	</div>
</template>

<script>
import formatString from '../utils/formatString'

export default {
	name: 'format-message',

	props: {
		content: { type: [String, Number], required: true },
		deleted: { type: Boolean, default: false },
		formatLinks: { type: Boolean, default: true },
		textFormatting: { type: Boolean, required: true }
	},

	computed: {
		linkifiedMessage() {
			return formatString(this.content, this.formatLinks)
		}
	}
}
</script>

<style>
.text-deleted {
	font-style: italic;
}
</style>