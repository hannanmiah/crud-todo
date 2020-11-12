<template>
	<div class="modal">
		<transition name="modal">
			<div class="content" v-if="open">
				<div class="header"><slot name="header">Default Header</slot></div>
				<div class="body"><slot>Default content goes here</slot></div>
				<div class="footer">
					<slot name="footer">The Default Footer</slot>
				</div>
			</div>
		</transition>
	</div>
</template>

<script>
import { onMounted, onUnmounted, onUpdated, ref } from 'vue';
	export default {
		props: ["modalIsVisible"],
		setup(props) {
      const open = ref(false)

      onMounted(() => open.value = props.modalIsVisible)

      return {open}
    },
	};
</script>

<style scoped>
	.modal {
		position: fixed;
		top: 0;
		bottom: 0;
		z-index: 1;
		height: 100vh;
		width: 100vw;
		background: rgba(0, 0, 0, 0.5);
	}

	.modal .content {
		position: absolute;
		height: auto;
		width: 50vw;
		left: 20%;
		top: 20%;
		background: white;
		color: black;
	}

	.modal .content .header {
		padding: 5px;
		height: 20%;
		font-weight: bold;
		text-align: center;
		border-bottom: 1px solid #ddd;
	}

	.modal .content .body {
		padding: 10px;
		height: 60%;
	}

	.modal .content .footer {
		display: flex;
		justify-content: flex-end;
		height: 20%;
		border-top: 1px solid #ddd;
		padding: 10px;
	}

	.modal-enter-from {
		opacity: 0;
		transform: translateY(-30px);
	}
	.modal-enter-active {
		transition: all 0.3s ease-in;
	}
	.modal-enter-to {
		opacity: 1;
		transform: translateY(0);
	}

  .modal-leave-from {
    opacity: 1;
    transform: translateY(0);
  }

  .modal-leave-active {
    transition: all 0.3s ease-in;
  }

  .modal-leave-to {
    opacity: 0;
    transform: translateY(-30px);
  }
</style>