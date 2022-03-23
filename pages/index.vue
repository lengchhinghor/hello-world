
<template>
    <div id="IndexPage">
        <button class='btn' @click='getUserAgeFeatureStatus'>
            Calculate Age
        </button>

        <p v-if='!userAgeFeature && !loading' class='text'>Sorry, This feature has been disabled by the Admin</p>
        <p v-else-if="error">{error}</p>

        <div v-else-if='userAgeFeature'>
            <p class='text'>Calculate your age below by providing your year of birth</p>
            <input
                type='number'
                v-model='birthYear'
            />
            <button class='btn btn-calculate' @click='calcAge'>
                Calculate
            </button>

            <p v-if='age'>You are {{ age }} years old</p>
        </div>
    </div>
</template>

<style scoped>
  .here {
    height: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
  }
</style>
<script>
import * as configCat from "configcat-js";
export default {
  name: "IndexPage",
  data(){
    return {
      userAgeFeature: null,
      error: null,
      birthYear: "",
      age: "",
      loading: true
    };
  },

  methods: {
    async getUserAgeFeatureStatus() {
      try{
        let configCatClient = configCat.createClient("bAzaCERnw06ACHbNVpGAgQ/dJkMIeitMkSA2E-Z6M6T1w");
        const res = await configCatClient.getValueAsync(
          "myvueapp",
          false
        );
        this.userAgeFeature = res;
      }catch (err) {
        this.error = err.message;
      }
      this.loading = false;
    },
    calcAge() {
      const age = 2021 - this.birthYear;
      this.age = age;
      this.birthYear = "";
    },
  },
};
</script>
