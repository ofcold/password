<template>
	<div id="ofcold-dot-spinner" class="ofcold-input-inline-spinner">
		<div class="inline-spinner">
			<div class="inline-spinner-element"></div>
			<div class="inline-spinner-element mod-second"></div>
			<div class="inline-spinner-element mod-third"></div>
		</div>
	</div>
</template>
<script>
	export default {
		
	}
</script>

<style lang="scss" scoped>
	.ofcold-input-inline-spinner {
		position: absolute;
		right: 4px;
		top: 6px;

		.inline-spinner {
			counter-reset: dot;
			display: inline-block;

			.inline-spinner-element {
				background-color: #707070;
				-webkit-border-radius: 8px;
				border-radius: 8px;
				display: block;
				float: left;
				height: 8px;
				line-height: 8px;
				width: 8px
			}

			.inline-spinner-element + .inline-spinner-element {
				margin-left: 4px
			}

			.inline-spinner-element {
				opacity: .4;
				-webkit-transform: scale(.4);
				-ms-transform: scale(.4);
				transform: scale(.4);
				-webkit-animation: c .8s infinite linear;
				animation: c .8s infinite linear;

				&.mod-second {
					-webkit-animation-name: d;
					animation-name: d;
					opacity: .65;
					-webkit-transform: scale(.65);
					-ms-transform: scale(.65);
					transform: scale(.65)
				}

				&.mod-third {
					-webkit-animation-name: e;
					animation-name: e;
					opacity: 1;
					-webkit-transform: scale(1);
					-ms-transform: scale(1);
					transform: scale(1)
				}
			}
		}
	}

	@-webkit-keyframes c {
		33% {
			opacity: 1;
			-webkit-transform: scale(1);
			transform: scale(1)
		}

		66% {
			opacity: .65;
			-webkit-transform: scale(.65);
			transform: scale(.65)
		}
	}

	@keyframes c {
		33% {
			opacity: 1;
			-webkit-transform: scale(1);
			transform: scale(1)
		}

		66% {
			opacity: .65;
			-webkit-transform: scale(.65);
			transform: scale(.65)
		}
	}

	@-webkit-keyframes d {
		33% {
			opacity: .65;
			-webkit-transform: scale(.65);
			transform: scale(.65)
		}

		66% {
			opacity: 1;
			-webkit-transform: scale(1);
			transform: scale(1)
		}
	}

	@keyframes d {
		33% {
			opacity: .65;
			-webkit-transform: scale(.65);
			transform: scale(.65)
		}

		66% {
			opacity: 1;
			-webkit-transform: scale(1);
			transform: scale(1)
		}
	}

	@-webkit-keyframes e {
		33% {
			opacity: .4;
			-webkit-transform: scale(.4);
			transform: scale(.4)
		}

		66% {
			opacity: .65;
			-webkit-transform: scale(.65);
			transform: scale(.65)
		}
	}

	@keyframes e {
		33% {
			opacity: .4;
			-webkit-transform: scale(.4);
			transform: scale(.4)
		}

		66% {
			opacity: .65;
			-webkit-transform: scale(.65);
			transform: scale(.65)
		}
	}

</style>