<template>
  <!--begin::Wrapper-->
  <div class="w-lg-500px bg-white rounded shadow-sm p-10 p-lg-15 mx-auto">
    <!--begin::Form-->
    <Form class="form w-100 fv-plugins-bootstrap5 fv-plugins-framework" @submit="onSubmitForgotPassword" id="kt_login_password_reset_form" :validation-schema="forgotPassword" >
      <!--begin::Heading-->
      <div class="text-center mb-10">
        <!--begin::Title-->
        <h1 class="text-dark mb-3">Esqueceu a senha ?</h1>
        <!--end::Title-->

        <!--begin::Link-->
        <div class="text-gray-400 fw-bold fs-4">
          Escreva o email que deseja recuperar a senha:
        </div>
        <!--end::Link-->
      </div>
      <!--begin::Heading-->

      <!--begin::Input group-->
      <div class="fv-row mb-10">
        <label class="form-label fw-bolder text-gray-900 fs-6">Email</label>
        <Field class="form-control form-control-solid" type="emai" placeholder="" id="email" name="email" />
        <div class="fv-plugins-message-container">
          <div class="fv-help-block">
            <ErrorMessage name="email" />
          </div>
        </div>
      </div>
      <!--end::Input group-->

      <!--begin::Actions-->
      <div class="d-flex flex-wrap justify-content-center pb-lg-0">
        <button type="submit" ref="submitButton" id="kt_password_reset_submit" class="btn btn-lg btn-primary fw-bolder me-4" >
          <span class="indicator-label"> Enviar </span>
          <span class="indicator-progress">
            Enviando...
            <span class="spinner-border spinner-border-sm align-middle ms-2" ></span>
          </span>
        </button>

        <router-link to="/sign-in" class="btn btn-lg btn-light-primary fw-bolder">Voltar</router-link >
        <!--<router-link to="/sign-up" class="btn btn-lg btn-light-primary fw-bolder">Cancelar</router-link >-->
      </div>
      <!--end::Actions-->
    </Form>
    <!--end::Form-->
  </div>
  <!--end::Wrapper-->
</template>

<script lang="ts" >
import { defineComponent, ref } from "vue";
import { ErrorMessage, Field, Form } from "vee-validate";
import { useStore } from "vuex";
import * as Yup from "yup";
import { Actions } from "@/store/enums/StoreEnums";
import Swal from "sweetalert2/dist/sweetalert2.min.js";

export default defineComponent({
  name: "password-reset",
  components: {
    Field,
    Form,
    ErrorMessage,
  },
  setup() {
    const store = useStore();

    const submitButton = ref<HTMLButtonElement | null>(null);

    //Create form validation object
    const forgotPassword = Yup.object().shape({
      email: Yup.string().email().required().label("Email"),
    });

    //Form submit function
    const onSubmitForgotPassword = async (values) => {
      // eslint-disable-next-line
      submitButton.value!.disabled = true;
      // Activate loading indicator
      submitButton.value?.setAttribute("data-kt-indicator", "on");

      // dummy delay
      // Send login request
      await store.dispatch(Actions.FORGOT_PASSWORD,  values); // Aqui mudar para chamar função da API

      const [errorName] = Object.keys(store.getters.getErrors);
      const error = store.getters.getErrors[errorName];

      if (!error) {
        Swal.fire({
          text: "Email enviado com sucesso!",
          icon: "success",
          buttonsStyling: false,
          confirmButtonText: "Logar novamente!",
          customClass: {
            confirmButton: "btn fw-bold btn-light-primary",
          },
        });
      } else {
        Swal.fire({
          text: error[0],
          icon: "error",
          buttonsStyling: false,
          confirmButtonText: "Erro - verifique o email !",
          customClass: {
            confirmButton: "btn fw-bold btn-light-danger",
          },
        });
      }

      submitButton.value?.removeAttribute("data-kt-indicator");
      // eslint-disable-next-line
        submitButton.value!.disabled = false;
    };

    return {
      onSubmitForgotPassword,
      forgotPassword,
      submitButton,
    };
  },
});
</script>
