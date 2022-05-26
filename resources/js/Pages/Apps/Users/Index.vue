<template>
    <Head>
        <title>Users - Aplikasi Kasir</title>
    </Head>
    <main class="c-main">
        <div class="container-fluid">
            <div class="fade-in">
                <div class="row">
                    <div class="col-md-12">
                        <div class="card border-0 rounded-3 shadow border-top-purple">
                            <div class="card-header">
                                <spa class="font-weight-bold"><i class="fa fa-users"></i>Users</spa>
                            </div>

                            <div class="card-body">
                                <form @submit.prevent="handleSearch">
                                    <div class="input-group mb-3">
                                        <Link href="/apps/users/create" v-if="hasAnyPermission(['users.create'])" class="btn btn-primary input-group-text"><i class="fa fa-plus-circle me-2"></i>New</Link>
                                        <input type="text" placeholder="search by user name" class="form-control" v-model="search">
                                        <button class="btn btn-primary input-group-text" type="submit"><i class="fa fa-search me-2"></i>Search</button>
                                    </div>
                                </form>
                                <table class="table table-striped table-bordered table-hover">
                                    <thead>
                                        <tr>
                                            <th scope="col">Full Name</th>
                                            <th scope="col">Email Address</th>
                                            <th scope="col">Roles</th>
                                            <th scope="col" style="width:20%">Actions</th>
                                        </tr>
                                    </thead>
                                    <tbody>
                                        <tr v-for="(user, index) in users.data" :key="index">
                                            <td>{{ user.name }}</td>
                                            <td>{{ user.email }}</td>
                                            <td>
                                                <span class="badge badge-primary shadow border-0 mb-2" v-for="(role, index) in user.roles" :key="index">
                                                    {{ role.name }}
                                                </span>
                                            </td>
                                            <td class="text-center">
                                                <Link class="btn btn-success btn-sm me-2" :href="`/apps/users/${user.id}/edit`" v-if="hasAnyPermission(['users.edit'])"><i class="fa fa-pencil-alt me-1"></i>Edit</Link>
                                                <button class="btn btn-danger btn-sm" v-if="hasAnyPermission(['users.delete'])"><i class="fa fa-trash"></i>Delete</button>
                                            </td>
                                        </tr>
                                    </tbody>
                                </table>
                                <Pagination :links="users.links" align="end"/>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </main>
</template>

<script>
    import LayoutApp from '../../../Layouts/App.vue';
    import { Head, Link } from '@inertiajs/inertia-vue3';
    import Pagination from '../../../Components/Pagination.vue';
    import { ref } from 'vue';
    import { Inertia } from '@inertiajs/inertia';

    export default {
        layout: LayoutApp,
        
        components: {
            Head,
            Link,
            Pagination
        },

        // props
        props: {
            users: Object,
        },

        setup() {
            const search = ref(' ' || (new URL(document.location)).searchParams.get('q'));

            const handleSearch = () => {
                Inertia.get('/apps/users', {
                    q: search.value,
                });
            }

            return { 
                search,
                handleSearch,
            }
        }
    }
</script>