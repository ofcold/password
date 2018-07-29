<template>
	<div class="ofcold-field">
		<div class="ofcold-field-password">
			<input
				:type="isShowPassword ? 'text' : 'password'"
				:id="safeId()"
				v-bind="$props"
				ref="input"
				:name="name"
				v-model="password"
				:class="[
					'form-control',
					sizeFormClass,
					stateClass
				]"
				placeholder="••• ••• •••"
				@keyup="checkFormat"
				@input="handleInput"
				@focus="handleFocus"
				@blur="handleBlur"
				@change="handleChange"
				:aria-invalid="computedAriaInvalid">
			<i
				:class="[
					'show-or-hide',
					'fa',
					'fa-' + (isShowPassword ? 'eye' : 'eye-slash'),
					'ofcold-field-right-icon'
				]"
				 v-if="!password || isHideTips"
				@click="onPreviewPassword"
			></i>
			<ofcold-input-dot v-if="(password && !isHideTips) || stateClass"></ofcold-input-dot>
		</div>

		<div
			:class="[
				'password-tips',
				password && !isHideTips ? 'transition' : ''
			]"
		>
			<br>
			<span class="condition-title">
				<i class="icon icon-unlock"></i>
				您的密码需要满足以下条件:
			</span>
			<dl class="ml-2">
				<dd :class="'text-' + (checked.symbol ? 'success' : 'danger')">
					<i :class="[
						'fa',
						'fa-' + (checked.symbol ? 'check-circle' : 'circle')
					]"></i>
					至少包括一个数字或一个
					<span
						 v-b-tooltip.hover
						 title="允许使用常用的标点符号以及 ^ = + $ # 或 @ 等不同符号。"
						 class="border-bottom border-dashed-bottom border-dark">
						符号
					</span>。
				</dd>

				<dd
					:class="'text-' + (checked.upper ? 'success' : 'danger')"
				>
					<i :class="[
						'fa',
						'fa-' + (checked.upper ? 'check-circle' : 'circle')
						]"
					></i>
					同时包括小写和大写拉丁
					<span
						 v-b-tooltip.hover
						 title="允许使用的拉丁字符为 A 到 Z 以及 a 到 z。"
						 class="border-bottom border-dashed-bottom border-dark">
						字符
					</span>。
				</dd>
				<dd
					:class="'text-' + (checked.minlength ? 'success' : 'danger')"
				>
					<i
						:class="[
						'fa',
						'fa-' + (checked.minlength ? 'check-circle' : 'circle')
						]"
					></i>
					至少为 8 个字符长.
				</dd>
			</dl>
		</div>
	</div>
</template>

<script>
	import idMixin from 'bootstrap-vue/src/mixins/id';
	import formMixin from 'bootstrap-vue/src/mixins/form';
	import formSizeMixin from 'bootstrap-vue/src/mixins/form-size';
	import formStateMixin from 'bootstrap-vue/src/mixins/form-state';
	import OfcoldInputDot from './OfcoldInputDot';

	export default {
		name: 'OfcoldPassword',
		mixins: [idMixin, formMixin, formSizeMixin, formStateMixin],
		data() {
			return {
				password: '',
				isHideTips: false,
				checked: {
					symbol: false,
					upper: false,
					minlength: false
				},
				isShowPassword: false
			}
		},

		/**
		 * Accept the data passed by the parent component.
		 *
		 * @type {Array}
		 */
		props: {
			isShowPasswordTipBody: {
				type: Boolean,
				default: false
			},
			value: {
				default: null
			},
			ariaInvalid: {
				type: [Boolean, String],
				default: false
			}
		},
		components: {
			OfcoldInputDot
		},
		computed: {
			computedAriaInvalid () {
				if (!this.ariaInvalid || this.ariaInvalid === 'false') {
					// this.ariaInvalid is null or false or 'false'
					return this.computedState === false ? 'true' : null
				}
				if (this.ariaInvalid === true) {
					// User wants explicit aria-invalid=true
					return 'true'
				}
				// Most likely a string value (which could be 'true')
				return this.ariaInvalid
			}
		},
		methods: {
			validateState(ref) {
				if (this.fields[ref] && this.fields[ref].dirty) {
					return !this.errors.has(ref)
				}

				return null
			},
			handleFocus(event) {
				this.$emit('focus', event);
			},
			handleInput(event) {
				const fValue = event.target.value;
				this.setValue(fValue);
			},
			handleChange(event)
			{
				const fValue = event.target.value;
				this.$emit('change', fValue);
				this.setValue(fValue);
			},
			handleBlur(event) {
				this.$emit('blur', event);
			},
			setValue (value) {
				this.$emit('input', value)
				// When formatter removes last typed character, value of text input should update to formatted value
				this.$refs.input.value = value
			},

			/**
			 * Verify the strength of the password, strictly specify the password. This will allow users to be more secure
			 * on the site.
			 *
			 * @return {void}
			 */
			checkFormat() {

				//	Display tips
				this.isHideTips = false;

				//let strength = 0;
				this.checked.symbol = /[0-9\x21-\x2F\x3A-\x3C\x3E-\x40\x5B\x5D-\x60\x7B\x7D-\x7E]/.test(this.password);
				this.checked.upper = /[a-z]/.test(this.password) && /[A-Z]/.test(this.password);
				this.checked.minlength = typeof this.password === 'string' && this.password.replace(/(^\s+)|(\s+$)/g, '').length >= 8;

				this.isHideTips = this.isSecretRequirements();
			},

			/**
			 * The password entered by the user is strictly tested.
			 *
			 * @return {Boolean}
			 */
			isSecretRequirements() {
				//	Is the password entered by the user? We let the user to the next step.
				return this.checked.symbol
					 && this.checked.upper
					 && this.checked.minlength
			},

			/**
			 * [onPreviewPassword description]
			 *
			 * @return {void}
			 */
			onPreviewPassword() {
				this.isShowPassword = this.isShowPassword ? false : true;
			}
		}
	}
</script>

<style lang="scss" scoped>
	.ofcold-field {
		&-password {

			position: relative;
			z-index: 0;

			.form-control[type="password"] {
				text-overflow: ellipsis;
				text-shadow: none;
				-webkit-transition: all .15s ease-in-out;
				transition: all .15s ease-in-out;
				letter-spacing: .34rem;
			}
			.show-or-hide {
				cursor: pointer;
				position: absolute;
				top: .7rem;
				right: .7rem;
			}
		}

		.password-tips {
			font-size: .8rem;
			max-height: 0;
			opacity: 0;
			overflow: hidden;
			-webkit-transition: max-height .2.7s ease, opacity .2.7s ease;
			transition: max-height .2.7s ease, opacity .2.7s ease;
			-webkit-transition-delay: 0ms;
			transition-delay: 0ms;

			&.transition {
				opacity: 1;
				max-height: 270px;
				overflow: visible;
				-webkit-transition-delay: .2, .2, .34s;
				transition-delay: .2, .2, .34s;
				-webkit-transition-duration: .4s;
				transition-duration: .4s
			}

			.condition-title {
				margin-bottom: .34rem;
				display: block;
				color: #727477;
			}

			dl {
				dd {
					line-height: 1rem;
					font-weight: 500;
				}
			}
		}
	}
</style>