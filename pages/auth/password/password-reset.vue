<template>
    <section class="authentication">
        <div class="auth-body">
            <h1 class="text-uppercase fw-500 mb-4 text-center font-22">
                パスワードリセット
            </h1>
            <form class="auth-form" @submit.prevent="submit">
                <alert-success :form="form">
                    {{ status }}
                    <p>
                        <nuxt-link to="/login">ログインする</nuxt-link>
                    </p>
                </alert-success>
                <div class="form-group">
                    <input
                        type="text"
                        name="email"
                        readonly
                        v-model="form.email"
                        class="form-control form-control-lg font-14 fw-300"
                        :class="{'is-invalid' : form.errors.has('email')}"
                        placeholder="メールアドレス"
                    />
                    <has-error :form="form" field="email"></has-error>
                </div>

                <div class="form-group">
                    <input
                        type="password"
                        name="password"
                        v-model="form.password"
                        class="form-control form-control-lg font-14 fw-300"
                        :class="{'is-invalid' : form.errors.has('password')}"
                        placeholder="新しいパスワード"
                    />
                    <has-error :form="form" field="password "></has-error>
                </div>

                <div class="form-group">
                    <input
                        type="password"
                        name="password_confirmation"
                        v-model="form.password_confirmation"
                        class="form-control form-control-lg font-14 fw-300"
                        :class="{'is-invalid' : form.errors.has('password_confirmation')}"
                        placeholder="新しいパスワード(確認用)"
                    />
                </div>

                <div class="text-right">
                    <button type="submit"
                            :disabled="form.busy"
                            class="btn btn-primary primary-bg-color font-16 fw-500 text-uppercase">
                        <span v-if="form.busy">
                            <i class="fas fa-spinner fa-spin"></i>
                        </span>
                        パスワードをリセットする
                    </button>
                </div>
                <p class="font-14 fw-400 text-center mt-4">

                    <nuxt-link :to="{name: 'login'}" class="color-blue">
                        ログインページへ戻る
                    </nuxt-link>
                </p>
            </form>
        </div>
    </section>
</template>

<script>
export default {
    middleware: ['guest'],
    data() {
    return {
        status: '',
        form: this.$vform({
        email: '',
        password: '',
        password_confirmation: '',
        token: ''
        })
        };
    },
    methods: {
        submit() {
            this.form
                .post('/password/reset')
                .then(res => {
                this.status = res.data.status;
                this.form.reset();
                })
                .catch(e => {
                console.log(e);
                });
        }
    },
    created() {
        this.form.email = this.$route.query.email;
        this.form.token = this.$route.params.token;
    }
};
</script>

<style>

</style>
