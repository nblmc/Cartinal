---
---

<br>

[← Back to description](./description.html)

# metadataSchema

<template>
    <div v-if="this.dataLifecycle.description" id = "container">
      <p class="larger-text">{{this.dataLifecycle.description.properties.metadataSchema.description}}</p>
      <p >Expected Type: <strong>{{this.dataLifecycle.description.properties.metadataSchema.type}}</strong></p>
    <table id ="property-table">
        <tr>
            <th>Property</th>
            <th>Expected Type</th>
            <th>Constant Value</th>
        </tr>
        <tr v-for="item, index in this.dataLifecycle.description.properties.metadataSchema.properties" :key="index">
            <td>{{index}}</td>
            <td>{{item.type}}</td>
            <td>{{item.const}}</td>
        </tr>
    </table> 
    </div>
</template>

<script>
import axios from 'axios'


export default {

    data() {
        return {
          schema: [],
          coreCitation: [],
          dataEndpoints: [],
          subjectTagging: [],
          dataBiography: [],
          resourceConstellation: [],
          dataLifecycle: [],
        }
    },
    methods: {
        whatsUp(){
          console.log(this.dataEndpoints)
        }
    },
    computed: {
        data() {
            return this.$page.frontmatter
        }
    },
    created() {
        //returns a promise
        axios.get("https://raw.githubusercontent.com/nblmc/Data-Context/master/schema.json")
            .then(response => {
                this.schema = response.data.properties
                this.coreCitation = response.data.properties.coreCitation.properties
                this.dataEndpoints = response.data.properties.dataEndpoints
                this.subjectTagging = response.data.properties.subjectTagging.properties
                this.dataBiography = response.data.properties.dataBiography.properties
                this.resourceConstellation = response.data.properties.resourceConstellation.properties
                this.dataLifecycle = response.data.properties.dataLifecycle.properties
            }).catch(err => {
                console.log(err)
            })
    }
}
</script>

<style lang="stylus">

table#property-table
  width:100%

p.larger-text
  font-size 120%

</style>

## Example 

``` json
"metadataSchema": {
	"schemaName": "LMEC Data Description Schema",
	"$id": "https://github.com/nblmc/Data-Context/blob/master/schema.json"
}
```