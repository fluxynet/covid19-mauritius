<template>
  <div class="py-10 container mx-auto">
    <h2 class="text-2xl text-center font-black">FAQ</h2>
    <ul
      class="faq px-8 sm:px-0 grid py-10 grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-10"
    >
      <li>
        <label>What is the difference between Active Cases and Total?</label>
        <p>
          Active cases are the last reported number of people who have the
          infection. It does not include deceased people or people who have
          recovered.
        </p>
      </li>
      <li>
        <label>What is the source ?</label>
        <p>
          Local newspapers and radio stations.
        </p>
      </li>
      <li>
        <label>When was this page last updated?</label>
        <template v-if="getUpdated">
          <p>Last updated: {{ getUpdated.ago }}</p>
          <p>{{ getUpdated.value }}</p>
        </template>
      </li>
      <li>
        <label>Should I trust this page?</label>
        <p>
          All data about mauritius are updated after cross checking with official press releases on radio or TV. This page is not updated based on rumors.
          Kindly check websites of local news papers such as
          <a href="https://www.lexpress.mu">lexpress.mu</a>
          for trusted sources.
        </p>
      </li>
      <li>
        <label>Is the data about Mauritius updated automatically?</label>
        <p>
          No. The data about Mauritius' number of cases is updated whenever practically possible after the Press Conferences of the Goverment. 
          The data comes from a
          <a
            href="https://docs.google.com/spreadsheets/d/1DhrU8lCWYS_VKKQ3QOYbLzrJgHioQGQp5qPnpSNcUU8/edit?usp=sharing"
            >Google Sheet</a
          >. Anyone can submit a PR to add new charts.
          <a href="https://github.com/MrSunshyne/covid19-mauritius">here</a>.
        </p>
      </li>
      <li>
        <label>Is the data about other countries updated automatically?</label>
        <p>Yes. The data for other countries is taken from this data sources: <a href="https://github.com/pomber/covid19/">Timeseries JSON on Github</a></p>

      </li>
      <li>
        <label>Why did you build this?</label>
        <p>
          To have a simple page where everyone can check the latest statistics.
        </p>
      </li>
      <li>
        <label
          >Are you a journalist or from the Ministry of Health and would you
          like to have access to update this page?</label
        >
        <p>
          Get in touch with me asap: san<span class="hidden"></span>dee<span
            class="hidden"
          ></span
          >p@ra<span class="hidden"></span>mgo<span class="hidden"></span
          >lam.com
        </p>
      </li>
    </ul>
  </div>
</template>

<script>
import { mapGetters } from "vuex";
export default {
  computed: {
    ...mapGetters(["getUpdated"])
  }
};
</script>

<style>
label {
  @apply font-bold my-4;
}

a {
  @apply font-bold;
}
</style>
