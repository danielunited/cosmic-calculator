<template>
  <div class="flex flex-col items-center justify-center min-h-screen text-gray-700 bg-gray-50">
    <div class="max-w-lg p-5">
      <h1 class="mb-4 text-3xl font-light">Universe Age Calculator</h1>
      <p class="mb-4 text-lg">Enter your date of birth and choose an event to find out when it happened in terms of your lifespan if it were equivalent to the age of the Universe.</p>

      <form @submit.prevent="calculateEvent">
        <div class="w-64">
          <label for="dob" class="block mb-1 text-sm font-medium text-gray-700">Your date of birth</label>
          <input
            id="dob"
            v-model="dob"
            type="date"
            :min="'1900-01-01'"
            :max="`${currentYear}-12-31`"
            required
            class="w-full px-3 py-2 leading-tight text-gray-700 bg-white border border-gray-300 rounded appearance-none focus:outline-none focus:ring-2 focus:ring-blue-500"
          />
        </div>

        <div class="w-64 mt-4">
          <label for="event" class="block mb-1 text-sm font-medium text-gray-700">Choose an event</label>
          <select
            id="event"
            v-model="selectedEvent"
            required
            class="w-full px-3 py-2 leading-tight text-gray-700 bg-white border border-gray-300 rounded appearance-none focus:outline-none focus:ring-2 focus:ring-blue-500"
          >
            <option disabled value="">Please select an event</option>
            <option v-for="event in events" :key="event.name">{{ event.name }}</option>
          </select>
        </div>

        <button class="px-4 py-2 mt-4 font-bold text-white bg-blue-500 rounded hover:bg-blue-700 focus:outline-none focus:ring-2 focus:ring-blue-700" type="submit">
          Calculate
        </button>

        <div class="mt-4 text-lg" v-if="message" v-html="message"></div>
      </form>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";

const dob = ref("");
const selectedEvent = ref("");
const message = ref("");

const events = [
  { name: "Creation of the first galaxies", year: -13000000000 },
  { name: "Formation of the Milky Way", year: -10000000000 },
  { name: "Formation of the Earth", year: -4500000000 },
  { name: "Emergence of first life on Earth", year: -3800000000 },
  { name: "Appearance of the dinosaurs", year: -230000000 },
  { name: "Extinction of the dinosaurs", year: -66000000 },
  { name: "Emergence of Sahelanthropus", year: -7000000 },
  { name: "Emergence of Homo Sapiens", year: -300000 },
  { name: "Discovery of fire", year: -125000 },
  { name: "Agricultural revolution", year: -12000 },
  { name: "Invention of the wheel", year: -5200 },
  { name: "Founding of the Roman Empire", year: -27 },
  { name: "Birth of Jesus Christ", year: 0 },
  { name: "Fall of the Roman Empire", year: 476 },
  { name: "Industrial revolution", year: 1760 },
  { name: "Moon landing", year: 1969 },
  { name: "Sun becomes a Red Giant", year: 5000000000 },
  { name: "Death of the Earth", year: 7500000000 },
  { name: "Milky Way merges with Andromeda", year: 4500000000 },
];

function calculateEvent() {
  const dobValue = new Date(dob.value);
  const today = new Date();
  let age = today.getFullYear() - dobValue.getFullYear();
  const monthDifference = today.getMonth() - dobValue.getMonth();

  if (monthDifference < 0 || (monthDifference === 0 && today.getDate() < dobValue.getDate())) {
    age--;
  }

  const universeAgeInYears = 13.8 * Math.pow(10, 9);
  const eventYear = events.find((event) => event.name === selectedEvent.value).year;
  const eventYearsAgo = today.getFullYear() - eventYear;
  const equivalentAgeInSeconds = ((age * eventYearsAgo) / universeAgeInYears) * 31556952;

  const timeUnits = ["years", "months", "days", "hours", "minutes", "seconds"];
  const timeConversions = [31556952, 2629746, 86400, 3600, 60, 1];

  let primaryUnit, primaryValue, secondaryUnit, secondaryValue;

  for (let i = 0; i < timeConversions.length; i++) {
    if (Math.abs(equivalentAgeInSeconds) >= timeConversions[i]) {
      primaryUnit = timeUnits[i];
      primaryValue = Math.floor(Math.abs(equivalentAgeInSeconds) / timeConversions[i]);

      if (primaryUnit === "seconds" && primaryValue < 30) {
        primaryValue = (Math.abs(equivalentAgeInSeconds) / timeConversions[i]).toFixed(2);
      }

      if (i + 1 < timeConversions.length && Math.abs(equivalentAgeInSeconds) % timeConversions[i] !== 0) {
        secondaryUnit = timeUnits[i + 1];
        secondaryValue = Math.floor((Math.abs(equivalentAgeInSeconds) % timeConversions[i]) / timeConversions[i + 1]);
      } else {
        break;
      }

      break;
    }
  }

  if (eventYear > 0) {
    if (secondaryValue) {
      message.value = `If your age were equivalent to the age of the Universe, the ${
        selectedEvent.value.charAt(0).toLowerCase() + selectedEvent.value.slice(1)
      } would occur <br/> <span class="font-semibold text-black">in ${primaryValue} ${primaryUnit} and ${secondaryValue} ${secondaryUnit}.</span>`;
    } else {
      message.value = `If your age were equivalent to the age of the Universe, the ${
        selectedEvent.value.charAt(0).toLowerCase() + selectedEvent.value.slice(1)
      } would occur <br/> <span class="font-semibold text-black">in ${primaryValue} ${primaryUnit}.</span>`;
    }
  } else {
    if (secondaryValue) {
      message.value = `If your age were equivalent to the age of the Universe, the ${
        selectedEvent.value.charAt(0).toLowerCase() + selectedEvent.value.slice(1)
      } would have occurred <br/> <span class="font-semibold text-black">${primaryValue} ${primaryUnit} and ${secondaryValue} ${secondaryUnit} ago.</span>`;
    } else {
      message.value = `If your age were equivalent to the age of the Universe, the ${
        selectedEvent.value.charAt(0).toLowerCase() + selectedEvent.value.slice(1)
      } would have occurred <br/> <span class="font-semibold text-black">${primaryValue} ${primaryUnit} ago.</span>`;
    }
  }
}
</script>
