<template>
  <section id="body">
    <div class="q-py-xl">
      <div 
      v-for="listId in fetchData" 
      v-bind:key="listId.listId" 
      class="q-py-md">
        <q-card class="cards shadow">
          <q-card-section><div class="text-h5 q-mt-sm q-mb-xs">ListId: {{ listId.listId }}</div>
            <div class="text-h6 text-grey-5">
              {{ listId.children.length }} Items
            </div>
          </q-card-section>

          <q-card-actions>
            <q-space />

            <q-btn position="bottom-right"
              color="grey"
              round
              flat
              dense
              :icon="listId.expanded ? 'keyboard_arrow_up' : 'keyboard_arrow_down'"
              @click="listId.expanded = !listId.expanded"
            />
          </q-card-actions>

          <q-slide-transition>
            <div v-show="listId.expanded">
              <q-separator />
              <q-card-section class="text-subitle2">
                <ul v-for="name in listId.children" 
                v-bind:key="name.id" 
                class="q-py-xs">
                  <li>{{ name.name }}</li>
                </ul>
              </q-card-section>
            </div>
          </q-slide-transition>
        </q-card>
      </div>
    </div>
  </section>
</template>

<script>
import axios from 'axios';
export default {
  data() {
    return {
      cors: 'https://cors-anywhere.herokuapp.com/',
      url: 'https://fetch-hiring.s3.amazonaws.com/hiring.json',
      fetchData: [],
    }
  },
  async mounted() {
    this.$axios.get(`${this.cors}${this.url}`).then(response => {
    var array = response.data;
    var result;

    //remove nulls and spaces
    array = array.filter(item => item.name);

    //sort by listId and name by removing "Item "
    array = array.sort(function(a, b) {
      return (b.name === null) - (a.name === null) || a.listId - b.listId || parseInt(a.name.replace("Item ", "")) - parseInt(b.name.replace("Item ", ""));
    });

    //group data by listId
    result = array.reduce((r, { listId: listId, ...object }) => {
      var temp = r.find(o => o.listId === listId);

      if (!temp) { 
        r.push(temp = { listId, children: [], expanded: false });
        
        }
        temp.children.push(object);
      return r;
      }, []);

      //assign to variable to be used
      this.fetchData = result;
    }).catch(error => {
      console.log(error.message)
    });
  }
}
</script>

<style lang="scss">
.cards {
  width: 350px;
  top:0; 
  margin: 0 auto;
}

</style>
