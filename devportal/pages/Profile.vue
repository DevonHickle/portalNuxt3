<template>
  <v-form @submit.prevent="updateProfile">
    <div>
      <v-label for="email">Email:</v-label>
      <v-text-field
        id="email"
        v-model="user.email"
        type="email"
        disabled />
    </div>
    <div>
      <v-label for="username">Username:</v-label>
      <v-text-field
        id="username"
        v-model="username"
        type="text" />
    </div>
    <div>
      <v-label for="website">Website:</v-label>
      <v-text-field
        id="website"
        v-model="website"
        type="text" />
    </div>
    <div>
      <v-btn
        type="submit"
        class="mt-2"
        block rounded
        :value="loading ? 'Loading' : 'Update Profile'"
        :disabled="loading"
      />
    </div>
    <div>
      <v-btn 
        class="mt-2"
        block rounded
        color="primary"
        :text="loading ? 'Loading' : 'Sign Out'"
        :disabled="loading"
        @click="signOut"
      />
    </div>
  </v-form>
</template>
<script setup>
const supabase = useSupabaseClient();
const loading = ref(true)
const username = ref('')
const website = ref('')
const avatar_path = ref('')

loading.value = true
const user = useSupabaseUser

let { data } = await supabase
  .from('profiles')
  .select(`username, website, avatar_url`)
  .eq('id', user.value.id)
  .single()

if (data) {
  username.value = data.username
  website.value = data.website
  avatar_path.value = data.avatar_url
}

loading.value = false

async function updateProfile() {
  try{
    loading.value = true
    const user = useSupabaseUser()

    const update = {
      id: user.value.id,
      username: username.value,
      website: website.value,
      avatar_url: avatar_path.value,
      updated_at: new Date()
    }

    let { error } = await supabase.from('profiles').upsert(updates, {
      returning: 'minimal',
    })
    if (error) throw error
  } catch (error) {
    alert(error.message)
  } finally {
    loading.value = false
  }
  }
  
async function signOut() {
  try {
    loading.value = true;
    let { error } = await supabase.auth.signOut();
    if (error) throw error;
    user.value = null;
  } catch (error) {
    alert(error.message);
  } finally {
    loading.value = false;
  }
}
</script>
