<template>
  <v-container fluid>
    <v-row>
      <v-col
        cols="12"
        sm="4"
        md="3"
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
            />
          </template>
          <v-date-picker
            v-model="startDate"
            @input="menu1 = false;filter(startDate, endDate)"
            :max="endDate"
            no-title
            scrollable
          />

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
            />
          </template>
          <v-date-picker
            v-model="endDate"
            @input="menu2 = false;filter(startDate, endDate)"
            :min="startDate"
            no-title
            scrollable
          />

        </v-menu>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  props: ["filter"],
  data: () => ({
    startDate: null,
    endDate: null,
    menu1: false,
    menu2: false
  })
};
</script>