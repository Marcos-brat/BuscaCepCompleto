<template>
	<div class="popup">
		<div class="popup-inner">
			<div class="popup-header">
				CEP Clicado -> {{cep}}
			</div>
		</div>
	</div>
</template>

<script>
export default {
	props: {['cep']: String},

};
</script>

<style lang="scss" scoped>
.popup {
	position: fixed;
	top: 0;
	left: 0;
	right: 0;
	bottom: 0;
	z-index: 99;
	background-color: rgba(0, 0, 0, 0.2);

	display: flex;
	align-items: center;
	justify-content: center;

	.popup-inner {
		background: #FFF;
		padding: 32px;
	}
}
</style>