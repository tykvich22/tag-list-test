<template>
	<v-app>
		<v-main>
			<v-container fluid class="pa-0"
				>100% ширина
				<div :class="[`${bemBlock}`, `${bemBlock}__full`]">
					<TagList :data="data" alignment="start" />
					<TagList :data="data" alignment="space-between" />
				</div>
				50% ширина
				<div :class="[`${bemBlock}`, `${bemBlock}__half`]">
					<TagList :data="data" alignment="start" />
					<TagList :data="data" alignment="space-between" />
				</div> </v-container
		></v-main>
	</v-app>
</template>

<script>
import TagList from './components/TagList.vue';

export default {
	name: 'App',

	components: {
		TagList,
	},

	data: () => ({
		bemBlock: 'width-container',
		data: [
			{ icon: 'mdi-star-outline', text: '4,2' },
			{ text: 'Фридландские ворота' },
			{ text: '1,5 км от вас' },
		],
	}),
};
</script>

<style lang="scss">
.width-container {
	height: 250px;
	border: 1px solid black;
	&__full {
		width: 100%;
	}
	&__half {
		width: 50%;
	}
}
</style>
