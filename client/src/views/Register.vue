<template>
  <div class="container form">
    <Loader v-if="loading"/>
    <b-alert v-model="showDismissibleAlert" variant="danger" dismissible>
      {{ alertMessage }}
    </b-alert>
    <div class="row h-100 justify-content-center">
      <div class="col-6 align-self-center">
        <router-link to="/">
          <div class="w-50 m-auto text-info">
            <svg style="height: auto;vertical-align: middle;fill: currentColor;overflow: hidden;" viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg"><path d="M668.8 967.68c-8.96 0-17.28-6.4-18.56-15.36l-51.84-257.92c-1.92-9.6 3.2-18.56 12.8-21.76 120.32-41.6 200.96-154.24 200.96-280.96 0-163.84-133.76-297.6-298.88-297.6-164.48 0-298.88 133.12-298.88 297.6 0 126.72 80.64 239.36 200.96 280.96 8.96 3.2 14.72 12.8 12.8 21.76l-51.84 257.92c-1.92 10.24-12.16 17.28-22.4 14.72-10.24-1.92-17.28-12.16-14.72-22.4l48.64-241.92c-126.72-51.2-211.2-174.08-211.2-311.68C175.36 206.08 326.4 55.68 512.64 55.68c185.6 0 337.28 150.4 337.28 336 0 137.6-83.84 260.48-211.2 311.68l48.64 241.92c1.92 10.24-4.48 20.48-14.72 22.4C671.36 967.68 670.08 967.68 668.8 967.68zM133.76 968.32c-1.28 0-2.56 0-3.84-0.64-10.24-1.92-17.28-12.16-14.72-22.4l33.92-167.04C58.88 740.48 0 652.8 0 554.88c0-113.92 78.08-211.84 189.44-236.8 10.24-2.56 20.48 3.84 23.04 14.72 2.56 10.24-3.84 20.48-14.72 23.04C103.68 376.32 38.4 458.88 38.4 554.88c0 87.04 55.68 165.12 138.24 193.92 8.96 3.2 14.72 12.8 12.8 21.76l-37.12 183.04C151.04 961.92 142.72 968.32 133.76 968.32zM890.24 968.32c-8.96 0-17.28-6.4-18.56-15.36l-37.12-183.04c-1.92-9.6 3.2-18.56 12.8-21.76 82.56-28.8 138.24-106.24 138.24-193.92 0-96-65.28-177.92-159.36-199.68-10.24-2.56-16.64-12.8-14.72-23.04s12.8-16.64 23.04-14.72c111.36 25.6 189.44 122.88 189.44 236.8 0 97.92-58.88 186.24-148.48 224l33.92 167.04c1.92 10.24-4.48 20.48-14.72 22.4C892.8 968.32 891.52 968.32 890.24 968.32zM512.64 581.76c-96.64 0-175.36-78.08-175.36-174.72 0-10.88 8.32-19.2 19.2-19.2s19.2 8.32 19.2 19.2c0 74.88 61.44 136.32 136.96 136.32 75.52 0 136.96-60.8 136.96-136.32 0-10.88 8.32-19.2 19.2-19.2s19.2 8.32 19.2 19.2C688 503.68 609.28 581.76 512.64 581.76z"></path></svg>
          </div>
        </router-link>
        <h2 class="text-center">
          Sign up
        </h2>
        <form action="" @submit.prevent="submit">
          <div class="form-group">
						<label for="user_name">Name</label>
						<input 
							type="text" 
							id="user_name"
							class="form-control"
							autofocus
							autocomplete="name"
							v-model="name"
						>
					</div>
          <div class="form-group">
            <label for="user_email">Email</label>
            <input 
              type="text" 
              id="user_email" 
              autofocus 
              autocomplete="email" 
              class="form-control"
              v-model="email"
            >
          </div>
          <div class="form-group">
            <label for="user_password">Password</label>
            <input 
              type="password" 
              id="user_password" 
              autofocus 
              autocomplete="current-password" 
              class="form-control"
              v-model="password"
            >
						<small class="form-text text-muted">Must be at least 6 characters long</small>
          </div>
          <div class="form-group">
            <label for="confirm_password">Password confirmation</label>
            <input 
              type="password" 
              id="confirm_password" 
              autofocus 
              autocomplete="current-password" 
              class="form-control"
              v-model="confirmPass"
            >
          </div>
          <div class="text-center">
            <button type="submit" class="btn btn-lg btn-info">Sign up</button>
            <p class="text-muted">
              Alreade a member?
              <router-link to="/login" class="text-muted text-decoration-none">Log in</router-link>
            </p>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>
<script>
import { required, email, minLength, sameAs } from 'vuelidate/src/validators'
import Loader from '../components/Loader.vue'
export default {
  name: 'Login',
  components: {Loader},
  created() {
    document.title = 'Sign up'
  },
  data: () => ({
		name: '',
    email: '',
    password: '',
		confirmPass: '',
    showDismissibleAlert: false,
    alertMessage: '',
    loading: false
  }),
  mounted() {
    if (localStorage.getItem('token')) {
      this.$router.push('/projects')
    }
  },
  validations: {
		name: {
			required
		},
    email: {
      required,
      email
    },
    password: {
      required,
      minLength: minLength(5)
    },
		confirmPass: {
			sameAsPassword: sameAs('password')
		}
  },
  methods: {
    async submit() {
      if (this.$v.$invalid) {
        this.$v.$touch()
        this.showDismissibleAlert = true
        if (this.$v.name.$invalid) {
          this.alertMessage = 'Name is required'
        } else if (this.$v.password.$invalid) {
            this.alertMessage = 'Password is required and must be at least 5 characters long'
        } else if (this.$v.email.$invalid) {
          this.alertMessage = 'Email is required and must be a valid email address'
				} else if (this.$v.confirmPass.$invalid) {
          this.alertMessage = 'Password confirmation must match password'
        }
        return
      }
      this.loading = true
      try {
        await this.$store.dispatch('register', {
          name: this.name,
          email: this.email,
          password: this.password,
        })
      } catch (e) {
        this.alertMessage = this.$store.state.authErrorMessage
        this.showDismissibleAlert = true
        this.loading = false
      }
      this.$router.push('/login')
    }
  }
}
</script>
<style scoped>

</style>