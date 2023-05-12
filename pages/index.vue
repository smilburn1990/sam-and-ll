<template>
  <section class="wedding">
    <div class="wedding__nav" :class="section.class">
      <ul>
        <li
          v-for="(item, i) in content.nav"
          :key="item"
          :class="
            section.name.toLowerCase() === item.name.toLowerCase()
              ? 'selected'
              : ''
          "
          @click="updateSection(item, i)"
        >
          <h3>{{ item.name }}</h3>
          <!-- <fa :icon="['fal', item.icon]" /> -->
        </li>
      </ul>
    </div>
    <div
      v-if="section.name.toLowerCase() === 'home'"
      class="wedding__home animate__animated animate_fadeIn yellow"
    ></div>
    <div
      v-if="section.name.toLowerCase() === 'schedule'"
      class="wedding__schedule animate__animated animate_fadeIn blue"
    ></div>
    <div
      v-if="section.name.toLowerCase() === 'location'"
      class="wedding__location animate__animated animate_fadeIn red"
    >
      <gmap-map
        :center="center"
        :zoom="16"
        :options="mapStyle"
        style="height: 80vh; width: 60%; margin: 0 20%"
      >
        <gmap-marker
          v-for="(m, index) in markers"
          :key="index"
          :position="m.position"
          :clickable="true"
          :draggable="true"
          @click="center = m.position"
        >
        </gmap-marker>
      </gmap-map>
    </div>
    <div
      v-if="section.name.toLowerCase() === 'faq'"
      class="wedding__faq animate__animated animate_fadeIn yellow"
    ></div>
    <div
      v-if="section.name.toLowerCase() === 'rsvp'"
      class="wedding__rsvp animate__animated animate_fadeIn blue"
    >
      <form class="wedding__rsvp--form" :action="content.rsvp.link">
        <label for="name">{{ content.rsvp.name.title }}</label>
        <input id="name" name="entry.1436407584" type="text" />

        <p>{{ content.rsvp.coming.title }}</p>
        <div class="input-wrap">
          <input
            id="e1"
            type="radio"
            name="entry.1749319050"
            :value="content.rsvp.coming.yes"
          />
          <label for="e1">{{ content.rsvp.coming.yes }}</label>
        </div>
        <div class="input-wrap">
          <input
            id="e2"
            type="radio"
            name="entry.1749319050"
            :value="content.rsvp.coming.no"
          />
          <label for="e2">{{ content.rsvp.coming.no }}</label>
        </div>

        <p>{{ content.rsvp.diet.title }}</p>
        <div class="input-wrap">
          <input
            id="e1"
            type="checkbox"
            name="entry.1805014275"
            :value="content.rsvp.diet.vegetarian"
          />
          <label for="e1">{{ content.rsvp.diet.vegetarian }}</label>
        </div>
        <div class="input-wrap">
          <input
            id="e2"
            type="checkbox"
            name="entry.1805014275"
            :value="content.rsvp.diet.vegan"
          />
          <label for="e2">{{ content.rsvp.diet.vegan }}</label>
        </div>
        <div class="input-wrap">
          <input
            id="e3"
            type="checkbox"
            name="entry.1805014275"
            :value="content.rsvp.diet.peanut"
          />
          <label for="e3">{{ content.rsvp.diet.peanut }}</label>
        </div>
        <div class="input-wrap">
          <input
            id="e4"
            type="checkbox"
            name="entry.1805014275"
            :value="content.rsvp.diet.shellfish"
          />
          <label for="e4">{{ content.rsvp.diet.shellfish }}</label>
        </div>
        <div class="input-wrap">
          <input
            id="e5"
            type="checkbox"
            name="entry.1805014275"
            :value="content.rsvp.diet.other"
          />
          <label for="e5">{{ content.rsvp.diet.other }}</label>
        </div>
        <label for="other">{{ content.rsvp.diet.other }}</label>
        <!-- <input
          id="other"
          name="entry.1805014275.other_option_response"
          type="text"
        /> -->
        <input class="button" type="submit" value="Submit" />
      </form>
    </div>
  </section>
</template>

<script>
export default {
  name: 'Wedding',
  async asyncData({ $content }) {
    const data = await $content('index').fetch()
    return { content: data }
  },
  data: () => ({
    section: { name: 'Home', icon: 'house', class: 'yellow' },
    center: { lat: -36.711569, lng: 175.610356 },
    markers: [
      {
        position: { lat: -36.711569, lng: 175.610356 },
      },
    ],
    mapStyle: {
      styles: [
        {
          featureType: 'all',
          elementType: 'labels.text.fill',
          stylers: [
            {
              saturation: 36,
            },
            {
              color: '#000000',
            },
            {
              lightness: 40,
            },
          ],
        },
        {
          featureType: 'all',
          elementType: 'labels.text.stroke',
          stylers: [
            {
              visibility: 'on',
            },
            {
              color: '#000000',
            },
            {
              lightness: 16,
            },
          ],
        },
        {
          featureType: 'all',
          elementType: 'labels.icon',
          stylers: [
            {
              visibility: 'off',
            },
          ],
        },
        {
          featureType: 'administrative',
          elementType: 'geometry.fill',
          stylers: [
            {
              color: '#000000',
            },
            {
              lightness: 20,
            },
          ],
        },
        {
          featureType: 'administrative',
          elementType: 'geometry.stroke',
          stylers: [
            {
              color: '#000000',
            },
            {
              lightness: 17,
            },
            {
              weight: 1.2,
            },
          ],
        },
        {
          featureType: 'landscape',
          elementType: 'geometry',
          stylers: [
            {
              color: '#000000',
            },
            {
              lightness: 20,
            },
          ],
        },
        {
          featureType: 'poi',
          elementType: 'geometry',
          stylers: [
            {
              color: '#000000',
            },
            {
              lightness: 21,
            },
          ],
        },
        {
          featureType: 'road.highway',
          elementType: 'geometry.fill',
          stylers: [
            {
              color: '#000000',
            },
            {
              lightness: 17,
            },
          ],
        },
        {
          featureType: 'road.highway',
          elementType: 'geometry.stroke',
          stylers: [
            {
              color: '#000000',
            },
            {
              lightness: 29,
            },
            {
              weight: 0.2,
            },
          ],
        },
        {
          featureType: 'road.arterial',
          elementType: 'geometry',
          stylers: [
            {
              color: '#000000',
            },
            {
              lightness: 18,
            },
          ],
        },
        {
          featureType: 'road.local',
          elementType: 'geometry',
          stylers: [
            {
              color: '#000000',
            },
            {
              lightness: 16,
            },
          ],
        },
        {
          featureType: 'transit',
          elementType: 'geometry',
          stylers: [
            {
              color: '#000000',
            },
            {
              lightness: 19,
            },
          ],
        },
        {
          featureType: 'water',
          elementType: 'geometry',
          stylers: [
            {
              color: '#000000',
            },
            {
              lightness: 17,
            },
          ],
        },
      ],
    },
  }),
  methods: {
    updateSection(section, i) {
      this.section = section
    },
  },
}
</script>

<style lang="scss">
$desktop: 1200px;
$tablet: 768px;

@font-face {
  font-family: 'SuisseIntl-Regular';
  src: url('../assets/fonts/SuisseIntl-Regular.otf') format('opentype');
}

body {
  margin: 0;
  padding: 0;
}
h1 {
  font-size: 60px;
}
h2 {
  font-size: 40px;
}
h3 {
  font-size: 30px;
}
h4 {
  font-size: 20px;
}
.blue {
  background-color: #62d6df;
}
.yellow {
  background-color: #fcda23;
}
.red {
  background-color: #ab2323;
}
.wedding {
  width: 100%;
  font-family: 'SuisseIntl-Regular', sans-serif;
  &__nav {
    width: 100%;
    padding: 20px 0;
    position: fixed;
    top: 0;
    z-index: 9999;
    ul {
      padding: 0 5%;
      display: flex;
      list-style: none;
      width: 60%;
      justify-content: space-around;
      li {
        cursor: pointer;
      }
    }
    .selected {
      text-decoration: underline;
    }
  }
  &__home,
  &__schedule,
  &__location,
  &__faq,
  &__rsvp {
    width: 90%;
    padding: 200px 5% 20px;
    height: 100vh;
  }
}
</style>
