<template lang="html">
	<div id="sightingsGrid">
		<sighting v-for="sighting in sightings" :sighting="sighting" />
	</div>
</template>

<script>
import { eventBus } from '@/main.js'
import Sighting from './Sighting';
import SightingService from '@/services/SightingService.js'


export default {
	name: 'sightings-grid',
	components: {
		'sighting': Sighting
	},
	props: ['sightings'],

mounted(){
    SightingService.getSightings()
    .then(sighting => this.sighting = sighting);

    eventBus.$on('sighting-added', (sighting) => {
      this.sighting.push(sighting)
    })

    eventBus.$on('sighting-deleted', (id) => {
      let index = this.sighting.findIndex(sighting => sighting._id === id)
      this.sighting.splice(index, 1)
    })
  },
	components: {
    'sighting': Sighting
  },
}
</script>

<style lang="css" scoped>
#sightingsGrid {
	display: flex;
	flex-wrap: wrap;
	justify-content: space-evenly;
}

h2 {
	padding: 0;
	margin: 0;
}
</style>
