<template>
  <v-app id="inspire">
    <v-content>
      <v-container
        class="fill-height"
        fluid
      >
        <v-row
          align="center"
          justify="center"
        >
          <v-col
            cols="12"
            sm="8"
            md="4"
          >
            <v-card class="elevation-12">
              <v-toolbar
                color="primary"
                dark
                flat
              >
                <v-toolbar-title>Login form</v-toolbar-title>
                <v-spacer></v-spacer>
              </v-toolbar>
              <v-card-text>
                <v-form>
                  <v-text-field
                    label="Email"
                    name="login.email"
                    prepend-icon="mdi-email"
                    type="text"
                    v-model="login.email"
                  ></v-text-field>

                  <v-text-field
                    id="password"
                    label="Password"
                    name="login.password"
                    prepend-icon="mdi-lock"
                    type="password"
                    v-model="login.password"
                  ></v-text-field>
                </v-form>
              </v-card-text>
              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn @click="userLogin" color="primary">Login</v-btn>
              </v-card-actions>
            </v-card>
          </v-col>
        </v-row>
      </v-container>
    </v-content>
  </v-app>
</template>

<script>
 export default {
  props: {
      source: String,
    },
   auth: false,
   name: "userLogin",
      data() {
        return {
        login: {
          email: 'niyas1@gmail.com',
          password: '123456'
      }
        }
      },
       mounted: function () {
      },
      methods: {
      async userLogin() {
      try {
        let response = await this.$auth.loginWith('local', { data: this.login })
        this.$router.push('/');
        console.log(response)
      } catch (err) {
        console.log(err)
      }
    },
      },
    };
    </script>