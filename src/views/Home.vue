<template>
  <v-container class="pt-10">

    <h3 class="ml-4 mb-3">Sort By</h3>
    <div class="ml-n2">
      <Button
        v-for="(button, index) in sortButtons"
        :buttonData="button"
        :sort="sortFollowers"
        :key="index"
        :index="index"
      />
    </div>

    <h3 class="ml-4 mt-6">Joined Twitter between</h3>

    <DatePickers :filter="filterFollowers" />

    <transition-group
      name="fade"
      tag="div"
      class="d-flex flex-wrap mt-2"
    >
      <ProfileCard
        v-for="follower in followers"
        :follower="follower"
        :removeFollower="removeFollower"
        :key="follower.uid"
      />
    </transition-group>

  </v-container>
</template>

<script>
import ProfileCard from "../components/Card";
import Button from "../components/Button";
import DatePickers from "../components/DatePickers";
import jsonData from "../data.json";

function dateToUnix(date) {
  return parseInt(new Date(date).getTime());
}

export default {
  name: "home",
  data: () => ({
    followers: jsonData,
    sortButtons: [
      {
        asc: true,
        active: false,
        value: "total",
        text: "Twubric Score"
      },
      {
        asc: true,
        active: false,
        value: "friends",
        text: "Friends"
      },
      {
        asc: true,
        active: false,
        value: "influence",
        text: "Influence"
      },
      {
        asc: true,
        active: false,
        value: "chirpiness",
        text: "Chirpiness"
      }
    ]
  }),
  components: {
    ProfileCard,
    Button,
    DatePickers
  },
  methods: {
    removeFollower(followerId) {
      this.followers = this.followers.filter(
        follower => follower.uid !== followerId
      );
    },
    sortFollowers({ value, active, asc }, index) {
      if (active) {
        this.sortButtons.splice(index, 1, {
          ...this.sortButtons[index],
          asc: !this.sortButtons[index].asc
        });
        this.followers.reverse();
      } else {
        this.sortButtons = this.sortButtons.map(button => {
          if (button.value === value) button.active = true;
          else button.active = false;
          return button;
        });
        this.followers.sort((a, b) => {
          if (asc) return a["twubric"][value] - b["twubric"][value];
          return b["twubric"][value] - a["twubric"][value];
        });
      }
    },
    filterFollowers(startDate, endDate) {
      const unixStartDate = startDate != null ? dateToUnix(startDate) : 0;
      const unixEndDate = endDate != null ? dateToUnix(endDate) : Infinity;

      this.followers = jsonData.filter(
        follower =>
          follower.join_date > unixStartDate && follower.join_date < unixEndDate
      );
    }
  }
};
</script>
