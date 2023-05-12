<template>
  <section class="wedding">
    <div class="wedding__nav">
      <ul>
        <li
          v-for="(item, i) in content.nav"
          :key="item"
          :class="section === item.name.toLowerCase() ? 'selected' : ''"
          @click="updateSection(item.name, i)"
        >
          {{ item.name }}
          <fa :icon="['fal', item.icon]" />
        </li>
      </ul>
    </div>
    <div
      v-if="section === 'home'"
      class="wedding__home animate__animated"
      :class="
        lastIndex < content.nav.indexOf('Home')
          ? 'animate__slideInLeft'
          : 'animate__slideInRight'
      "
    ></div>
    <div
      v-if="section === 'schedule'"
      class="wedding__schedule animate__animated"
      :class="
        lastIndex < content.nav.indexOf('Home')
          ? 'animate__slideInLeft'
          : 'animate__slideInRight'
      "
    ></div>
    <div
      v-if="section === 'location'"
      class="wedding__location animate__animated"
      :class="
        lastIndex < content.nav.indexOf('Home')
          ? 'animate__slideInLeft'
          : 'animate__slideInRight'
      "
    >
      <iframe
        :src="content.location.link"
        width="600"
        height="450"
        style="border: 0"
        allowfullscreen=""
        loading="lazy"
        referrerpolicy="no-referrer-when-downgrade"
      ></iframe>
    </div>
    <div
      v-if="section === 'faq'"
      class="wedding__faq animate__animated"
      :class="
        lastIndex < content.nav.indexOf('Home')
          ? 'animate__slideInLeft'
          : 'animate__slideInRight'
      "
    ></div>
    <div
      v-if="section === 'rsvp'"
      class="wedding__rsvp animate__animated"
      :class="
        lastIndex < content.nav.indexOf('Home')
          ? 'animate__slideInLeft'
          : 'animate__slideInRight'
      "
    ></div>
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
    section: 'home',
    lastIndex: 0,
  }),
  methods: {
    updateSection(section, i) {
      this.section = section.toLowerCase()
      this.lastIndex = i
    },
  },
}
</script>

<style lang="scss">
$desktop: 1200px;
$tablet: 768px;
@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@300;600;900&display=swap');

.wedding {
  width: 100%;
  font-family: 'Roboto', sans-serif;
  &__nav {
    width: 100%;
    ul {
      display: flex;
      list-style: none;
      justify-content: space-between;
      li {
        cursor: pointer;
      }
    }
    .selected {
    }
  }
  &__home,
  &__schedule,
  &__location,
  &__faq,
  &__rsvp {
    width: 90%;
    padding: 0 5%;
    height: 100vh;
  }
}
</style>
