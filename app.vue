<template>
  <div class="flex flex-col items-center justify-center min-h-screen text-gray-700 bg-gray-100">
    <div class="absolute inset-x-0 top-0 z-20 flex justify-center overflow-hidden pointer-events-none">
      <!-- <div class="w-[108rem] flex-none flex justify-end">
        <picture
          ><source srcset="https://tailwindcss.com/_next/static/media/docs@30.8b9a76a2.avif" type="image/avif" />
          <img src="https://tailwindcss.com/_next/static/media/docs@30.8b9a76a2.avif" alt="" class="w-[71.75rem] flex-none max-w-none dark:hidden" decoding="async" /></picture
        ><picture
          ><source srcset="https://tailwindcss.com/_next/static/media/docs@tinypng.d9e4dcdc.png" type="image/avif" />
          <img src="https://tailwindcss.com/_next/static/media/docs@tinypng.d9e4dcdc.png" alt="" class="w-[90rem] flex-none max-w-none hidden dark:block" decoding="async"
        /></picture>
      </div> -->
    </div>
    <div class="max-w-xl p-5 text-center">
      <h1 class="text-4xl font-bold tracking-tight text-gray-900 sm:text-6xl">The universe in human years</h1>
      <p class="mt-6 text-lg leading-8 text-gray-600">Map cosmic timescales onto your lifespan. Enter your birth date, pick an event, and visualize when it would occur in your universe-age life.</p>

      <form @submit.prevent="calculateEvent">
        <div class="flex flex-col gap-4 my-4 text-left sm:flex-row">
          <div class="w-full">
            <label for="dob" class="block text-sm font-medium leading-6 text-gray-900">Your date of birth</label>
            <input
              id="dob"
              v-model="dob"
              type="date"
              :min="'1900-01-01'"
              :max="`${currentYear}-01-01`"
              required
              class="block w-full h-10 px-3 py-2 text-gray-900 border-0 rounded-md ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6"
            />
          </div>

          <div class="w-full">
            <label for="event" class="block text-sm font-medium leading-6 text-gray-900">Choose an event</label>
            <select
              id="event"
              v-model="selectedEvent"
              required
              class="block w-full h-10 px-3 py-2 text-gray-900 border-0 rounded-md ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6"
            >
              <option disabled value="">Please select an event</option>
              <option v-for="event in events" :key="event.name">{{ event.name }}</option>
            </select>
          </div>
        </div>

        <button
          class="rounded-md bg-indigo-600 px-3.5 py-2.5 text-sm font-semibold text-white shadow-sm hover:bg-indigo-500 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-600"
          type="submit"
        >
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

const today = new Date();
const currentYear = today.getFullYear();

const events = [
  { name: "Emergence of the first galaxies", year: -13800000000 },
  { name: "Formation of the Milky Way galaxy", year: -13400000000 },
  { name: "Formation of the Solar System and the Sun", year: -4600000000 },
  { name: "Formation of Earth", year: -4530000000 },
  { name: "Formation of the Moon", year: -4450000000 },
  { name: "Emergence of the first life on Earth", year: -3500000000 },
  { name: "Emergence of multicellular life", year: -600000000 },
  { name: "Appearance of dinosaurs", year: -230000000 },
  { name: "Appearance of the first mammals", year: -200000000 },
  { name: "Extinction of dinosaurs", year: -66000000 },
  { name: "Emergence of the Sahelanthropus species", year: -7000000 },
  { name: "Emergence of Homo Sapiens species", year: -300000 },
  { name: "Discovery of fire", year: -125000 },
  { name: "Agricultural revolution", year: -10000 },
  { name: "Invention of the wheel", year: -3500 },
  { name: "Construction of Pyramids", year: -2560 },
  { name: "Establishment of the Roman Empire", year: -27 },
  { name: "Birth of Jesus Christ", year: 0 },
  { name: "Fall of the Roman Empire", year: 476 },
  { name: "Newton's Principia published", year: 1687 },
  { name: "Invention of the Steam Engine", year: 1712 },
  { name: "Industrial revolution", year: 1760 },
  { name: "First public flight", year: 1903 },
  { name: "Moon landing", year: 1969 },
  // Future events
  // { name: "Sun becomes a Red Giant", year: 5000000000 },
  // { name: "Death of the Earth", year: 7500000000 },
  // { name: "Milky Way merges with Andromeda", year: 4500000000 },
];

function calculateEvent() {
  const dobValue = new Date(dob.value);

  const age = (today - dobValue) / (365.25 * 24 * 60 * 60 * 1000);

  const universeAgeInYears = 13.8 * Math.pow(10, 9);
  const eventYear = events.find((event) => event.name === selectedEvent.value).year;
  const eventYearsAgo = currentYear - eventYear;
  const equivalentAgeInSeconds = ((age * eventYearsAgo) / universeAgeInYears) * 31556952;

  const timeUnits = ["years", "months", "days", "hours", "minutes", "seconds"];
  const timeConversions = [31556952, 2629746, 86400, 3600, 60, 1];

  let primaryUnit, primaryValue, secondaryUnit, secondaryValue;

  for (let i = 0; i < timeConversions.length; i++) {
    if (Math.abs(equivalentAgeInSeconds) >= timeConversions[i]) {
      primaryUnit = timeUnits[i];
      primaryValue = Math.floor(Math.abs(equivalentAgeInSeconds) / timeConversions[i]);

      if (primaryUnit === "seconds" && primaryValue < 10) {
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

  if (eventYear > currentYear) {
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
