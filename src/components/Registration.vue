<template>
    <modal title="Registration"
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

                    <div class="form-item" :class="{ errorInput: $v.repeatPassword.$error }">
                        <label>Repeat password</label>
                        <p class="errorText" v-if="!$v.repeatPassword.sameAsPassword">Passwords must be identical.</p>
                        <input v-model.trim="$v.repeatPassword.$model"
                               :class="{ error: $v.password.$error }"
                               @change="$v.password.$touch()"/>
                    </div>

                    <!--         end password           -->

                <button class="btn btnPrimary">Registration!</button>

                </div>

            </form>

            <!--                link to login-->
            <button class="form-item-link" @click="login = !login">
                <p class="form-text">I already have an account</p>
            </button>
            <login v-show="login" @close="login = false" />

        </div>
    </modal>

</template>

<script>
    import { required, minLength, email, sameAs } from 'vuelidate/lib/validators'
    import modal from '@/components/UI/Modal.vue'
    import login from '@/components/Login.vue'
    export default {
        components: {
            modal,
            login
        },
        data() {
            return {
                email: '',
                password: '',
                repeatPassword: '',
                login: false,
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
            repeatPassword: {
                sameAsPassword: sameAs('password')
            }
        },
        methods: {
            onSubmit () {
                this.$v.$touch()
                if (!this.$v.$invalid) {
                    const user = {
                        email: this.email,
                        password: this.password,
                        repeatPassword: this.repeatPassword,
                    }
                    console.log(user)

                    // DONE
                    this.email = ''
                    this.password = ''
                    this.repeatPassword = ''
                    this.$v.$reset()
                    this.$emit('close')
                }
            },
            //reset
            onClose() {
                // DONE
                this.email = ''
                this.password = ''
                this.repeatPassword = ''
                this.$emit('close');
                this.$v.$reset();
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