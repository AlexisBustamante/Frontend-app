    <template>
  <v-data-table
    :headers="dessertHeaders"
    :items="desserts"
    :single-expand="singleExpand"
    :expanded.sync="expanded"
    item-key="name"
    show-expand
    class="elevation-1"
  >
    <template v-slot:top>
      <v-toolbar flat>
        <v-spacer></v-spacer>

        <v-switch
          v-model="singleExpand"
          label="Single expand"
          class="mt-2"
        ></v-switch>

      </v-toolbar>
    </template>

    <template v-slot:expanded-item="{ headers, item }">
    
      <td :colspan="headers.length">
         <v-icon dark color="purple" class="mr-2">
            mdi-account-details
          </v-icon>

          <v-icon
            dark
            color="primary"
            class="mr-2"
            >
            mdi-pencil
          </v-icon>

          <v-icon dark color="red" class="mr-2" >
            mdi-delete
          </v-icon>
      </td>
    </template>
  </v-data-table>
</template>

<script>
  export default {
    data () {
      return {
        expanded: [],
        singleExpand: false,
        dessertHeaders: [
          {
            text: 'Dessert (100g serving)',
            align: 'start',
            sortable: false,
            value: 'name',
          },
          { text: 'Calories', value: 'calories' },
          { text: 'Fat (g)', value: 'fat' },
          { text: 'Carbs (g)', value: 'carbs' },
          { text: 'Protein (g)', value: 'protein' },
          { text: 'Iron (%)', value: 'iron' },
          { text: '', value: 'data-table-expand' },
        ],
        desserts: [
          {
            name: 'Frozen Yogurt',
            calories: 159,
            fat: 6.0,
            carbs: 24,
            protein: 4.0,
            iron: '1%',
            autos:[{
                marca:'susuki',
                modelo:'vitara',
                id:1230
            },{
                marca:'nissan',
                modelo:'v 16',
                id:1231
            }]              
            
          },
          {
            name: 'Ice cream sandwich',
            calories: 237,
            fat: 9.0,
            carbs: 37,
            protein: 4.3,
            iron: '1%',
            autos:[{
                marca:'toyota',
                modelo:'urban',
                id:1232
            },{
                marca:'brutal',
                modelo:'v 17',
                id:1233
            }]              
          }
        
        ],
      }
    },
  }
</script>
