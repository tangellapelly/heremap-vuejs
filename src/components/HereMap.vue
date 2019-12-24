<template>
  <div id="mapBox">
    <div id="mapContainer" style="height:600px;width:100%" ref="hereMap"></div>
  </div>
</template>

<script>
export default {
  name: "HereMap",
  props: {
    center: Object
  },
  data() {
    return {
      platform: null,
      apikey: "F3uoJ36IxvsIEnrf12wSJxmwhgRBVhIiLw0jpTQsvlw"
    };
  },
  async mounted() {
    // Initialize the platform object:
    const platform = new window.H.service.Platform({
      apikey: this.apikey
    });
    this.platform = platform;
    this.initializeHereMap();
  },
  methods: {
    initializeHereMap() {

      const mapContainer = this.$refs.hereMap;
      const H = window.H;
      // Obtain the default map types from the platform object
      var maptypes = this.platform.createDefaultLayers();

      // Instantiate (and display) a map object:
      var map = new H.Map(mapContainer, maptypes.vector.normal.map, {
        zoom: 10,
        center: this.center
      });

      addEventListener("resize", () => map.getViewPort().resize());

      // add behavior control
      new H.mapevents.Behavior(new H.mapevents.MapEvents(map));

      // add UI
      H.ui.UI.createDefault(map, maptypes);
      // End rendering the initial map
    }
  }
};
</script>

<style scoped>
#mapBox {
  width: 60vw;
  min-width: 360px;
  text-align: center;
  margin: 5% auto;
  background-color: #ccc;
}
</style>