<template>
  <ion-page>
    <ion-header :translucent="true">
      <ion-toolbar>
        <ion-title>Blank</ion-title>
      </ion-toolbar>
    </ion-header>

    <ion-content :fullscreen="true">
      <ion-header collapse="condense">
        <ion-toolbar>
          <ion-title size="large">Blank</ion-title>
        </ion-toolbar>
      </ion-header>

      <div>
        <h1>{{ year }}</h1>
        <div>
          <button @click="updateMonth(-1)">prev</button>
          <h2>{{ monthName }}</h2>
          <button @click="updateMonth(1)">next</button>
        </div>
      </div>
      <div class="calender">
        <ul class="day-names">
          <li v-for="day in dayNames">
            {{ day }}
          </li>
        </ul>
        <ul class="dates">
          <li v-for="n in datelist">
            {{ n }}
          </li>
        </ul>
      </div>
    </ion-content>
  </ion-page>
</template>

<script setup lang="ts">
import {
  IonContent,
  IonHeader,
  IonPage,
  IonTitle,
  IonToolbar,
} from "@ionic/vue";
import { computed, ref } from "vue";

const dayNames = [
  "Sunday",
  "Monday",
  "Tuesday",
  "Wednesday",
  "Thursday",
  "Friday",
  "Saturday",
];
const monthNames = [
  "January",
  "February",
  "March",
  "April",
  "May",
  "June",
  "July",
  "August",
  "September",
  "October",
  "November",
  "December",
];

const year = ref(new Date().getFullYear());
const month = ref(new Date().getMonth());
const monthName = computed(() => {
  return monthNames[month.value];
});

const lastDateOfMonth = computed(() => {
  return new Date(year.value, month.value + 1, 0).getDate();
});
const lastDayOfMonth = computed(() => {
  return new Date(year.value, month.value, lastDateOfMonth.value).getDay();
});
const lastDateOfLastMonth = computed(() => {
  return new Date(year.value, month.value, 0).getDate();
});
const lastDayOfLastMonth = computed(() => {
  return new Date(year.value, month.value, lastDateOfLastMonth.value).getDay();
});
const firstDayOfMonth = computed(() => {
  return new Date(year.value, month.value, 1).getDay();
});

let datelist: any[] = [];
const updateDatelist = computed(() => {
  datelist = [];
  let nextmonthdate = 0;
  for (let i = 0; i < firstDayOfMonth.value; i++) {
    let prevdate = lastDateOfLastMonth.value - firstDayOfMonth.value + i + 1;
    datelist.push(prevdate);
  }
  for (let i = 1; i <= lastDateOfMonth.value; i++) {
    datelist.push(i);
  }
  for (let i = lastDayOfMonth.value + 1; i < 7; i++) {
    nextmonthdate++;
    datelist.push(nextmonthdate);
  }
  return datelist;
});

function updateMonth(num: number) {
  if (month.value) {
  }
  month.value = month.value + num;
}
</script>

<style scoped>
.day-names {
  display: flex;
  flex-wrap: wrap;
}
.day-names li {
  width: calc(100% / 7);
}

.dates {
  display: flex;
  flex-wrap: wrap;
}
.dates li {
  width: calc(100% / 7);
}
</style>
