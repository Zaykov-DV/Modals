<template>
  <div class="wrapper">

<!--    header-->

    <div class="wrapper-content">
      <section>
        <div class="container">

            <!-- first modal-->
            <button class="btn btnPrimary" @click="modalFirst = !modalFirst">Show first modal</button>
            <modals title="First modal"
                    v-show="modalFirst"
                    @close="modalFirst = false">

                <div slot="body">
                <p> Text </p>
                <button class="btn btnPrimary" @click="modalFirst = !modalFirst">Well done!</button>
                </div>
            </modals>


            <!--  modal with form-->
            <button class="btn btnPrimary" @click="modalSecond.show = !modalSecond.show">Show modal with form</button>
            <modals title="Modal with form"
                    v-show="modalSecond.show"
                    @close="onClose">

                <div slot="body">
                    <form @submit.prevent="submitSecondForm">
<!--       @submit.prevent  чтобы не обновлялась страница -->
                        <label>Name:</label>
                        <input type="text" required v-model="modalSecond.name">
                        <label>Email:</label>
                        <input type="email" required v-model="modalSecond.email">
                        <button class="btn btnPrimary">Submit!</button>
                    </form>
                </div>
            </modals>


<!--            modal with validate-->
            <button class="btn btnPrimary" @click="modalValidate = !modalValidate">Show modal with validate</button>
            <modalValidate v-show="modalValidate" @close="modalValidate = false" />

 <!--      modal with validate and password-->
            <button class="btn btnPrimary" @click="modalValidatePassword = !modalValidatePassword">Show modal with validate</button>
            <modalValidatePassword v-show="modalValidatePassword" @close="modalValidatePassword = false" />

<!--Homework-->
            <button class="btn" @click="login = !login">Login</button>
            <login v-show="login" @close="login = false" />

            <button class="btn" @click="registration = !registration">Registration</button>
            <registration v-show="registration" @close="registration = false" />



        </div>
      </section>
    </div>

<!--    footer-->

  </div>
</template>

<script>
    import modals from '@/components/UI/Modal.vue'
    import modalValidate from '@/components/ModalValidate.vue'
    import modalValidatePassword from '@/components/ModalValidatePassword.vue'
    import login from '@/components/Login.vue'
    import registration from '@/components/Registration.vue'

export default {
    components: { modals, modalValidate, modalValidatePassword, login, registration },
    data () {
        return {
            modalFirst: false,
            modalSecond: {
                show: false,
                name: '',
                email: ''
            },
            modalValidate: false,
            modalValidatePassword: false,
            login: false,
            registration: false
        }
    },
    methods: {
        submitSecondForm () {
            console.log({
                name: this.modalSecond.name,
                email: this.modalSecond.email
            })
            this.modalSecond.name = ''
            this.modalSecond.email = ''
            this.modalSecond.show = false
        },
        //reset для modalSecond
        onClose() {
            this.modalSecond.name = ''
            this.modalSecond.email = ''
            this.modalSecond.show = false
            this.$emit('close');
        }
    }
}
</script>
