<template>
	<div class="tip">
		<div class="tip-title">
			<div class="info-section">
				<span v-if="canModify" class="event-action" @click.prevent="editTip">
					<img :src="`/images/edit.svg`" class="icon-button" alt="edit" />
				</span>
				<span v-if="canModify" class="event-action" @click.prevent="deleteTip">
					<img :src="`/images/delete.svg`" class="icon-button" alt="delete" />
				</span>
			</div>
		</div>
		<Tweet :id="tweetId" class="tip-wrapper">
			<div class="spinner">
				loading...
			</div>
		</Tweet>
		<div class="tool-tip-tags">
			{{ tags }}
		</div>
	</div>
</template>

<script>
import { Tweet } from 'vue-tweet-embed';

export default {
	name: 'TipStrip',
	components: {
		Tweet
	},
	props: {
		tip: {
			type: Object,
			required: true
		}
	},
	computed: {
		signedInUser() {
			return this.$store.state.signedInUser;
		},
		canModify() {
			return (
				this.$store.getters.isAdmin ||
				(this.signedInUser &&
					this.signedInUser.username === this.tip.createdBy.username)
			);
		},
		tweetId() {
			return this.tip.twitterLink.split('/status/')[1];
		},
		tags() {
			return this.tip.tags.map(tip => `#${tip}`).join(' ');
		}
	},
	methods: {
		deleteTip() {
			this.$emit('delete', this.tip._id);
		},
		editTip() {
			this.$router.push(`/tip/form/${this.tip._id}`);
		}
	}
};
</script>

<style scoped lang="scss">
.tip {
	border-bottom: dotted 1px #114273;
	padding: 10px;
	position: relative;
	min-width: 50%;
}

@media screen and (max-width: 700px) {
	.tip {
		min-width: 300px;
	}
}

.tip-title {
	display: flex;
	justify-content: flex-end;
}

.tool-tip-tags {
	color: rgb(27, 149, 224);
}

.tip-wrapper {
	min-height: 200px;
}
</style>
