<template>
  <div class="flex flex-col items-center justify-center min-h-screen bg-gray-50 text-gray-700">
    <h1 class="text-4xl font-bold mb-4">Universe Age Calculator</h1>
    <p class="mb-4 text-lg">Enter your age and choose an event to find out when it happened in terms of your lifespan if it were equivalent to the age of the Universe.</p>

    <input
      class="bg-white shadow-md appearance-none border border-gray-200 rounded w-64 py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:ring-2 focus:ring-blue-500 mb-4"
      v-model="age"
      type="number"
      min="0"
      placeholder="Your age in years..."
    />

    <select class="bg-white shadow-md border border-gray-200 rounded w-64 py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:ring-2 focus:ring-blue-500 mb-4" v-model="selectedEvent">
      <option disabled value="">Please select an event</option>
      <option v-for="event in events" :key="event.name">{{ event.name }}</option>
    </select>

    <button class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded focus:outline-none focus:ring-2 focus:ring-blue-700" @click="calculateEvent">Calculate</button>

    <p class="mt-4 text-lg font-medium" v-if="message">{{ message }}</p>
  </div>
</template>

<script setup>
import { ref } from "vue";

const age = ref("");
const selectedEvent = ref("");
const message = ref("");

const events = [
  { name: "Formation of the Earth", yearsAgo: 4500000000 },
  { name: "Emergence of the first humans", yearsAgo: 2000000 },
  { name: "The extinction of the dinosaurs", yearsAgo: 66000000 },
  { name: "Formation of the Milky Way", yearsAgo: 10000000000 },
  { name: "Moon Landing", yearsAgo: 54 },
  { name: "Beginning of the Industrial Revolution", yearsAgo: 260 },
];

function calculateEvent() {
  const eventYearsAgo = events.find((event) => event.name === selectedEvent.value).yearsAgo;
  const universeAgeInBillionYears = 13.8; // Approximate age of the Universe
  const universeAgeInYears = universeAgeInBillionYears * Math.pow(10, 9); // Convert to years
  const eachYearInUserLifeRepresents = universeAgeInYears / age.value; // Each year of user's life in cosmic years
  const eventCosmicYearsAgo = eventYearsAgo / eachYearInUserLifeRepresents; // Event in cosmic years
  const eventUserLifeYearsAgo = eventCosmicYearsAgo * age.value; // Convert back to user's lifespan
  const eventUserLifeMinutesAgo = eventUserLifeYearsAgo * 525600; // Convert to minutes

  message.value = `If your age were equivalent to the age of the Universe, the '${selectedEvent.value}' event would have occurred about ${eventUserLifeMinutesAgo.toFixed(2)} minutes ago.`;
}
</script>
