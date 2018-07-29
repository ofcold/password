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
				{{ this.$t('users.password.meet_conditions') }}:
			</span>
			<dl class="ml-2">
				<dd :class="'text-' + (checked.symbol ? 'success' : 'danger')">
					<i :class="[
						'fa',
						'fa-' + (checked.symbol ? 'check-circle' : 'circle')
					]"></i>
					{{ this.$t('users.password.strict_symbol') }}
					<span
						 v-ofcold-tooltip.hover
						 :title="this.$t('users.password.allowed_symbol')"
						 class="border-bottom border-dashed-bottom border-dark">
						{{ this.$t('users.password.symbol') }}
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
					{{ this.$t('users.password.strict_upper') }}
					<span
						 v-ofcold-tooltip.hover
						 :title="this.$t('users.password.allowed_character')"
						 class="border-bottom border-dashed-bottom border-dark">
						{{ this.$t('users.password.character') }}
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
					{{ this.$t('users.password.max_length') }}
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

	export default {
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