<template>
    <head>
        <title>Login Account - Aplikasi Kasir</title>
    </head>
    <div class="col-md-4">
        <div class="fade-in">
            <div class="text-center mb-4">
                <a href="" class="text-dark text-decoration-none">
                    <img src="/images/cash-machine.png" alt="" width="70">
                    <h3 class="mt-2 font-weight-bold">Aplikasi POS</h3>
                </a>
            </div>
            <div class="card-group">
                <div class="card border-top-purple border-0 shadow-sm rounded-3">
                    <div class="card-body">
                        <div class="text-start">
                            <h5>Login Account</h5>
                            <p class="text-muted">Sign In to Your Account</p>
                        </div>
                        <hr>
                        <div class="alert alert-success mt-2" v-if="session.status">
                            {{ session.status }}
                        </div>
                        <form @submit.prevent="submit">
                            <div class="input-group mb-3">
                                <div class="input-group-prepend">
                                    <span class="input-group-text">
                                        <i class="fa fa-envelope"></i>
                                    </span>
                                </div>
                                <input class="form-control" v-model="form.email" :class="{ 'is-invalid': errors.email }" type="email" placeholder="Email Address">
                            </div>
                            <div class="alert alert-danger" v-if="errors.email">
                                {{ errors.email }}
                            </div>
                            <div class="input-group mb-3">
                                <div class="input-group-prepend">
                                    <span class="input-group-text">
                                        <i class="fa fa-lock"></i>
                                    </span>
                                </div>
                                <input class="form-control" v-model="form.password" :class="{ 'is-invalid': errors.password }" type="password" placeholder="Password">
                            </div>
                            <div v-if="errors.password" class="alert alert-danger">
                                {{ errors.password }}
                            </div>
                            <div class="row">
                                <div class="col-12 mb-3 text-end">
                                    <Link href="/forgot-password">Forgot Password </Link>
                                </div>
                                <div class="col-12">
                                    <button class="btn btn-primary shadow-sm rounded-sm px-4 w-100" type="submit">Login</button>
                                </div>
                            </div>
                        </form>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
// import layout
import LayoutAuth from '../../Layouts/Auth.vue';

// import reactive
import { reactive } from 'vue';

// import inertia adapter
import { Inertia, inertia } from '@inertiajs/inertia';

// import header and useForm from inertia
import { Head, Link,} from '@inertiajs/inertia-vue3';

export default {
    
    // layout
    layout : LayoutAuth,

    // register component
    components: {
        Head,
        Link
    },

    props: {
        errors: Object,
        session: Object
    },

    // define composition PAI
    setup() {

        // define form state
        const form = reactive({
            email: '',
            password: '',
        });

        // submit method
        const submit = () => {
            // send data to server

            Inertia.post('/login', {
                // data
                email: form.email,
                password: form.password,
            });
        }

        // return form state and submit method
        return  {
            form,
            submit
        };
    }
}
</script>

<style>
</style>