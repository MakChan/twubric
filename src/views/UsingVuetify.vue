<template>
  <v-container>
    <v-layout
      text-center
      wrap
    >
      <v-flex xs12>

        <v-data-iterator
          :items="followers"
          :search="`${startDate},${endDate}`"
          :sort-by="sortBy"
          :sort-desc="sortDesc"
          :custom-filter="dateFilter"
          hide-default-footer
        >
          <template v-slot:header>

            <h3 class="text-left">Sort By</h3>

            <v-flex
              xs12
              sm8
              class="py-2 d-flex flex-row flex-"
            >
              <v-select
                v-model="sortBy"
                solo
                hide-details
                :items="sortItems"
                prepend-inner-icon="sort"
                label="Sort by"
              ></v-select>

              <v-btn-toggle
                v-model="sortDesc"
                class="ml-4 ml-md-10"
              >

                <v-tooltip bottom>
                  <template v-slot:activator="{ on }">
                    <v-btn
                      :value="false"
                      v-on="on"
                      text
                    >
                      <v-icon>arrow_upward</v-icon>
                    </v-btn>

                  </template>
                  <span>Ascending Order</span>
                </v-tooltip>

                <v-tooltip bottom>
                  <template v-slot:activator="{ on }">
                    <v-btn
                      :value="true"
                      v-on="on"
                      text
                    >
                      <v-icon>arrow_downward</v-icon>
                    </v-btn>

                  </template>
                  <span>Descending Order</span>
                </v-tooltip>

              </v-btn-toggle>
            </v-flex>

            <h3 class="text-left mt-3">Joined Twitter between</h3>

            <v-container fluid>
              <v-row>
                <v-col
                  cols="12"
                  sm="6"
                  md="4"
                >
                  <v-menu
                    v-model="menu1"
                    :close-on-content-click="false"
                    transition="scale-transition"
                    offset-y
                    min-width="290px"
                  >
                    <template v-slot:activator="{ on }">
                      <v-text-field
                        v-model="startDate"
                        label="Start Date"
                        prepend-icon="event"
                        class="pt-0"
                        readonly
                        v-on="on"
                      ></v-text-field>
                    </template>
                    <v-date-picker
                      v-model="startDate"
                      @input="menu1 = false"
                      no-title
                      scrollable
                    >

                    </v-date-picker>
                  </v-menu>
                </v-col>
                <v-col
                  cols="12"
                  sm="6"
                  md="4"
                >
                  <v-menu
                    v-model="menu2"
                    :close-on-content-click="false"
                    transition="scale-transition"
                    offset-y
                    min-width="290px"
                  >
                    <template v-slot:activator="{ on }">
                      <v-text-field
                        v-model="endDate"
                        label="End date"
                        prepend-icon="event"
                        class="pt-0"
                        readonly
                        v-on="on"
                      ></v-text-field>
                    </template>
                    <v-date-picker
                      v-model="endDate"
                      @input="menu2 = false"
                      no-title
                      scrollable
                    >

                    </v-date-picker>
                  </v-menu>
                </v-col>
              </v-row>
            </v-container>
          </template>
          <template v-slot:default="props">

            <v-container fluid>
              <v-row>
                <v-col
                  v-for="item in props.items"
                  :key="item.name"
                  cols="12"
                  sm="6"
                  md="4"
                  lg="3"
                >
                  <v-card outlined>
                    <h4 class="d-flex justify-space-between align-center px-4 py-2">
                      <span>{{ item.fullname }}</span>
                      <span class="indigo--text font-weight-bold title">{{ item.twubric.total}}</span>
                    </h4>

                    <v-divider></v-divider>

                    <div class="d-flex align-center pa-3">
                      <img
                        :src="require(`@/assets/${item.image}`)"
                        class="avatar"
                      />
                      <div class="px-3 text-left">
                        <div>
                          <span class="grey--text text--darken-2">Friends</span>
                          {{ item.twubric.friends}}
                        </div>
                        <div>
                          <span class="grey--text text--darken-2">Influence</span>
                          {{ item.twubric.influence}}
                        </div>
                        <div>
                          <span class="grey--text text--darken-2">Chirpiness</span>
                          {{ item.twubric.chirpiness}}
                        </div>
                      </div>
                    </div>

                    <v-divider></v-divider>

                    <div class="d-flex justify-space-between py-2 px-3">
                      <span>{{ new Date(item.join_date).toLocaleDateString('en-IN', dateOptions) }}</span>

                      <v-tooltip bottom>
                        <template v-slot:activator="{ on }">

                          <v-btn
                            class=""
                            icon
                            text
                            small
                            color="red"
                            v-on="on"
                            v-on:click="removeFollower(item.uid)"
                          >
                            <v-icon>delete</v-icon>
                          </v-btn>
                        </template>
                        <span>Delete</span>
                      </v-tooltip>

                    </div>
                  </v-card>
                </v-col>
              </v-row>
            </v-container>
          </template>
          <template v-slot:no-results>
            <div class="text-left">
              No matching items found
            </div>
          </template>
        </v-data-iterator>
      </v-flex>
    </v-layout>
  </v-container>
</template>

<style scoped>
.avatar {
  max-width: 60px;
}
</style>

<script>
import jsonData from "../data.json";

function dateToUnix(date) {
  return parseInt((new Date(date).getTime() / 1000).toFixed(0));
}

export default {
  data: () => ({
    sortBy: "username",
    sortDesc: false,
    startDate: null,
    endDate: null,
    menu1: false,
    menu2: false,
    followers: jsonData,
    sortItems: [
      {
        value: "twubric.total",
        text: "Twubric Score"
      },
      {
        value: "twubric.friends",
        text: "Friends"
      },
      {
        value: "twubric.influence",
        text: "Influence"
      },
      {
        value: "twubric.chirpiness",
        text: "Chirpiness"
      }
    ],
    dateOptions: {
      year: "numeric",
      month: "long",
      day: "numeric"
    }
  }),
  methods: {
    dateFilter(followers, search) {
      const dates = search.split(",");

      const unixStartDate = dates[0] !== "null" ? dateToUnix(dates[0]) : 0;
      const unixEndDate = dates[1] != "null" ? dateToUnix(dates[1]) : Infinity;

      return followers.filter(
        follower =>
          follower.join_date > unixStartDate && follower.join_date < unixEndDate
      );
    },
    removeFollower(followerId) {
      this.followers = this.followers.filter(
        follower => follower.uid !== followerId
      );
    }
  }
};
</script>


