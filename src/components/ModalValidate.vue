<template>
    <modal title="Modal with form"
            @close="onClose()">

        <div slot="body">
            <form @submit.prevent="onSubmit">
           <!--        Name      -->
                <div class="form-item" :class="{ errorInput: $v.name.$error }">
                    <label>Name:</label>
                    <p class="errorText" v-if="!$v.name.required"> Field is required! </p>
                    <p class="errorText" v-if="!$v.name.minLength"> Name must have at least {{ $v.name.$params.minLength.min }} symbols! </p>
                   <!--   ошибки которые приходят от Vuelidate    -->
                    <input
                            v-model="name"
                            :class="{ error: $v.name.$error }"
                            @change="$v.name.$touch()">
<!--запуск проверки на ошибки методом change (наблюдает изменилось ли значение инпут), метод touch запускает инициализацию -->
                </div>

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


                <button class="btn btnPrimary">Submit!</button>
                </div>

            </form>

        </div>
    </modal>

</template>

<script>
    import { required, minLength, email } from 'vuelidate/lib/validators'
    import modal from '@/components/UI/Modal.vue'
    export default {
        components: { modal },
        data() {
            return {
                name: '',
                email: '',
            }
        },
        validations: {
            name: {
                required,
                minLength: minLength(4)
            },
            email: {
                required,
                email
            }
        },
        methods: {
            onSubmit () {
                this.$v.$touch()
                if (!this.$v.$invalid) {
                    const user = {
                        name: this.name,
                        email: this.email
                    }
                    console.log(user)

                    // DONE
                    this.name = ''
                    this.email = ''
                    this.$v.$reset()
                    this.$emit('close')
                }
            },
            //reset
            onClose() {
                // DONE
                this.name = ''
                this.email = ''
                this.$emit('close');
                this.$v.$reset();
            }
        }
    }
</script>

<style lang="scss">
    .form-item .errorText {
        display: none;
        margin-bottom: 8px;
        font-size: 13px;
        color: #de4040;
    }
    .form-item {
        &.errorInput .errorText {
            display: block;
        }
    }

    input.error {
        border-color: #de4040 ;
    }



</style>