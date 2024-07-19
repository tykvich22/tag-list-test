<template>
	<v-row :justify="alignment" ref="rowRef">
		<v-col
			:class="[`${bemBlock}__tag`]"
			v-for="(tag, index) in tags"
			:key="index"
			xs="auto"
			sm="auto"
			md="auto"
			lg="auto"
			xl="auto"
			:ref="(el) => setTagsRef(el, index)"
		>
			<div>
				<v-icon :class="[`${bemBlock}__star__icon`]" small v-if="tag.icon">
					{{ tag.icon }}
				</v-icon>
				{{ tag.text }}
				<v-icon
					v-if="index < tags.length - 1"
					:class="[`${bemBlock}__circle__icon`]"
					>mdi-circle-small</v-icon
				>
			</div>
		</v-col>
	</v-row>
</template>

<script>
export default {
	name: 'TagList',
	props: {
		data: Array,
		alignment: String,
	},
	data() {
		return {
			bemBlock: 'tag-list',
			tagsRefs: [],
			rowWidth: 0,
			tags: [],
		};
	},
	methods: {
		calculateRowWidth() {
			if (this.$refs.rowRef) {
				this.rowWidth = this.$refs.rowRef.offsetWidth;
			}
		},
		setTagsRef(el, index) {
			if (el) {
				this.tagsRefs[index] = el;
			}
		},
		setVisibleTags() {
			let currentWidth = 15;
			const visibleTags = [];
			for (let i = 0; i < this.tagsRefs.length; i++) {
				const tagWidth = this.tagsRefs[i].offsetWidth;
				if (currentWidth + tagWidth <= this.rowWidth) {
					currentWidth += tagWidth;
					visibleTags.push(this.data[i]);
				} else {
					break;
				}
			}
			return visibleTags;
		},
		updateTags() {
			this.calculateRowWidth();
			const newTags = this.setVisibleTags();

			this.tags = [...newTags];
		},
		debounce(func, wait) {
			let timeout;
			return () => {
				clearTimeout(timeout);
				timeout = setTimeout(func, wait);
			};
		},
	},
	created() {
		this.tags = [...this.data];
	},
	mounted() {
		this.$nextTick(() => {
			this.updateTags();
			window.addEventListener('resize', this.debounce(this.updateTags, 150));
		});
	},
	beforeDestroy() {
		window.removeEventListener('resize', this.debounce(this.updateTags, 150));
	},
};
</script>

<style lang="scss" scoped>
.tag-list {
	&__tag {
		white-space: nowrap;
		flex-wrap: nowrap;
	}
	&__star__icon {
		transform: translateY(-0.1rem);
	}
}
</style>
