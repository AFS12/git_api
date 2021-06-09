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
            v-model="userFind"
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
              <v-avatar @click="userLink(user.html_url)">
                <img :src="user.avatar_url" />
              </v-avatar>
              <h2 @click="userLink(user.html_url)">&nbsp; {{ user.name }}</h2>
            </v-card-title>
            <v-card-subtitle>
              <p class="">{{ user.bio }}</p>
            </v-card-subtitle>
            <v-card-text>
              <p>
                <v-btn small text :href="user.followers_url" target="_blank"
                  >Followers: {{ user.followers }}
                </v-btn>

                <v-btn small text :href="user.following_url" target="_blank"
                  >Following: {{ user.following }}
                </v-btn>
                
              </p>
              <p>
                <v-btn small text :href="user.blog" target="_blank">
                  {{ user.blog }}
                </v-btn>
              </p>
              <br />
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
    userFind: "",
    user: "",
    showAvatar: false,
    repos: ""
  }),

  methods: {
    test: function() {
      console.log(this.userFind);
    },

    userLink: function(url) {
      window.open(url, "blank_");
    },

    getApi: function() {
      axios.get(this.getUrl() + this.userFind).then(response => {
        console.log(response.data);
        this.user = response.data;
        this.showAvatar = true;
        axios.get(this.getUrl() + this.userFind + "/repos").then(repos => {
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
