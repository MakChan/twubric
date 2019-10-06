<template>
  <div class="card">
    <h4 class="d-flex justify-space-between align-center px-4 py-2 title">
      <span class="">{{ follower.fullname }}</span>
      <span class="indigo--text font-weight-bold ">{{ follower.twubric.total}}</span>
    </h4>

    <div class="d-flex align-center pa-3">
      <img
        :src="require(`@/assets/${follower.image}`)"
        class="avatar"
      />
      <div class="px-3 text-left">
        <div>
          <span class="grey--text text--darken-2">Friends</span>
          {{ follower.twubric.friends}}
        </div>
        <div>
          <span class="grey--text text--darken-2">Influence</span>
          {{ follower.twubric.influence}}
        </div>
        <div>
          <span class="grey--text text--darken-2">Chirpiness</span>
          {{ follower.twubric.chirpiness}}
        </div>
      </div>
    </div>

    <div class="d-flex justify-space-between py-2 px-3">
      <span class="text--secondary">{{ new Date(follower.join_date).toLocaleDateString('en-IN', dateOptions) }}</span>

      <v-tooltip bottom>
        <template v-slot:activator="{ on }">

          <v-btn
            class=""
            icon
            text
            small
            color="red"
            v-on="on"
            v-on:click="removeFollower(follower.uid)"
          >
            <v-icon>delete</v-icon>
          </v-btn>
        </template>
        <span>Delete</span>
      </v-tooltip>

    </div>
  </div>
</template>

<style>
.card {
  display: inline-block;
  margin: 1rem;
  width: 300px;
  background-color: #fff;
  box-shadow: #eaedff 0px 0px 20px;
  transition: box-shadow 100ms ease-in-out;
}

.card:hover {
  box-shadow: #eaedff 0px 0px 1px;
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.3s;
}
.fade-leave-active {
  position: absolute;
}
.fade-enter,
.fade-leave-to {
  opacity: 0;
}
.fade-move {
  transition: transform 0.3s;
}
</style>

<script>
export default {
  data: () => ({
    dateOptions: {
      year: "numeric",
      month: "long",
      day: "numeric"
    }
  }),
  props: ["follower", "removeFollower"]
};
</script>


