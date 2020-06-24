<template>
  <div class="form-container">
    <a-card
      class="card"
      title="Sign Up"
      :bordered="false"
    >
      <a-form-model
        :model="form"
        class="form"
      >
        <div class="account-exit">
          <span>Already have an account?</span>
          <a-button
            @click="changeMode"
            type="primary"
          >
            Sign In
          </a-button>
        </div>
        <div class="name-container">
          <a-form-model-item label="First name">
            <a-input v-model="form.fname" />
          </a-form-model-item>
          <a-form-model-item label="Last name">
            <a-input v-model="form.lname" />
          </a-form-model-item>
        </div>
        <a-form-model-item label="Phone number">
          <a-input v-model="form.phone" />
        </a-form-model-item>
        <a-form-model-item label="Email address">
          <a-input v-model="form.email"/>
        </a-form-model-item>
        <a-form-model-item label="Password">
          <a-input v-model="form.password"/>
        </a-form-model-item>
        <a-form-model-item>
          <a-button type="primary" @click="onSubmit">
            Sign Up
          </a-button>
        </a-form-model-item>
      </a-form-model>
    </a-card>
  </div>
</template>

<script>
import axios from 'axios';

const form = {
  fname: '',
  lname: '',
  phone: '',
  email: '',
  password: '',
};

export default {
  props: {
    changeMode: {
      type: Function,
      required: true,
    },
  },
  data() {
    return {
      form: { ...form },
    };
  },
  methods: {
    async onSubmit() {
      try {
        const response = await axios.post('http://localhost:8080/user', { ...this.form });
        console.log(response);
        if (response.status === 201) {
          this.form = { ...form };
          // TODO: user created load user id and redirect user to home page
        } else {
          // TODO: display error message.
        }
      } catch (err) {
        console.log(err);
      }
    },
    onCancel() {
      // reset form on cancel
      this.form = { ...form };
    },
  },
};
</script>

<style lang="scss" scoped>
.form-container {
  display: flex;
  justify-content: center;
  .card {
    width: 360px;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    ::v-deep .ant-card-head {
      width: 100%;
      .ant-card-head-wrapper{
        display: flex;
        justify-content: center;
        .ant-card-head-title{
          flex: unset;
        }
      }
    }
    .form {
      width: 300px;
      .account-exit {
        display: flex;
        justify-content: center;
        align-items: center;
        span {
          margin-right: 12px;
        }
      }
      ::v-deep .ant-form-item-label {
        line-height: unset;
      }
      .name-container {
        display: flex;
        justify-content: space-between;
      }
      .ant-form-item-control {
        button {
          width: 100%;
        }
      }
    }
  }
}
</style>