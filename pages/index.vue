<template>
  <div>
    <autocomplete
      class="mx-auto my-5 w-11/12 md:w-3/4"
      :data="locations"
      v-model="location"
      @chosen="handleChosen"
    />
    <main
      v-if="city"
      class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-10 justify-items-center mx-3 md:mx-10 mt-24"
    >
      <card
        v-for="job in city.jobs"
        :key="job.id"
        :title="job.title"
        :city="city.name"
        :company="job.company"
        :tags="job.tags"
        :remote="job.remotes[0] ? job.remotes[0].type : false"
      />
    </main>
  </div>
</template>

<script>
import gql from "graphql-tag";
import Autocomplete from "../components/autocomplete";
import Card from "../components/card";
export default {
  components: {
    Autocomplete,
    Card,
  },
  data() {
    return {
      locations: [],
      location: "",
      choosenLocation: "",
    };
  },
  apollo: {
    locations: {
      query: gql`
        query Locations($location: String!) {
          locations(input: { value: $location }) {
            name
            slug
          }
        }
      `,
      variables() {
        return {
          location: this.location,
        };
      },
    },
    city: {
      query: gql`
        query City($location: String!) {
          city(input: { slug: $location }) {
            name
            jobs {
              id
              title
              tags {
                id
                name
              }
              company {
                name
                logoUrl
              }
              remotes {
                type
              }
            }
          }
        }
      `,
      variables() {
        return {
          location: this.choosenLocation,
        };
      },
      skip: true,
    },
  },
  methods: {
    handleChosen(city) {
      this.choosenLocation = city.slug;
      this.$apollo.queries.city.skip = false;
    },
  },
};
</script>

<style>
</style>
