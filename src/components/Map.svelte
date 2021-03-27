<script>
  export let markers;

  let container;
  let map;
  
  import { onMount } from 'svelte';
   
  onMount(async () => {
    map = new google.maps.Map(container, {
      zoom: 6,
      center: {lat: 52.039337210816925, lng: 19.170637733080902},
      mapId: '27ec8f13ee381840',
      disableDefaultUI: true,
    });

    markers.forEach(element => {
      const location = element.geopoint.split(',');

      const marker = new google.maps.Marker({
        position: { lat: parseFloat(location[0]), lng: parseFloat(location[1]) },
        map: map,
        title: element.name,
      });

      marker.addListener("click", () => {
        window.location.href = `http://localhost:3000/associations/${element.id}`
      });
    });
  });
</script>

<style>
  .map {
    height: 450px;
  }
</style>

<div class="w-full map" bind:this={container}></div>