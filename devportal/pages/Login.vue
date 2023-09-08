<template>
  <section class="section">
    <div class="container">
      <div class="columns">
        <div class="column is-4 is-offset-4">
          <h2 class="text-centered">Sign In To DevDevPortal</h2>

          <Notification :message="error" v-if="error" />

          <form @submit.prevent="login">
            <div class="field">
              <label class="label"
                >Sign in via Magic Link with your email below:</label
              >
              <div class="control">
                <input
                  type="email"
                  class="input"
                  name="email"
                  v-model="email"
                  required
                />
              </div>
            </div>
            <div class="control">
              <input
                type="submit"
                class="button block"
                :value="loading ? 'Loading' : 'Send Magic Link'"
                :disabled="loading"
              />
            </div>
          </form>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup lang="js">
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
