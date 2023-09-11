<template>
  <section class="d-flex align-center justify-center" style="height: 100vh">
    <div class="container">
      <div class="columns">
        <div class="column">
          <h2 class="d-flex">DevDevPortal</h2>

          <Notification :message="error" v-if="error" />

          <v-form @submit.prevent="login">
            <div class="field">
              <label class="label mt-1"
                >Sign in via Magic Link with your email below:
            </label>
              <div class="control">
                <v-text-field
                  variant="outlined"
                  label="Email"
                  type="email"
                  class="input"
                  name="email"
                  v-model="email"
                  required
                />
              </div>
            </div>
            <div class="control">
              <v-btn
                type="submit"
                class="mt-2"
                block rounded
                color="primary"
                :text="loading ? 'Loading' : 'Send Magic Link'"
                :disabled="loading"
              />
            </div>
          </v-form>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup lang="js">
import Notification from '~/components/Notification.vue';

const supabase = useSupabaseClient();
const loading = ref(false);
const email = ref("");

const login = async () => {
  try {
    loading.value = true;
    const { error } = await supabase.auth.signInWithOtp({ email: email.value });
    if (error) throw error;
    alert("Check your email for the login link!");
  } catch (error) {
    alert(error.error_description || error.message);
  } finally {
    loading.value = false;
  }
};
</script>
<style></style>
