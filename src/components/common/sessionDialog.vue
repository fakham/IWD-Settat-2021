<template>
  <v-dialog v-model="dialog" width="800">
    <template v-slot:activator="{ on }">
      <div v-on="on" style="cursor: pointer;">
        <v-chip
          color="#4285F4"
          label
          style="color:white; font-size:80%"
          class="mt-1 mb-0 google-font"
          small
        >{{data.vdata.track}}</v-chip>
        <br />
        <v-chip
          color="#00BFA5"
          label
          style="color:white; font-size:80%"
          class="mt-1 mb-0 google-font"
          small
        >{{data.vdata.format}}</v-chip>
        <p class="google-font ma-0 mt-2" style="font-size:110%;">{{data.vdata.title}}</p>
        <p class="google-font mt-1 mb-0" style="font-size:80%;color:#424242">
          {{data.vdata.timeDuration}} min
          <b>|</b>
          {{data.vdata.date}}
        </p>
      </div>
    </template>

    <v-card color v-if="dialog">
      <v-card-title class="px-5 py-5 hidden-sm-and-down align-center justify-center">
        <iframe
          width="750"
          height="400"
          :src="data.vdata.link"
          frameborder="0"
          allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture"
          allowfullscreen
        ></iframe>
      </v-card-title>
      <v-card-title class="hidden-md-and-up align-center justify-center">
        <iframe
          width="450"
          height="200"
          id="mobile"
          :src="data.vdata.link"
          frameborder="0"
          allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture"
          allowfullscreen
        ></iframe>
      </v-card-title>
      <v-card-text class="pa-5">
        <v-chip
          color="#4285F4"
          label
          style="color:white; font-size:90%"
          class="mt-1 mb-0 mr-3 google-font"
          small
        >{{data.vdata.track}}</v-chip>
        <v-chip
          color="#00BFA5"
          label
          class="mt-1 mb-0 google-font"
          style="color:white; font-size:90%"
          small
        >{{data.vdata.format}}</v-chip>

        <h1
          class="mt-5 mb-5 google-font"
          style="color:#4285F4;word-break:normal;"
        >{{data.vdata.title}}</h1>

        <p class="google-font mt-2 mb-0" style="font-size:110%">
          <b>Time Duration:</b>
          {{data.vdata.timeDuration}} min
        </p>
        <p class="google-font mt-3" style="font-size:105%">{{data.vdata.description }}</p>

        <v-flex xs12 md12 v-for="(item,index) in data.vdata.speakers" :key="index">
          <div v-for="(speaker,j) in SpeakersDetails" :key="j">
            <div v-if="speaker.id == item">
              <v-list two-line subheader class="pa-0 ma-0">
                <v-list-item>
                  <v-list-item-avatar>
                    <img :src="getImgUrl(speaker.image)" />
                  </v-list-item-avatar>
                  <v-list-item-content>
                    <v-list-item-title class="google-font" style="color:#424242">{{ speaker.name }}</v-list-item-title>
                    <v-list-item-subtitle
                      class="google-font text-wrap"
                      v-if="speaker.company.designation"
                    >
                      {{ speaker.company.designation }}
                      <span v-if="speaker.company.name">,</span>
                      {{speaker.company.name}}
                    </v-list-item-subtitle>
                  </v-list-item-content>
                </v-list-item>
              </v-list>
            </div>
          </div>
        </v-flex>
      </v-card-text>

      <v-divider></v-divider>

      <v-card-actions>
        <v-spacer></v-spacer>
        <v-btn color="primary" text @click="dialog = false">Close</v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>

<script>
import SpeakersDetails from "@/assets/data/speaker.json";
export default {
  props: {
    data: {
      vdata: []
    }
  },
  data() {
    return {
      dialog: false,
      SpeakersDetails: SpeakersDetails
    };
  },
  methods: {
    getImgUrl(pic) {
      if (pic.length > 0) {
        return require("@/assets/img/speakers/" + pic);
      } else {
        return require("@/assets/img/common/avatar.png");
      }
    }
  },
  filters: {
    summary: (val, num) => {
      if (val.length > num) {
        return val.substring(0, num) + "..";
      } else {
        return val;
      }
    }
  }
};
</script>


<style scoped>
@media only screen and (width: 320px) {
  #mobile { 
    height: 181px;
    width: 260px;
  }
}
@media only screen and (width: 375px) {
  #mobile { 
    height: 181px;
    width: 300px;
  }
}
@media only screen and (width: 425px) {
  #mobile { 
    height: 185px;
  }
}
</style>