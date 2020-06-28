<template>
    <section class="authentication">
        <div class="auth-body">
            <h1 class="text-uppercase fw-500 mb-4 text-center font-22">
                ログイン
            </h1>
            <form class="auth-form" @submit.prevent="submit">
                <alert-error v-if="form.errors.has('message')" :form="form">
                    {{ form.errors.get('message') }}
                    <nuxt-link :to="{ name: 'verification.resend' }"
                        >確認メールを再送する</nuxt-link
                    >
                </alert-error>
                <div class="form-group">
                    <base-input
                        :form="form"
                        field="email"
                        v-model="form.email"
                        placeholder="メールアドレス"
                        ></base-input>
                </div>
                <div class="form-group">
                    <base-input
                        :form="form"
                        field="password"
                        inputType="password"
                        v-model="form.password"
                        placeholder="パスワード"
                        ></base-input>
                </div>
                <div class="mt-4 mb-4 clearfix">
                    <nuxt-link to="/password/email" class="forgot-pass color-blue font-14 fw-400">パスワードを忘れた場合はこちら</nuxt-link>
                </div>
                <div class="text-center">
                    <base-button :loading="form.busy">
                        ログイン
                    </base-button>
                </div>
                <p class="font-14 fw-400 text-center mt-4">
                    <nuxt-link :to="{name: 'register'}" class="color-blue"> 会員登録はこちら</nuxt-link>
                </p>
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
            email: '',
            password: ''
            })
        }
    },
    methods: {
        submit() {
            this.$auth
                .loginWith('local', {
                    data: this.form
                })
                .then(res => {
                    console.log(res);
                })
                .catch(e => {
                    this.form.errors.set(e.response.data.errors);
                });
        }
    }
}


</script>

<style>

</style>
