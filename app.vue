<template>
  <div class="flex flex-col items-center justify-center min-h-screen text-gray-700 bg-gray-50">
    <div class="max-w-lg p-5">
      <h1 class="mb-4 text-3xl font-light">Universe Age Calculator</h1>
      <p class="mb-4 text-lg">Enter your age and choose an event to find out when it happened in terms of your lifespan if it were equivalent to the age of the Universe.</p>

      <div class="w-64">
        <label for="age" class="block mb-1 text-sm font-medium text-gray-700">Your age in years</label>
        <input
          id="age"
          v-model="age"
          type="number"
          min="0"
          placeholder="Enter your age"
          class="w-full px-3 py-2 leading-tight text-gray-700 bg-white border border-gray-300 rounded appearance-none focus:outline-none focus:ring-2 focus:ring-blue-500"
        />
      </div>

      <div class="w-64 mt-4">
        <label for="event" class="block mb-1 text-sm font-medium text-gray-700">Choose an event</label>
        <select
          id="event"
          v-model="selectedEvent"
          class="w-full px-3 py-2 leading-tight text-gray-700 bg-white border border-gray-300 rounded appearance-none focus:outline-none focus:ring-2 focus:ring-blue-500"
        >
          <option disabled value="">Please select an event</option>
          <option v-for="event in events" :key="event.name">{{ event.name }}</option>
        </select>
      </div>

      <button class="px-4 py-2 mt-4 font-bold text-white bg-blue-500 rounded hover:bg-blue-700 focus:outline-none focus:ring-2 focus:ring-blue-700" @click="calculateEvent">
        Calculate
      </button>

      <p class="mt-4 text-lg font-medium" v-if="message">{{ message }}</p>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";

const age = ref("");
const selectedEvent = ref("");
const message = ref("");

const currentYear = new Date().getFullYear();

const events = [
  { name: "Creation of the first galaxies", yearsAgo: 13000000000 },
  { name: "Formation of the Milky Way", yearsAgo: 10000000000 },
  { name: "Formation of the Earth", yearsAgo: 4500000000 },
  { name: "Emergence of first life on Earth", yearsAgo: 3800000000 },
  { name: "Appearance of the dinosaurs", yearsAgo: 230000000 },
  { name: "Extinction of the dinosaurs", yearsAgo: 66000000 },
  { name: "Emergence of Homo Sapiens", yearsAgo: 300000 },
  { name: "Discovery of fire", yearsAgo: 125000 },
  { name: "Founding of the Roman Empire", yearsAgo: currentYear - 27 },
  { name: "Year zero (Birth of Jesus Christ)", yearsAgo: currentYear },
  { name: "Fall of the Roman Empire", yearsAgo: currentYear - 476 },
];

function calculateEvent() {
  const universeAgeInYears = 13.8 * Math.pow(10, 9);
  const eventYearsAgo = events.find((event) => event.name === selectedEvent.value).yearsAgo;
  const equivalentAgeInSeconds = ((age.value * eventYearsAgo) / universeAgeInYears) * 31556952;

  const timeUnits = ["years", "months", "days", "hours", "minutes", "seconds"];
  const timeConversions = [31556952, 2629746, 86400, 3600, 60, 1];

  let primaryUnit, primaryValue, secondaryUnit, secondaryValue;

  for (let i = 0; i < timeConversions.length; i++) {
    if (equivalentAgeInSeconds >= timeConversions[i]) {
      primaryUnit = timeUnits[i];
      primaryValue = Math.floor(equivalentAgeInSeconds / timeConversions[i]);

      if (i + 1 < timeConversions.length) {
        secondaryUnit = timeUnits[i + 1];
        secondaryValue = Math.floor((equivalentAgeInSeconds % timeConversions[i]) / timeConversions[i + 1]);
      } else {
        break;
      }

      break;
    }
  }

  if (secondaryValue) {
    message.value = `If your age were equivalent to the age of the Universe, the '${selectedEvent.value}' event would have occurred ${primaryValue} ${primaryUnit} and ${secondaryValue} ${secondaryUnit} ago.`;
  } else {
    message.value = `If your age were equivalent to the age of the Universe, the '${selectedEvent.value}' event would have occurred ${primaryValue} ${primaryUnit} ago.`;
  }
}
</script>
