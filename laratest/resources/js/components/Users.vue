<template>
    <div class="container mt-5">
        <div class="row">
            <div class="col-12">
                <div class="card">
                    <div class="card-header">
                        <h3 class="card-title">Fixed Header Table</h3>

                        <div class="card-tools">
                            <button type="button" class="btn btn-primary" data-toggle="modal" data-target="#NewUser"> new user </button>
                        </div>

                        <!-- Modal -->
                        <div class="modal fade" id="NewUser" tabindex="-1" role="dialog" aria-labelledby="NewUserLabel" aria-hidden="true">
                            <div class="modal-dialog" role="document">
                                <div class="modal-content">
                                    <div class="modal-header">
                                        <h5 class="modal-title" id="NewUserLabel">Modal title</h5>
                                        <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                                        <span aria-hidden="true">&times;</span>
                                        </button>
                                    </div>

                                    <form @submit.prevent="login" @keydown="form.onKeydown($event)">
                                        <!-- Alert -->
                                        <alert-error :form="form"></alert-error>
                                        <div class="modal-body">

                                            <!-- Username -->
                                            <div class="form-group">
                                                <label for="username">Username</label>
                                                <input v-model="form.name" type="text" name="name" id="name"
                                                class="form-control" :class="{ 'is-invalid': form.errors.has('name') }">
                                                <has-error :form="form" field="name"></has-error>
                                            </div>

                                            <!-- Email -->
                                            <div class="form-group">
                                                <label for="email">Email</label>
                                                <input v-model="form.email" type="email" name="email" id="email"
                                                class="form-control" :class="{ 'is-invalid': form.errors.has('email') }">
                                                <has-error :form="form" field="email"></has-error>
                                            </div>

                                            <!-- Bio -->
                                            <div class="form-group">
                                                <label for="bio">Biography</label>
                                                <textarea v-model="form.bio" name="bio" id="bio"
                                                class="form-control" :class="{ 'is-invalid': form.errors.has('bio') }"></textarea>
                                                <has-error :form="form" field="bio"></has-error>
                                            </div>

                                            <!-- Type -->
                                            <div class="form-group">
                                                <label for="type">User Type </label>
                                                <select v-model="form.type" name="type" id="type"
                                                class="form-control" :class="{ 'is-invalid': form.errors.has('type') }">
                                                    <option value="">select user role</option>
                                                    <option value="admin">Admin</option>
                                                    <option value="user">Standard User</option>
                                                    <option value="author">Author</option>
                                                </select>
                                                <has-error :form="form" field="type"></has-error>
                                            </div>

                                            <!-- Password -->
                                            <div class="form-group">
                                                <label for="password">Password</label>
                                                <input v-model="form.password" type="password" name="password" id="password"
                                                class="form-control" :class="{ 'is-invalid': form.errors.has('password') }">
                                                <has-error :form="form" field="password"></has-error>
                                            </div>
                                        </div>
                                        <div class="modal-footer">
                                            <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
                                            <button :disabled="form.busy" type="submit" class="btn btn-primary">Save changes</button>
                                        </div>
                                    </form>
                                </div>
                            </div>
                        </div>

                    </div>

                    <!-- /.card-header -->
                    <div class="card-body table-responsive p-0">
                        <table class="table table-head-fixed table table-striped">
                            <thead>
                                <tr>
                                    <th>ID</th>
                                    <th>UserName</th>
                                    <th>Email</th>
                                    <th>Status</th>
                                    <th>Action</th>
                                </tr>
                            </thead>
                            <tbody>
                                <tr v-for="user in users.data.data" :key="user.id">
                                    <td>{{user.id}}</td>
                                    <td>{{user.name}}</td>
                                    <td>{{user.email}}</td>
                                    <td> {{user.type}} </td>
                                    <td> {{user.created_at | filterDate}} </td>
                                    <td>
                                        <a href="#" class="btn btn-primary"> Edit </a>
                                        <a href="#" class="btn btn-danger"> Remove </a>
                                    </td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                    <!-- /.card-body -->
                </div>
                <!-- /.card -->
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                // Create the form instance
                form: new Form({
                    name: '',
                    email: '',
                    password: '',
                    type: '',
                    bio: '',
                    photo: '',
                    password: ''
                }),
                users: {}
            }
        },
        methods: {
            showUsers() {
                axios.get('api/user').then((response) => {this.users = response});

            },
            login () {
                // Submit the form via a POST request.
                this.form.post('api/user')
            }
        },
        created() {
            this.showUsers();
        }
    }

</script>
