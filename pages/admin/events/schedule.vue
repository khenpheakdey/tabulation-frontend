<template>
  <!-- <div class="h-screen p-4 bg-gray-light rounded-xl xl:p-6 xl:rounded-xl">
    <no-ssr>
      <v-date-picker
        mode="range"
        v-model="selectedDate"
        :min-date="date"
        show-caps
      />
    </no-ssr>
  </div> -->
  <div class="h-screen p-4 bg-gray-light rounded-xl xl:p-6 xl:rounded-xl">
    <div class="text-center section">
      <h2 class="h2 mb-6 text-primary">Event Calendars</h2>
      <!-- <p class="text-lg font-medium text-gray-600 mb-6">
        Roll your own calendars using scoped slots
      </p> -->

      <v-calendar
        class="custom-calendar max-w-full"
        :masks="masks"
        :attributes="attributes"
        disable-page-swipe
        is-expanded
      >
        <template v-slot:day-content="{ day, attributes }">
          <div class="flex flex-col h-full z-10 overflow-hidden">
            <span class="day-label text-sm text-gray-900">{{ day.day }}</span>
            <div class="flex-grow overflow-y-auto overflow-x-auto">
              <p
                v-for="attr in attributes"
                :key="attr.key"
                class="text-xs leading-tight rounded-sm p-1 mt-0 mb-1"
                :class="attr.customData.class"
              >
                <nuxt-link :to="`admin/events/${attr.key}`">{{
                  attr.customData.title
                }}</nuxt-link>
              </p>
            </div>
          </div>
        </template>
      </v-calendar>
    </div>
  </div>
</template>

<script>
import * as _ from "lodash";
// export default {
//   data() {
//     return {
//       date: new Date(),
//       selectedDate: {
//         start: new Date(2018, 10, 9),
//         end: new Date(2018, 10, 10),
//       },
//     };
//   },
// };
export default {
  async asyncData({ $axios, $config }) {
    const month = new Date().getMonth();
    const year = new Date().getFullYear();
    const events = await $axios.$get(`api/event/all`);
    const attributes = _.map(events.data, function (event, idx) {
      return {
        key: event._id,
        customData: {
          title: event.title,
          class: "bg-primary text-white",
        },
        dates: new Date(year, month, 1 + idx),
      };
    });
    return { attributes };
  },
  data() {
    const month = new Date().getMonth();
    const year = new Date().getFullYear();
    return {
      masks: {
        weekdays: "WWW",
      },
      // attributes: [
      // {
      //   key: 1,
      //   customData: {
      //     title: "Lunch with mom.",
      //     class: "bg-red-600 text-white",
      //   },
      //   dates: new Date(year, month, 1),
      // },
      // {
      //   key: 2,
      //   customData: {
      //     title: "Take Noah to basketball practice",
      //     class: "bg-blue-500 text-white",
      //   },
      //   dates: new Date(year, month, 2),
      // },
      // {
      //   key: 3,
      //   customData: {
      //     title: "Noah's basketball game.",
      //     class: "bg-blue-500 text-white",
      //   },
      //   dates: new Date(year, month, 5),
      // },
      // {
      //   key: 4,
      //   customData: {
      //     title: "Take car to the shop",
      //     class: "bg-indigo-500 text-white",
      //   },
      //   dates: new Date(),
      // },
      // {
      //   key: 4,
      //   customData: {
      //     title: "Meeting with new client.",
      //     class: "bg-primary text-white",
      //   },
      //   dates: new Date(year, month, 7),
      // },
      // {
      //   key: 5,
      //   customData: {
      //     title: "Mia's gymnastics practice.",
      //     class: "bg-pink-500 text-white",
      //   },
      //   dates: new Date(year, month, 11),
      // },
      // {
      //   key: 6,
      //   customData: {
      //     title: "Cookout with friends.",
      //     class: "bg-yellow text-white",
      //   },
      //   dates: { months: 5, ordinalWeekdays: { 2: 1 } },
      // },
      // {
      //   key: 7,
      //   customData: {
      //     title: "Mia's gymnastics recital.",
      //     class: "bg-pink-500 text-white",
      //   },
      //   dates: new Date(year, month, 22),
      // },
      // {
      //   key: 8,
      //   customData: {
      //     title: "Visit great grandma.",
      //     class: "bg-red-600 text-white",
      //   },
      //   dates: new Date(year, month, 25),
      // },
      // ],
    };
  },
};
</script>



<style lang="postcss" scoped>
::-webkit-scrollbar {
  width: 0px;
}
::-webkit-scrollbar-track {
  display: none;
}
/deep/ .custom-calendar.vc-container {
  --day-border: 1px solid #b8c2cc;
  --day-border-highlight: 1px solid #b8c2cc;
  --day-width: 90px;
  --day-height: 90px;
  --weekday-bg: #f8fafc;
  --weekday-border: 1px solid #eaeaea;
  border-radius: 20;
  width: 100%;
  & .vc-header {
    background-color: #f1f5f8;
    padding: 10px 0;
  }
  & .vc-weeks {
    padding: 0;
  }
  & .vc-weekday {
    background-color: var(--weekday-bg);
    border-bottom: var(--weekday-border);
    border-top: var(--weekday-border);
    padding: 5px 0;
  }
  & .vc-day {
    padding: 0 5px 3px 5px;
    text-align: left;
    height: var(--day-height);
    min-width: var(--day-width);
    background-color: white;
    &.weekday-1,
    &.weekday-7 {
      background-color: #eff8ff;
    }
    &:not(.on-bottom) {
      border-bottom: var(--day-border);
      &.weekday-1 {
        border-bottom: var(--day-border-highlight);
      }
    }
    &:not(.on-right) {
      border-right: var(--day-border);
    }
  }
  & .vc-day-dots {
    margin-bottom: 5px;
  }
}
</style>
