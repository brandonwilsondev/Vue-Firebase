<template>
  <container bg fill-height grid-list-md text-xs-center>
    <v-card class="main">
      <v-card-title class="justify-center">
        Specialties
      </v-card-title>
      <div class="justify-center">
        <v-row row>
          <v-flex xs-1 md-3 offset-1>
            <v-list class="list">
              <li>Agricultural</li>
              <li>Air Conditioning</li>
              <li>Auto Paint & Body</li>
              <li>Auto Mechanics</li>
              <li>Auto Detailing</li>
              <li>Brick Pavers Brick & Block Masonry</li>
              <li>Carpentry (rough and finish)</li>
              <li>Cabinetry</li>
              <li>Construction Cleaning</li>
              <li>Cutting and Drilling</li>
              <li>Concrete Placing & Finishing</li>
              <li>Demolition</li>
              <li>Delivery/Driver</li>
              <li>Drywall</li>
              <li>Electrical</li>
            </v-list>
          </v-flex>
          <v-flex xs-1 md-3 offset-1>
            <v-list>
              <li>Flooring</li>
              <li>Fence</li>
              <li>Fire & Security Alarm</li>
              <li>Fire Protection</li>
              <li>Glass & Windows</li>
              <li>General Construction</li>
              <li>Granite & Stone Fabrication</li>
              <li>Golf Course and Greens keeper</li>
              <li>Handyman</li>
              <li>HVAC</li>
              <li>Home Remodeling</li>
              <li>Housekeeping/ Maid service</li>
              <li>Irrigation/Sprinkler</li>
              <li>Insulation</li>
              <li>Janitorial</li>
            </v-list>
          </v-flex>
          <v-flex xs-1 md-3 offset-1>
            <v-list>
              <li>Landscaping</li>
              <li>Marine</li>

              <li>Mechanical Systems</li>
              <li>Moving</li>
              <li>Painting</li>
              <li>Paving & Roadwork</li>
              <li>Pest Control</li>
              <li>Plumbing</li>
              <li>Pool Service</li>
              <li>Pool & Spa installation</li>
              <li>Roofing</li>
              <li>Stucco & Plastering</li>
              <li>Tile & Marble</li>
              <li>Tree Work</li>
              <li>Warehouse</li>
            </v-list>
          </v-flex>
        </v-row>
        <div class="buttons text-center">
          <router-link to="/WorkersPage">
            <v-btn class="callbutton" tile color="primary lighten-3">
              I'm looking for work
            </v-btn>
          </router-link>
          <router-link to="/NeedWorkers">
            <v-btn class="callbutton" tile color="primary lighten-3">
              I'm looking for workers
            </v-btn>
          </router-link>
        </div>
      </div>
    </v-card>
  </container>
</template>

<script>
export default {
  metaInfo: {
    title: 'Specialty Stairs What Specialty do you have, What Specialty are you looking to hire?',
    titleTemplate: 'Specialty Stars',
     htmlAttrs: {
        lang: 'en',
        amp: true
      }
  },
  name: "Specialties"
};
</script>

<style scoped>
.callbutton {
  margin: 15px;
}
.main {
  margin-top: 0px;
}
.text {
  display: inline-block;
}
.list {
  background: lightgrey;
}
</style>
