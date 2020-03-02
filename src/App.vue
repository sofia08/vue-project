<template>
  <div id='app' class='container'>
    <div class='row my-2'>
      <h1 class="col-10">Next to Go</h1>
      <select class='dropdfown-menu col-2' v-model='filter' v-on:change="() => this.fetchRaces()">
        <option class='dropdown-item' value='4a2788f8-e825-4d36-9894-efd4baf1cfae'>Horse</option>
        <option class='dropdown-item' value='161d9be2-e909-4326-8c2c-35ed71fb460b'>Harness</option>
        <option class='dropdown-item' value='9daef0d7-bf3c-4f50-921d-8e818c60fe61'>Greyhound</option>
      </select>
    </div>
    <div class='row'>
    <Races v-model='races' v-on:change='this.updateRaces'/>
    </div>
  </div>
</template>

<script>
import Races from './components/Races.vue'

export default {
  name: 'App',
  components: {
    Races
  },
  created() {
    // Once the component has been mounted, fetch the races
    this.fetchRaces()
  },
  data() {
    return {
      filter: '4a2788f8-e825-4d36-9894-efd4baf1cfae', // Default the filter to horses 
      races: []
    }
  },
  methods: {
    updateRaces() {

    },
    fetchRaces(count) {
      // Default the count to 10 if it is undefined
      if (count === undefined) {
        count = 30
      }

      // Fetch the data using the API
      fetch(`https://api.neds.com.au/rest/v1/racing/?method=nextraces&count=${count}`)
        .then((response) => response.json())
        .then((response) => {
          const raceList = response.data.race_summaries
          const ids = response.data.next_to_go_ids

          // Fetch more races if we don't get at least 5 races for the filter type
          if (this.filterRaces(ids, raceList) !== 5 && count <= 200) {
            this.fetchRaces(count + 10)
          }
        })
        .catch((e) => console.error('Could not fetch races', e))
    },

    filterRaces(keys, raceList) {
      this.races = []
      
      for (const key of keys) {
        let race = raceList[key]

        // Stop if we have found 5 races to display
        if (this.races.length == 5) {
          break;
        }

        if (race.category_id == this.filter){
          this.races.push(race)
        }
      }

      return this.races.length;
    }

  }
}
</script>

<style>
select {
  height: 50px;
  vertical-align: middle;
}
</style>
