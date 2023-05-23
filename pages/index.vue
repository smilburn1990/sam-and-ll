<template>
  <section class="wedding">
    <div class="wedding__nav" :class="section.class">
      <ul>
        <li
          v-for="(item, i) in content.nav"
          :key="`nav_${i}`"
          :class="
            section.name.toLowerCase() === item.name.toLowerCase()
              ? 'selected'
              : ''
          "
          @click="updateSection(item, i)"
        >
          <h3>{{ item.name }}</h3>
        </li>
      </ul>
    </div>
    <div class="wedding__mobile-nav" :class="section.class">
      <fa
        :icon="['fal', showNav ? 'xmark' : 'bars']"
        @click="showNav = !showNav"
      />
      <ul v-if="showNav">
        <li
          v-for="(item, i) in content.nav"
          :key="`nav_${i}`"
          :class="
            section.name.toLowerCase() === item.name.toLowerCase()
              ? 'selected'
              : ''
          "
          @click="updateSection(item, i)"
        >
          <h3>{{ item.name }}</h3>
        </li>
      </ul>
    </div>
    <div
      v-if="section.name.toLowerCase() === 'home'"
      class="wedding__home animate__animated animate_fadeIn yellow"
    >
      <div class="wedding__home--text">
        <h1>{{ content.home.names }}</h1>
        <h1>{{ content.home.location }}</h1>
        <h1>{{ content.home.date }}</h1>
        <h3>{{ content.home.message }}</h3>
      </div>
      <div class="wedding__home--videos">
        <video autoplay loop muted class="one">
          <source
            src="https://res.cloudinary.com/and-dine/video/upload/v1684246286/gifs/part-1.mp4"
            type="video/mp4"
          />
        </video>
        <video autoplay loop muted class="two">
          <source
            src="https://res.cloudinary.com/and-dine/video/upload/v1684246287/gifs/part-2.mp4"
            type="video/mp4"
          />
        </video>
        <video autoplay loop muted class="three">
          <source
            src="https://res.cloudinary.com/and-dine/video/upload/v1684246287/gifs/part-3.mp4"
            type="video/mp4"
          />
        </video>
      </div>
    </div>
    <div
      v-if="section.name.toLowerCase() === 'schedule'"
      class="wedding__schedule animate__animated animate_fadeIn blue"
    >
      <h3>{{ content.schedule.location }}</h3>
      <div class="wedding__schedule--section">
        <h2>{{ content.schedule.ceremony.date }}</h2>
        <h2>{{ content.schedule.ceremony.name }}</h2>
        <h2>{{ content.schedule.ceremony.time }}</h2>
      </div>
      <div class="wedding__schedule--section">
        <h2>{{ content.schedule.reception.name }}</h2>
        <h2>{{ content.schedule.reception.time }}</h2>
      </div>
      <div class="wedding__schedule--section">
        <h2>{{ content.schedule.pizza.date }}</h2>
        <h2>{{ content.schedule.pizza.name }}</h2>
        <h2>{{ content.schedule.pizza.time }}</h2>
      </div>
      <h3>{{ content.schedule.outro }}</h3>
    </div>
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
          v-for="(marker, i) in markers"
          :key="`map_${i}`"
          :position="marker.position"
          :clickable="true"
          :draggable="true"
          @click="center = marker.position"
        >
        </gmap-marker>
      </gmap-map>
    </div>
    <div
      v-if="section.name.toLowerCase() === 'faq'"
      class="wedding__faq animate__animated animate_fadeIn yellow"
    >
      <div
        v-for="(faq, i) in content.faqs"
        :key="`faq-${i}`"
        class="wedding__schedule--section"
      >
        <h2>{{ faq.title }}</h2>
        <h3 v-for="(detail, i) in faq.details" :key="`detail-${i}`">
          {{ detail }}
        </h3>
        <a :href="faq.link" target="_blank">{{ faq.link }}</a>
      </div>
    </div>
    <div
      v-if="section.name.toLowerCase() === 'rsvp'"
      class="wedding__rsvp animate__animated animate_fadeIn blue"
    >
      <video v-if="showVideo" class="wedding__rsvp--video" autoplay>
        <source
          src="https://res.cloudinary.com/and-dine/video/upload/v1684310816/gifs/success.mp4"
          type="video/mp4"
        />
      </video>
      <form
        v-else
        class="wedding__rsvp--form"
        :action="content.rsvp.link"
        method="post"
        target="_blank"
      >
        <h3>{{ content.rsvp.email.title }}</h3>
        <input
          id="email"
          v-model="email"
          name="emailAddress"
          type="email"
          placeholder="Email"
        />
        <h3>{{ content.rsvp.name.title }}</h3>
        <input
          id="name"
          v-model="name"
          name="entry.1436407584"
          type="text"
          placeholder="Name"
        />

        <div class="radio">
          <h3>{{ content.rsvp.coming.title }}</h3>
          <div class="element">
            <input
              :id="content.rsvp.coming.yes"
              v-model="coming"
              type="radio"
              name="entry.1749319050"
              :value="content.rsvp.coming.yes"
            />
            <label :for="content.rsvp.coming.yes"
              >{{ content.rsvp.coming.yes }}
            </label>
          </div>
          <div class="element">
            <input
              :id="content.rsvp.coming.no"
              v-model="coming"
              type="radio"
              name="entry.1749319050"
              :value="content.rsvp.coming.no"
            />
            <label :for="content.rsvp.coming.no"
              >{{ content.rsvp.coming.no }}
            </label>
          </div>
        </div>

        <div class="checkbox">
          <h3>{{ content.rsvp.diet.title }}</h3>
          <div class="element">
            <input
              :id="content.rsvp.diet.vegetarian"
              type="checkbox"
              name="entry.972850836"
              :value="content.rsvp.diet.vegetarian"
            />
            <label :for="content.rsvp.diet.vegetarian">{{
              content.rsvp.diet.vegetarian
            }}</label>
          </div>
          <div class="element">
            <input
              :id="content.rsvp.diet.vegan"
              type="checkbox"
              name="entry.972850836"
              :value="content.rsvp.diet.vegan"
            />
            <label :for="content.rsvp.diet.vegan">{{
              content.rsvp.diet.vegan
            }}</label>
          </div>
          <div class="element">
            <input
              :id="content.rsvp.diet.peanut"
              type="checkbox"
              name="entry.972850836"
              :value="content.rsvp.diet.peanut"
            />
            <label :for="content.rsvp.diet.peanut">{{
              content.rsvp.diet.peanut
            }}</label>
          </div>
          <div class="element">
            <input
              :id="content.rsvp.diet.shellfish"
              type="checkbox"
              name="entry.972850836"
              :value="content.rsvp.diet.shellfish"
            />
            <label :for="content.rsvp.diet.shellfish">{{
              content.rsvp.diet.shellfish
            }}</label>
          </div>
          <div class="element">
            <input
              :id="content.rsvp.diet.other"
              name="entry.653095865"
              type="text"
              placeholder="Other"
            />
          </div>
        </div>
        <input
          v-if="formFilled"
          class="button yellow"
          type="submit"
          value="Send it!"
          @click="showVideo = true"
        />
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
    name: '',
    email: '',
    coming: false,
    showVideo: false,
    showNav: false,
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
  computed: {
    formFilled() {
      return this.name !== '' && this.email !== '' && this.coming
    },
  },
  methods: {
    updateSection(section, i) {
      this.showNav = false
      this.section = section
    },
  },
}
</script>

<style lang="scss">
$screen: 1600px;
$desktop: 1200px;
$tablet: 768px;

@font-face {
  font-family: 'SuisseIntl-Regular';
  src: url('../assets/fonts/SuisseIntl-Regular.otf') format('opentype');
}
@font-face {
  font-family: 'SuisseIntl-Light';
  src: url('../assets/fonts/SuisseIntl-Light.otf') format('opentype');
}
html {
  font-family: 'SuisseIntl-Regular', sans-serif !important;
  font-size: 100%;
  @media (max-width: $screen) {
    font-size: 75%;
  }
  @media (max-width: $tablet) {
    font-size: 50%;
  }
}
body {
  margin: 0;
  padding: 0;
}
h1 {
  font-size: 11rem;
  text-transform: uppercase;
  margin: 1rem 0;
  @media (max-width: $tablet) {
    font-size: 7rem;
  }
}
h2 {
  font-size: 4rem;
  margin: 0.1rem 0;
  padding: 0;
}
h3,
a,
.button {
  font-size: 1.5rem;
  margin: 0.2rem 0;
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
  word-wrap: break-word;
  overflow: hidden;
  &__nav {
    width: 100%;
    padding: 2rem 0;
    position: fixed;
    top: 0;
    z-index: 9999;
    display: flex;
    @media (max-width: $tablet) {
      display: none;
    }
    ul {
      padding: 0 5% 0 1%;
      margin: 0;
      display: flex;
      list-style: none;
      width: 60%;
      justify-content: space-around;
      li {
        padding: 0;
        margin: 0;
        cursor: pointer;
      }
    }
    .selected {
      text-decoration: underline;
    }
  }
  &__mobile-nav {
    display: none;
    position: fixed;
    top: 0;
    z-index: 9999;
    width: 96%;
    padding: 2rem 2%;
    @media (max-width: $tablet) {
      display: block;
    }
    svg {
      float: right;
      font-size: 2rem;
      width: 2rem;
    }
    ul {
      padding: 0 5% 0 1%;
      margin: 0;
      list-style: none;
      width: 60%;
      li {
        padding: 0;
        margin: 0;
        cursor: pointer;
      }
    }
  }
  &__home,
  &__schedule,
  &__location,
  &__faq,
  &__rsvp {
    width: 90%;
    padding: 7rem 5%;
    height: 200vh;
    position: relative;
    overflow: hidden;
    @media (max-width: $tablet) {
      width: 96%;
      padding: 7em 2%;
    }
  }
  &__home {
    &--text {
      position: fixed;
      top: 5rem;
      left: 5rem;
      width: calc(100% - 14rem);
      padding: 7rem 5%;
      @media (max-width: $tablet) {
        top: 5rem;
        left: 0;
        width: 100%;
        overflow: hidden;
        width: 96%;
        padding: 2em 2%;
      }
    }
    &--videos {
      height: 200vh;
      width: 100%;
      .one,
      .two,
      .three {
        position: absolute;
        width: 20rem;
        height: 20rem;
        object-fit: cover;
      }
      .one {
        top: 20%;
        left: 10%;
        @media (max-width: $tablet) {
          top: 4%;
          left: 15%;
        }
      }
      .two {
        left: 30%;
        top: 60%;
        @media (max-width: $tablet) {
          top: 20%;
          left: 55%;
        }
      }
      .three {
        left: 70%;
        top: 30%;
        @media (max-width: $tablet) {
          top: 28%;
          left: -15%;
        }
      }
    }
  }
  &__schedule,
  &__faq {
    &--section {
      margin: 4rem 0;
    }
  }
  &__rsvp {
    &--video {
      width: 25%;
      padding: 0 37.5%;
    }
    &--form {
      width: 100%;
      height: 100%;
      border: none;
      h3 {
        margin: 1rem 0;
      }
      input {
        border: none;
        padding: 1rem;
        margin: 0.5rem 0;
      }
      .button {
        padding: 1rem;
        margin: 1rem 0;
        cursor: pointer;
        &:hover {
          transform: scale(1.1);
        }
      }
    }
  }
}
</style>
