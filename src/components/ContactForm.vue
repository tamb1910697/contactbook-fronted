<template>
    <Form @submit="submitContact" :validation-schema="contactFormSchema">
        <div class="form-group">
            <label for="name">Name</label>
            <Field name="name" type="text" class="form-control" v-model="contactLocal.name" />
            <ErrorMessage name="name" class="error-feedback" />
        </div>
        <div class="form-group">
            <label for="email">E-mail</label>
            <Field name="email" type="email" class="form-control" v-model="contactLocal.email" />
            <ErrorMessage name="email" class="error-feedback" />
        </div>
        <div class="form-group">
            <label for="address">Address</label>
            <Field name="address" type="text" class="form-control" v-model="contactLocal.address" />
            <ErrorMessage name="address" class="error-feedback" />
        </div>
        <div class="form-group">
            <label for="phone">Phone</label>
            <Field name="phone" type="tel" class="form-control" v-model="contactLocal.phone" />
            <ErrorMessage name="phone" class="error-feedback" />
        </div>
        <div class="form-group form-check">
            <input name="favorite" type="checkbox" class="form-check-input" v-model="contactLocal.favorite" />
            <label for="favorite" class="form-check-label">
                <strong>Favorite</strong>

            </label>
        </div>
        <div class="form-group">
            <button class="btn btn-primary">Save</button>
            <button v-if="contactLocal.id" type="button" class="ml-2 btn btn-danger" @click="deleteContact">
                Delete
            </button>
        </div>
    </Form>
</template>
<script>
import * as yup from 'yup';
import { Form, Field, ErrorMessage } from 'vee-validate';
export default {
    components: {
        Form,
        Field,
        ErrorMessage,
    },
    emits: ['submit:contact', 'delete:contact'],
    props: {
        contact: { type: Object, required: true }
    },
    data() {
        const contactFormSchema = yup.object().shape({
            name: yup
                .string()
                .required('T??n ph???i c?? gi?? tr???.')
                .min(2, 'T??n ph???i ??t nh???t 2 k?? t???.')
                .max(50, 'T??n c?? nhi???u nh???t 50 k?? t???.'),
            email: yup
                .string()
                .email('E-mail kh??ng ????ng.')
                .max(50, 'E-mail t???i ??a 50 k?? t???.'),
            address: yup.string().max(100, '?????a ch??? t???i ??a 100 k?? t???.'),
            phone: yup
                .string()
                .matches(
                    /((09|03|07|08|05)+(\d{8})\b)/g,
                    'S??? ??i???n tho???i kh??ng h???p l???.'
                ),
        });
        return {
            contactLocal: {
                ...this.contact, favorite: !!this.contact.favorite
            },
            contactFormSchema
        };
    },
    methods: {
        submitContact() {
            this.$emit('submit:contact', this.contactLocal);
        },
        deleteContact() {
            this.$emit('delete:contact', this.contactLocal.id);
        },
    },
};
</script>
<style scoped>
@import '@/assets/form.css';
</style>