<template>
  <div class="form-container">
    <form
      action="https://dmb_doubledot.dev.doubledot.es/api/auto_contact/test"
      @submit="checkForm"
      method="post"
    >
      <!-- email -> validar email | required -->
      <label for="model-email">Email *</label>
      <input
        type="email"
        v-model="formModel.email"
        id="model-email"
        name="email"
        placeholder="email@email.com"
        :class="{ 'incorrect-input': !isEmail }"
        required
      />

      <!-- nombre -> required -->
      <label for="model-name">Nombre *</label>
      <input
        type="text"
        v-model="formModel.name"
        id="model-name"
        name="name"
        required
      />

      <!-- apellidos -> required -->
      <label for="model-surname">Apellidos *</label>
      <input
        type="text"
        v-model="formModel.surname"
        id="model-surname"
        name="surname"
        required
      />

      <!-- direccion -> required -->
      <label for="model-address">Dirección *</label>
      <input
        type="text"
        v-model="formModel.address"
        id="model-address"
        name="address"
        required
      />

      <label for="model-province">Provincia</label>
      <input
        type="text"
        v-model="formModel.province"
        id="model-province"
        name="province"
        placeholder="Madrid"
      />

      <!-- codigo_postal -> required | 5 digitos -->
      <label for="model-postCode">Código Postal *</label>
      <input
        type="number"
        v-model="formModel.postCode"
        id="model-postCode"
        name="postCode"
        placeholder="28036"
        :class="{ 'incorrect-input': !isPostCode }"
        required
      />

      <input type="submit" value="Enviar" />
    </form>
  </div>
</template>

<script>
import { reactive, computed } from "vue";
export default {
  name: "App",
  setup(props, context) {
    const formModel = reactive({});

    // validation for email
    const isEmail = computed(() => {
      if (!formModel?.email) return true;
      // regex obtained from google to use a custom check on emails
      const emailRegex = /^(([^<>()\[\]\\.,;:\s@"]+(\.[^<>()\[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;
      return emailRegex.test(formModel.email);
    });

    // validation for post code
    const isPostCode = computed(() => {
      if (!formModel?.postCode) return true;
      return formModel.postCode.toString().length === 5;
    });

    //validation before sending the request
    const checkForm = (e) => {
      // dont send the request if post code is incorrect
      // for another cases the default validations from the browser will act
      if (isPostCode.value) return true;

      // stop the submit
      e.preventDefault();
    };

    return {
      formModel,
      isEmail,
      isPostCode,
      checkForm,
    };
  },
};
</script>

<style>
/* to remove the style when you focus an input and clearly see the red border
if its incorrect while typing*/
input:focus {
  outline: none;
}

.form-container {
  display: flex;
  justify-content: center;
}

form {
  display: flex;
  flex-direction: column;
  width: 30%;
}
/* affects all the inputs in the page but the submit one */
input:not([type="submit"]) {
  margin-bottom: 16px;
  border: 1px solid #000;
  border-radius: 4px;
  height: 24px;
}

input[type="submit"] {
  border-radius: 24px;
  height: 48px;
  background-color: #0a66c2;
  border: 0;
  color: #fff;
}

.incorrect-input {
  border-color: red !important;
}

.correct-input {
  border-color: green !important;
}
</style>
