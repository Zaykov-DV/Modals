<template>
    <modal title="Login"
            @close="onClose()">

        <div slot="body">
            <form @submit.prevent="onSubmit">

                <!--        Email      -->
                <div class="form-item" :class="{ errorInput: $v.email.$error }">
                    <label>Email:</label>
                    <p class="errorText" v-if="!$v.email.required"> Field is required! </p>
                    <p class="errorText" v-if="!$v.email.email"> Email is not correct! </p>

                    <input v-model="email"
                           :class="{ error: $v.email.$error }"
                           @change="$v.email.$touch()">
<!-- v-model может изменять data, работа с выходными и входными данными -->
<!-- v-bind работа только с выходными параметрами, не может изменять -->

                    <!----         password  ---->

                    <div class="form-item" :class="{ errorInput: $v.password.$error }">
                        <label>Password</label>
                        <p class="errorText" v-if="!$v.password.required">Password is required.</p>
                        <p class="errorText" v-if="!$v.password.minLength">Password must have at least {{ $v.password.$params.minLength.min }} letters.</p>
                        <input v-model.trim="$v.password.$model"
                               :class="{ error: $v.password.$error }"
                               @change="$v.password.$touch()"/>
                    </div>
                    <!--         end password           -->

                <button class="btn btnPrimary">Enter</button>

                </div>
            </form>
            <!--link to registration-->
            <button class="form-item-link" @click="registration = !registration">
                <p class="form-text">Need registration?</p>
            </button>
            <registration v-show="registration" @close="registration = false" />
        </div>
    </modal>

</template>

<script>
    import { required, minLength, email } from 'vuelidate/lib/validators'
    import modal from '@/components/UI/Modal.vue'
    import registration from '@/components/Registration.vue'
    export default {
        components: { modal, registration },
        data() {
            return {
                email: '',
                password: '',
                registration: false,
            }
        },
        validations: {
            email: {
                required,
                email
            },
            password: {
                required,
                minLength: minLength(6)
            },
        },
        methods: {
            onSubmit () {
                this.$v.$touch()
                if (!this.$v.$invalid) {
                    const user = {
                        email: this.email,
                        password: this.password,
                    }
                    console.log(user)

                    // DONE
                    this.email = ''
                    this.password = ''
                    this.$v.$reset()
                    this.$emit('close')
                }
            },
            //reset
            onClose() {
                // DONE
                this.email = ''
                this.password = ''
                this.$v.$reset()
                this.$emit('close')

            }
        }
    }
</script>

<style lang="scss">
    @import "../assets/scss/utils/vars.scss";

    .form-item .errorText {
        display: none;
        margin-bottom: 8px;
        font-size: 13px;
        color: #de4040;
    }
    .form-item {
        display: flex;
        flex-direction: column;
        &.errorInput .errorText {
            display: block;
        }
    }

    input.error {
        border-color: #de4040 ;
    }

    .form-item-link {
        display: flex;
        justify-content: center;
        align-items: center;
        border: none;
        margin-top: 20px;
    }

    .form-text {
        color: $neutral-regular;
        text-decoration: none;
        font-size: 12px;
        border-bottom: 1px solid $neutral-regular;
    }


</style>