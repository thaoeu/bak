<style lang="less">
    @import '../login/login.less';
</style>

<template>
    <div class="login">
        <div class="login-con">
            <Card icon="log-in" title="欢迎注册" :bordered="false">
                <div class="form-con">
                    <register-form @on-success-valid="handleSubmit"></register-form>
                    <p class="login-tip"><a href="https://github.com/thaoeu/pavment_management_system">仓库地址</a></p>
                </div>
            </Card>
        </div>
    </div>
</template>

<script>
    import RegisterForm from '../../components/register-form'
    export default {
        components: {
            RegisterForm
        },
        methods: {
            handleSubmit ({ userName, password }) {
                this.$router.push('/backend/home')
            }
        }
    }
</script>

<style>

</style>
