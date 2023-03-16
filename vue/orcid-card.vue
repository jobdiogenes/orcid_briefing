<script>
export default {
props: ["orcid"],
  data() {
    return {
      res: {}
    };
  },
  created() {
    this.loadData(this.orcid);
  },
  methods: {
    loadData(orcid) {
      const qURL =
        "https://raw.githubusercontent.com/adminpea/orcid_briefing/main/profiles/" +
        orcid +
        ".json";
      fetch(qURL)
        .then((resp) => resp.json())
        .then((json) => {
          this.res = json;
          this.res.external_ids.map((el) => {
            el["icon"] = this.icon(el.type);
            return el;
          });
          console.log(text);
        });
    },
    icon(name) {
      const names = [
        "scopus",
        "lattes",
        "researchid",
        "schoolar",
        "researchgate"
      ];
      const icons = [
        "ai ai-scopus",
        "ai ai-lattes",
        "ai ai-researchid",
        "ai ai-google-schoolar",
        "ai ai-researchgate"
      ];
      for (var i in icons) {
        if (name.toLowerCase().indexOf(names[i]) >= 0) {
          return icons[i];
        }
      }
      return "Person";
    }
  }
}
</script>

<template>
  <q-card class="my-card">                  
        <q-card-section horizontal>
          <q-img class="col-4" :src="res.photo"> </q-img>
          <q-card-section>
            <i class="ai ai-ciencia-vitae" style="font-size:36px;  padding-right: 30px;"></i>
            <span class="text-h5 text-bold">{{res.names["given-names"]}} {{res.names["family-name"]}}</span>
            <hr>
            <q-card-section>
              <span class="text-h6">Publications
                <q-badge color="orange" align="top">{{res.total_papers}}</q-badge>
              </span>
              <span>Citations
                <q-badge color="orange" align="top">{{res.total_citations}}</q-badge>
              </span>
              <span>H-Index
                <q-badge color="orange" align="top">{{res.hindex}}</q-badge>
              </span>              
            </q-card-section>
            {{res.biography}}
          </q-card-section>
        </q-card-section>
        <q-card-section class="bg-black text-white lnks">
          <a :href="'https://orcid.org/'+res.orcid">
            <i class="ai ai-orcid"></i>
            <q-tooltip>ORCID</q-tooltip>
          </a>
          <span v-for="id in res.external_ids">
            <a :href="id.url">
              <i :class="id.icon"></i>
              <q-tooltip>{{id.type}}</q-tooltip>
            </a>
          </span>          
        </q-card-section>
      </q-card>
</template>

<style>
@import url(https://fonts.googleapis.com/css?family=Open+Sans:300,400,600);
@import url(https://cdn.jsdelivr.net/gh/jpswalsh/academicons/css/academicons.min.css);
@import url(https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.13.1/css/all.min.css);
@import url("https://cdn.jsdelivr.net/npm/quasar@2.10.2/dist/quasar.min.css");
@import url("https://fonts.googleapis.com/css?family=Roboto:100,300,400,500,700,900|Material+Icons");
.my-card {
  width: 100%;
  min-width: 640px;
  min-heigth: 340px;
}

.lnks i {
  color: white;
  font-size: 28px;
  padding: 5px;
}
</style>
