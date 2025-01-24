<template>
  <div>
    <div class="bx--grid bx--grid--full-width login-bg">
      <div class="bx--row">
        <div class="bx--offset-md-1 bx--col-md-6 bx--offset-lg-4 bx--col-lg-8">
          <div class="test">
            <cv-tile :light="true" class="login-tile">
              <h2 class="login-title">Log in</h2>
              <div v-if="step === 'username'">
                <InlineNotification
                  v-if="error.login"
                  kind="error"
                  title="Cannot log in:"
                  :sub-title="error.login"
                  low-contrast
                />
                <cv-form @submit.prevent="checkUsername" class="login-form">
                  <cv-text-input
                    label="Username"
                    v-model="username"
                    class="mg-bottom-md"
                    placeholder="Your username"
                    :invalid-message="error.username"
                    ref="usernameInput"
                  ></cv-text-input>
                  <cv-checkbox
                    label="Remember username"
                    class="mg-bottom-md"
                    v-model="rememberUsername"
                    value="checkRememberUsername"
                  />
                  <div class="login-footer">
                    <cv-button
                      kind="primary"
                      :icon="ArrowRight20"
                      class="login-button"
                      >Continue</cv-button
                    >
                  </div>
                </cv-form>
              </div>
              <div v-else-if="step === 'password'">
                <span
                  >Logging in as <strong>{{ username }}</strong></span
                >
                <cv-link @click="goToUsername" class="not-you"
                  >Not you?</cv-link
                >
                <cv-form @submit.prevent="checkPassword" class="login-form">
                  <cv-text-input
                    label="Password"
                    type="password"
                    v-model="password"
                    class="bx--password-input mg-bottom-md"
                    placeholder="Your password"
                    :invalid-message="error.password"
                    password-hide-label="Hide password"
                    password-show-label="Show password"
                    ref="passwordInput"
                  ></cv-text-input>
                  <div class="login-footer">
                    <cv-button
                      kind="primary"
                      :icon="ArrowRight20"
                      class="login-button"
                      >Log in</cv-button
                    >
                  </div>
                </cv-form>
              </div>
            </cv-tile>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import IconService from "@/mixins/icon";
import LoginService from "@/mixins/login";
import StorageService from "@/mixins/storage";
import InlineNotification from "@/components/InlineNotification";
import UtilService from "@/mixins/util";
import { mapState } from "vuex";
import { mapActions } from "vuex";

import to from "await-to-js";

export default {
  name: "Login",
  components: { InlineNotification },
  mixins: [IconService, LoginService, StorageService, UtilService],
  data() {
    return {
      username: "",
      password: "",
      rememberUsername: false,
      step: "username",
      error: {
        username: "",
        password: "",
        login: "",
      },
    };
  },
  computed: {
    ...mapState(["loggedUser"]),
  },
  mounted() {
    console.log("mounted login page"); ////
    this.focusUsername();
  },
  methods: {
    ...mapActions(["setLoggedUserInStore"]),
    checkUsername() {
      this.error.username = "";

      if (!this.username.trim()) {
        this.error.username = "Username is required";
        this.focusUsername();
      } else {
        this.step = "password";
        this.focusPassword();
      }
    },
    async checkPassword() {
      this.error.password = "";

      if (!this.password.trim()) {
        this.error.password = "Password is required";
        this.focusPassword();
      } else {
        // invoke login API
        const [loginError, response] = await to(
          this.login(this.username, this.password)
        );

        if (loginError) {
          this.handleLoginError(loginError);
          return;
        }

        const loginInfo = {
          username: this.username,
          token: response.data.token,
        };

        this.saveToStorage("loginInfo", loginInfo);
        this.setLoggedUserInStore(this.username);

        if (this.$route.name !== "Dashboard") {
          this.$router.push("dashboard");
        }
      }
    },
    handleLoginError(error) {
      let errorMessage = this.GENERIC_ERROR_MESSAGE;

      if (error.response) {
        switch (error.response.data.Code) {
          case 401:
            errorMessage = "Invalid username or password";
            break;
        }
      } else if (error.message === "Network Error") {
        errorMessage = "Network error"; //// use i18n string
      }

      this.error.login = errorMessage;
      this.step = "username";
      this.password = "";
      this.focusUsername();
    },
    goToUsername() {
      this.step = "username";
      this.username = "";
      this.password = "";
      this.focusUsername();
      this.error.login = "";
    },
    focusUsername() {
      // focus on username field
      this.$nextTick(() => {
        const usernameInput = this.$refs.usernameInput;
        usernameInput.focus();
      });
    },
    focusPassword() {
      // focus on password field
      this.$nextTick(() => {
        const passwordInput = this.$refs.passwordInput;
        passwordInput.focus();
      });
    },
  },
};
</script>

<style scoped lang="scss">
@import "../styles/carbon-utils";

.login-bg {
  height: calc(100vh - 3rem);
  background-color: $interactive-02;
}

.login-tile {
  margin-top: $spacing-13;
  padding-bottom: 0;
}

.login-title {
  margin-bottom: $spacing-03;
}

.login-form {
  margin-top: $spacing-07;
}

.login-footer {
  display: flex;
  height: 4rem;
  justify-content: flex-end;
  margin-top: $spacing-07;
}

.login-button {
  width: 50%;
  height: 100%;
  margin-right: -1rem;
}

.not-you {
  margin-left: $spacing-03;
}
</style>
