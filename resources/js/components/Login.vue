<template>
    <div class="d-flex align-items-center justify-content-center my-5">
        <Login class="flex-fill" :action="login">
            <template #header>
                <img
                    src="img/logo.png"
                    width="175"
                    class="d-block mx-auto mb-5 user-select-none pe-none"
                    :draggable="false"
                />
            </template>
        </Login>

    </div>
</template>


<script setup>
    import useUserStore from '@/bootstrap/stores/user.js';

    import { useRoute } from 'vue-router';
    import router from '@/bootstrap/router.js';
    import Login from "@dh-center-tuebingen/dhc-components/Blocks/Login";
    const route = useRoute();
    const userStore = useUserStore();


    const login = async ({ username, password }) => {

        const credentials = {
            password: password
        };
        // dirty check if email field should be treated
        // as actual email address or nickname
        if(username.includes('@')) {
            credentials.email = username;
        } else {
            credentials.nickname = username;
        }

        await userStore.login(credentials)

        if(route.query.redirectTo) {
            router.push(route.query.redirectTo);
        } else {
            router.push({
                name: 'home',
            });
        }
    }

    // import {
    //     reactive,
    //     onMounted,
    // } from 'vue';

    // import { useI18n } from 'vue-i18n';

    // import {
    //     getErrorMessages,
    //     getValidClass,
    // } from '@/helpers/helpers.js';

    // export default {
    //     setup() {
    //         const { t, locale } = useI18n();
    //         const route = useRoute();
    //         const userStore = useUserStore();
    //         // DATA
    //         const state = reactive({
    //             user: {},
    //             redirect: {
    //                 name: 'home'
    //             },
    //             submitting: false,
    //             error: {},
    //         });

    //         // FUNCTIONS
    //         const login = async _ => {
    //             state.submitting = true;
    //             state.error = {};
    //             const credentials = {
    //                 password: state.user.password
    //             };
    //             // dirty check if email field should be treated
    //             // as actual email address or nickname
    //             if(state.user.email.includes('@')) {
    //                 credentials.email = state.user.email;
    //             } else {
    //                 credentials.nickname = state.user.email;
    //             }
    //             await userStore.login(credentials)
    //                 .then(_ => {
    //                     state.error = {};
    //                     if(route.query.redirectTo) {
    //                         router.push(route.query.redirectTo);
    //                     } else {
    //                         router.push({
    //                             name: 'home',
    //                         });
    //                     }
    //                 })
    //                 .catch(e => {
    //                     userStore.logout();
    //                     state.error = getErrorMessages(e);
    //                     return Promise.reject();
    //                 }).finally(_ => {
    //                     state.submitting = false;
    //                 });
    //         };

    //         // ON MOUNTED
    //         onMounted(_ => {
    //             if(userStore.userLoggedIn) {
    //                 router.push({
    //                     name: 'home'
    //                 });
    //             }
    //         });

    //         // RETURN
    //         return {
    //             t,
    //             state,
    //             login,
    //             getValidClass,
    //         };
    //     },
    // }
</script>
