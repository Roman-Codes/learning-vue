<template>
    <div id="employee-form">
        <form @submit.prevent="handleSubmit">
            <label for="">Employee Name</label>
            <input
                ref="first"
                type="text"
                :class="{ 'has-error': submitting && invalidName }"
                v-model="employee.name"
                @focus="clearStatus"
                @keypress="clearStatus"
            />
            <label for="">Email</label>
            <input
                v-model="employee.email"
                type="text"
                :class="{ 'has-error': submitting && invalidEmail }"
                @focus="clearStatus"
            />
            <p v-if="error && submitting" class="error-message">
                ❗Please fill out all required fields
            </p>
            <p v-if="success" class="success-message">
                ✅ Employee successfully added
            </p>
            <button>Add Employee</button>
        </form>
    </div>
</template>

<script>
    export default {
        name: 'employee-form',

        methods: {
            handleSubmit() {
                this.submitting = true;
                this.clearStatus();

                if (this.invalidName || this.invalidEmail){
                    this.error = true;
                    return
                }

                this.$emit('add:employee', this.employee);
                this.$refs.first.focus();
                this.employee = {
                    name: '',
                    email: '',
                }
            },
            clearStatus() {
                this.success = false;
                this.error = false;
            }
        },

        data() {
            return {
                submitting: false,
                error: false,
                success: false,
                employee: {
                    name: '',
                    email: '',
                },
            }
        },

        computed: {
            invalidName() {
                return this.employee.name === ''
            },

            invalidEmail() {
                return this.employee.email === ''
            },
        },
    }
</script>

<style scoped>
    form {
        margin-bottom: 2rem;
    }

    [class*='-message'] {
        font-weight: 500;
    }

    .error-message {
        color: #d33c40;
    }

    .success-message {
        color: #32a95d;
    }
</style>