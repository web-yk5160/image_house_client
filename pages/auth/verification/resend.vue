<template>
    <section class="authentication">
        <div class="auth-body">
            <h1 class="text-uppercase fw-500 mb-4 text-center font-22">
                確認メールの再送信
            </h1>
            <form class="auth-form" @submit.prevent="submit">
                <alert-error v-if="form.errors.has('message')" :form="form">
                    {{ form.errors.get('message') }}
                    <nuxt-link :to="{ name: 'verification.resend' }"
                        >確認メールを再送する</nuxt-link
                    >
                </alert-error>
                <alert-success :form="form">
                    登録したメールアドレスに確認メールを送りました
                </alert-success>
                <div class="form-group">
                    <input
                        type="text"
                        name="email"
                        v-model="form.email"
                        class="form-control form-control-lg font-14 fw-300"
                        :class="{'is-invalid' : form.errors.has('email')}"
                        placeholder="メールアドレス"
                    />
                    <has-error :form="form" field="email"></has-error>
                </div>
                <div class="text-right">
                    <button type="submit"
                            :disabled="form.busy"
                            class="btn btn-primary primary-bg-color font-16 fw-500 text-uppercase">
                        <span v-if="form.busy">
                            <i class="fas fa-spinner fa-spin"></i>
                        </span>
                        再送
                    </button>
                </div>
            </form>
        </div>
    </section>
</template>

<script>
export default {
    middleware: ['guest'],
    data(){
        return {
            form: this.$vform({
            email: ''
            })
        };
        },

        methods: {
        submit() {
            this.form.post(`/verification/resend`)
            .then(res => this.form.resent())
            .catch(e => console.log(e));
        }
    }
}
</script>

<style>

</style>
