<template>
  <v-app>
    <v-app-bar app color="primary" dark>
      <v-spacer></v-spacer>
      <div class="d-flex align-center">
        <h1>Consuming Github API</h1>
      </div>

      <v-spacer></v-spacer>
    </v-app-bar>

    <v-main>
      <br />
      <v-row>
        <v-col md="4"></v-col>
        <v-col md="4">
          <v-text-field
            label="Github User"
            placeholder="Placeholder"
            v-model="user"
          ></v-text-field>
          <v-btn color="accent" elevation="4" outlined @click="getApi"
            >Search</v-btn
          >
        </v-col>
        <v-col md="4"></v-col>
      </v-row>
      <br />
      <v-divider class=""></v-divider>
      <br />
      <v-row>
        <v-col md="2"></v-col>
        <v-col md="8">
          <v-card elevation="4" v-show="showAvatar">
            <v-card-title>
              <v-avatar>
                <img :src="avatar" />
              </v-avatar>
              <h2>&nbsp; {{ name }}</h2>
            </v-card-title>
            <v-card-text>
              <p class="">{{ bio }}</p>
              <p>{{ blog }}</p>
              <br>
              <h1>Repositories:</h1>
            </v-card-text>
            <v-row v-for="(repo, index) in repos" :key="index">
              <v-col md="2"></v-col>
              <v-col md="8">
                <v-card elevation="2" v-show="showAvatar">
                  <v-card-title>
                    <h2>{{ repo.name }}</h2>
                  </v-card-title>
                  <v-card-text>
                    <p class="">{{ repo.language }}</p>
                    <p>{{ repo.description }}</p>
                    <p>stars: {{ repo.stargazers_count }}</p>
                  </v-card-text>
                  <v-card-actions>
                    <v-btn text color=""> </v-btn>
                  </v-card-actions>
                </v-card>
              </v-col>
              <v-col md="2"></v-col>
            </v-row>
          </v-card>
        </v-col>
        <v-col md="2"></v-col>
      </v-row>
    </v-main>
  </v-app>
</template>

<script>
import axios from "axios";

export default {
  name: "App",

  data: () => ({
    user: "",
    avatar: "",
    showAvatar: false,
    name: "",
    bio: "",
    blog: "",
    repos: ""
  }),

  methods: {
    test: function() {
      console.log(this.user);
    },

    getApi: function() {
      axios.get(this.getUrl() + this.user).then(response => {
        console.log(response.data);
        this.avatar = response.data.avatar_url;
        this.name = response.data.name;
        this.showAvatar = true;
        this.bio = response.data.bio;
        this.blog = response.data.blog;
        axios.get(this.getUrl() + this.user + "/repos").then(repos => {
          console.log(repos.data[6]);
          this.repos = repos.data;
        });
      });
    },

    getUrl: function() {
      return "https://api.github.com/users/";
    }
  }
};
</script>
