<template>
<section class="authentication">
    <div class="auth-body">
        <h1 class="text-uppercase fw-500 mb-4 text-center font-22">
            登録
        </h1>
        <form class="auth-form" @submit.prevent="submit">
            <alert-success :form="form">
                有効化するためのメールを送信しました
            </alert-success>
            <div class="form-group">
                <input
                    type="text"
                    v-model.trim="form.name"
                    name="name"
                    class="form-control form-control-lg font-14 fw-300"
                    :class="{ 'is-invalid': form.errors.has('name') }"
                    placeholder="名前"
                />
                <has-error :form="form" field="name"></has-error>
            </div>
            <div class="form-group">
                <input
                    type="text"
                    v-model.trim="form.username"
                    name="username"
                    class="form-control form-control-lg font-14 fw-300"
                    :class="{ 'is-invalid': form.errors.has('username') }"
                    placeholder="ユーザー名"
                />
                <has-error :form="form" field="username"></has-error>
            </div>
            <div class="form-group">
                <input
                    type="text"
                    name="email"
                    v-model.trim="form.email"
                    class="form-control form-control-lg font-14 fw-300"
                    :class="{ 'is-invalid': form.errors.has('email') }"
                    placeholder="メールアドレス"
                />
                <has-error :form="form" field="email"></has-error>
            </div>
            <div class="form-group">
                <input
                    type="password"
                    v-model.trim="form.password"
                    name="password"
                    class="form-control form-control-lg font-14 fw-300"
                    :class="{ 'is-invalid': form.errors.has('password') }"
                    placeholder="パスワード"
                />
                <has-error :form="form" field="password"></has-error>
            </div>
            <div class="form-group">
                <input
                    type="password"
                    v-model.trim="form.password_confirmation"
                    name="password_confirmation"
                    class="form-control form-control-lg font-14 fw-300"
                    :class="{ 'is-invalid': form.errors.has('password_confirmation') }"
                    placeholder="パスワード(確認用)"
                />
                <has-error :form="form" field="password_confirmation"></has-error>
            </div>

            <div class="text-right">
                <button type="submit"
                        :disabled="form.busy"
                        class="btn btn-primary primary-bg-color font-16 fw-500 text-uppercase">
                    <span v-if="form.busy">
                        <i class="fas fa-spinner fa-spin"></i>
                    </span>
                    登録
                </button>
            </div>
            <p class="font-14 fw-400 text-center mt-4">
                既にアカウントをお持ちの方
                <nuxt-link :to="{name: 'login'}" class="color-blue" href="#"> ログイン</nuxt-link>
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
                name: '',
                username: '',
                email: '',
                password: '',
                password_confirmation: ''
            })
        }
    },

    methods: {
        submit() {
            this.form
            .post(`/register`)
            .then(res => {
                this.form.reset();
                // console.log(res);
            })
            .catch(error => {
                console.log(error);
            });
        }
    }
}
</script>

<style>

</style>
