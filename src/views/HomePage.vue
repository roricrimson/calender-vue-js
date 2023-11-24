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
        <div>
          <button @click="updateYear(-1)">prev</button>
          <h2>{{ year }}</h2>
          <button @click="updateYear(1)">next</button>
        </div>
        <div>
          <button @click="updateMonth(-1)">prev</button>
          <h2>{{ monthName }}</h2>
          <button @click="updateMonth(1)">next</button>
        </div>
        <div class="calender">
          <ul class="day-names">
            <li v-for="day in dayNames">
              {{ day }}
            </li>
          </ul>
          <ul class="dates">
            <li :class="{ current: n.month === 'curr', selected: n.month === 'selected'}" v-for="n in updateDatelist">
              {{ n.date }}
            </li>
          </ul>
        </div>
      </div>

      <div>
        <ul class="day-names">
          <li v-for="day in dayNames">
            {{ day }}
          </li>
        </ul>
        <ul class="dates">
          <li v-for="n in updateWeekList">
            {{ n.date }}
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

const time = ref(new Date())
const year = ref(time.value.getFullYear());
const month = ref(time.value.getMonth());
const monthName = computed(() => {
  return monthNames[month.value];
});
const selectedTime = ref(new Date())
const selectedDate = ref(selectedTime.value.getDate());
const selectedDay = ref(selectedTime.value.getDay());
const selectedMonth = ref(selectedTime.value.getMonth());
const selectedYear = ref(selectedTime.value.getFullYear());

let weekList: any[] = [];
const updateWeekList = computed(() => {
  weekList = [];
  const firstDateOfWeek = selectedDate.value - selectedDay.value;
  updateDatelist.value.forEach((item) => {
    if (item.date >= firstDateOfWeek && item.date < firstDateOfWeek + 7) {
      weekList.push(item);
    }
  });

  return weekList;
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
    datelist.push({ date: prevdate, month: "prev" });
  }
  for (let i = 1; i <= lastDateOfMonth.value; i++) {
    if(i === selectedDate.value && selectedMonth.value === month.value && selectedYear.value === year.value){
      datelist.push({ date: i, month: "selected" });
    }else{
      datelist.push({ date: i, month: "curr" });
    }
  }
  for (let i = lastDayOfMonth.value + 1; i < 7; i++) {
    nextmonthdate++;
    datelist.push({ date: nextmonthdate, month: "next" });
  }

  return datelist;
});

function updateMonth(num: number) {
  month.value = month.value + num
  if (month.value === -1) {
    updateYear(-1)
    month.value = 11
  }else if(month.value === 12){
    updateYear(1)
    month.value = 0
  }
  // const value = (month.value + num) % 12;
  // month.value = value >= 0 ? value : 12 + value;
}
function updateYear(num: number) {
  const value = year.value + num;
  year.value = Math.min(Math.max(value, 1923), 2123)
}
</script>

<style scoped>
.day-names {
  display: flex;
  flex-wrap: wrap;
}
.day-names li {
  width: calc(100% / 7);
  list-style-type: none;
  text-align: center;
}

.dates {
  display: flex;
  flex-wrap: wrap;
}
.dates li {
  width: calc(100% / 7);
  list-style-type: none;
  text-align: center;
}
.current{
  color: blue;
}
.selected{

  color: red;
}
</style>
