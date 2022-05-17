<template>
  <v-container fluid>
    <v-app-bar
      color="black "
      dense
      dark
      elevation="0"
      height="160"
      app
      class="px-2"
    >
      <v-toolbar-title>
        <v-img class="image" width="150" height="150" />
      </v-toolbar-title>
      <div class="d-flex flex-column">
        <v-toolbar-title class="ml-10">
          <h2 class="display-2 font-weight-light">Raumbelegungsplan</h2>
        </v-toolbar-title>
        <h4
          style="font-family: Roboto; font-size: 25px"
          class="font-weight-light ml-10"
        >
          für heute, den
          {{ new Date(karrieretagDaten.datum).toLocaleDateString() }}
        </h4>
      </div>
      <v-spacer></v-spacer>
      <v-toolbar-title class="ml-10">
        <h2 class="display-2 font-weight-medium">{{ timestamp }}</h2>
      </v-toolbar-title>
    </v-app-bar>
    <v-list
      v-for="h in searchCompany"
      :key="h.history_id"
      class="pa-5"
      color="black"
    >
      <!-- grey darken-4 -->
      <v-list-item class="white--text text--lighten-1 font-weight-medium">
        <v-list-item-avatar width="150" height="100">
          <span>{{ h.stock == 'EG' ? h.stock : `${h.stock}. Stock ` }}</span>
        </v-list-item-avatar>
        <v-list-item-avatar width="180" height="100" class="mr-10">
        </v-list-item-avatar>

        <v-list-item-content>
          {{ h.firmen_name }}
        </v-list-item-content>

        <v-list-item-action>
          <span>{{ h.platz }}</span>
        </v-list-item-action>
      </v-list-item>
    </v-list>
    <!-- <div class="mx-10 my-5" id="search">
      <v-text-field
        v-model="searchFirma"
        :class="`bg-white ${locked ? 'bg-opacity-10' : ''} black--text`"
        filled
        hide-details
        single-line
        dense
        :disabled="locked"
      >
        <v-icon
          slot="append-outer"
          @click="
            locked = !locked;
            searchFirma = '';
          "
          class="lock-button mt-0"
          large
          >mdi-magnify</v-icon
        >
      </v-text-field>
    </div> -->
  </v-container>
</template>

<script>
import axios from 'axios';
export default {
  components: {},
  props: {
    firmenJson: {
      type: Array,
    },
  },
  data() {
    return {
      searchFirma: '',
      locked: true,
      timestamp: '',
      flipped: false,
      karrieretagDaten: {},

      // abteilung: '',
      // chooseAbteilungen: ['Informationstechnologie', 'Elektrotechnik', 'Maschinenbau', 'All'],
    };
  },
  async created() {
    await this.getKarrieretagDaten();

    setInterval(this.getTime, 1000);
  },
  mounted() {
    this.autoScroll();
  },

  methods: {
    async getKarrieretagDaten() {
      const { data } = await axios({
        url: '/karrieretag',
        method: 'GET',
      });
      this.karrieretagDaten = data;
    },
    autoScroll() {
      const main = document.querySelector('#main');
      setInterval(function () {
        window.scrollBy(0, 150);
      }, 7000);
      window.onscroll = function () {
        if (
          window.innerHeight + Math.ceil(window.pageYOffset) >=
          main.offsetHeight
        ) {
          window.scrollTo(0, 0);
        }
      };
    },
    getTime() {
      const today = new Date();
      let hours =
        today.getHours() < 10 ? '0' + today.getHours() : today.getHours();
      let minutes =
        today.getMinutes() < 10 ? '0' + today.getMinutes() : today.getMinutes();
      let seconds =
        today.getSeconds() < 10 ? '0' + today.getSeconds() : today.getSeconds();
      const time = hours + ':' + minutes + ':' + seconds;
      const dateTime = time;
      this.timestamp = dateTime;
    },
  },
  computed: {
    // filterAbteilung() {
    //   if (this.abteilung == 'All') {
    //     return this.firmen.filter((el) => el.firmen_name.includes(this.searchFirma));
    //   }
    //   return this.firmen.filter(
    //     (el) => el.fachrichtung.includes(this.abteilung) && el.firmen_name.includes(this.searchFirma),
    //   );
    // },

    searchCompany() {
      return this.firmenJson.filter((el) =>
        el.firmen_name.toLowerCase().includes(this.searchFirma.toLowerCase()),
      );
    },
    icon() {
      return this.locked ? 'lock' : 'lock_open';
    },
  },
};
</script>

<style>
html * {
  scroll-behavior: smooth;
  overflow: scroll;
  overflow-x: hidden;
}

::-webkit-scrollbar {
  width: 0; /* Remove scrollbar space */
  background: transparent; /* Optional: just make scrollbar invisible */
}
/* Optional: show position indicator in red */
::-webkit-scrollbar-thumb {
  background: #ff0000;
}
#search {
  position: fixed;
  bottom: 0;
  right: 0;
  z-index: 100;
}
#clock {
  position: fixed;
  bottom: 0;
  left: 1;
  font-weight: bold;
  z-index: 100;
}
.lock-button {
  pointer-events: auto;
}
.v-text-field {
  width: 400px;
}
.image {
  background-position: center center;
  background-image: url('/logo_n.png');
  background-size: contain;
}
</style>
