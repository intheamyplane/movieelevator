<template>
  <div class="genre-menu">
    
    <label for="genres">Select genres:</label>

    <table class="genre-table">
      <tr></tr>
      <tbody>
        <tr v-for="genre in $store.state.genres" :key="genre.id">
          <td>
            {{ genre.genreName }}
          </td>
          <td>
            <input
              type="checkbox"
              v-bind:id="genre.id"
              v-bind:value="genre.id"
              v-model="$store.state.user.preferences"
            />
          </td>
        </tr>
      </tbody>
    </table>
    <div class="update-button">
      <button @click="updatePreferences">Save Genre Preferences</button>
    </div>
  </div>
</template>

<script>
import MovieService from "../services/MovieService";
export default {
  created() {
    MovieService.getAllGenres().then((response) => {
      console.log(`loaded ${response.data.length} genres`);
      this.$store.commit("SET_GENRES", response.data);
    });
  },
  computed: {
    currentUserId() {
      return this.$store.state.user.id;
    },
  },
  methods: {
    updatePreferences() {
      MovieService.addUserPrefs(this.$store.state.user).then(() => {
        // Save user preferences to local storage
        localStorage.setItem(
          "user_preferences",
          JSON.stringify(this.$store.state.user.preferences)
        );
        window.alert("Genre Preferences saved");
      });
    },
  },
};
</script>
<style scoped>
.genre-menu {
  display: flex;
  justify-content: center;
  flex-direction: column;
  border: 3px solid#FFC107;
  border-radius: 10px;
  padding-bottom: 10px;
  padding-top: 10px;
  background-color: rgb(51, 49, 49);
}
.update-button {
  display: flex;
  justify-content: center;
  padding-top: 20px;
}

.genre-table {
  display: flex;
  justify-content: center;
  font-size: 20px;
}
label {
  text-align: center;
  font-size: 20px;
  font-weight: bold;
}
input {
  margin-left: 70px;
  margin-right: 17px;
}
button {
  padding: 8px;
  background-color: #ffc107;
  border-radius: 6px;
  font-size: 14px;
}
button:hover {
  background-color: wheat;
}

</style>







