---

---

<br>

[← Back to Schema Home](./)

# publicTrust

<template>
   <table v-if="this.documentationHealth.publicTrust" id ="property-table">
     <p class="larger-text">{{this.documentationHealth.publicTrust.description}}</p>
  <tr>
    <th>Property</th>
    <th>Expected Type</th>
    <th>Required</th>
    <th>Description</th>
  </tr>
  <tr v-for="item, index in this.documentationHealth.publicTrust.properties" :key="index">
    <td><a :href="index + '.html'" >{{index}}</a></td>
    <td>{{item.type}}</td>
    <td id="required">{{checkRequired(index, schema.documentationHealth.properties.publicTrust.properties.required)}}</td>
    <td>{{item.description}}</td>
  </tr>
</table> 
</template>

<script>
import axios from 'axios'


export default {

    data() {
        return {
          schema: [],
          citation: [],
          endpoints: [],
          filterTagging: [],
          documentationHealth: [],
          relatedResources: [],
          peopleLifecycle: []
        }
    },
    methods: {
        checkRequired(evaluatedItem, requiredFieldsList){
        if (requiredFieldsList === undefined || requiredFieldsList.length == 0) {
            return ''
        } else {
          if (requiredFieldsList.includes(evaluatedItem)){
              return 'x'
          } else {
              return ''
          }
        }
      }
    },
    computed: {
        data() {
            return this.$page.frontmatter
        }
    },
    created() {
        //returns a promise
        axios.get("https://raw.githubusercontent.com/bplmaps/data-description-schema/master/schema.json")
            .then(response => {
                this.schema = response.data.properties
                this.citation = response.data.properties.citation.properties
                this.endpoints = response.data.properties.endpoints
                this.filterTagging = response.data.properties.filterTagging.properties
                this.documentationHealth = response.data.properties.documentationHealth.properties
                this.relatedResources = response.data.properties.relatedResources.properties
                this.peopleLifecycle = response.data.properties.peopleLifecycle.properties
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

td#required
  text-align center

</style>

## Health checklist

::: tip How we check documentation health
You can find out how we check documentation in the [documentation health check guide](./healthcheck.html)
:::

## Example use

```json
"publicTrust":{
  "healthScore": 3,
  "healthEvaluation": "Census documentation is clear about methods used for anonymizing and participant privacy. It discusses the challenges of grouping or [aggregating](http://wiki.gis.com/wiki/index.php/Aggregation) survey responses into homogenous areas. It acknowledges Census Bureau reasoning for why the survey is mandatory. It explains why the data is beneficial to society. \n \n The documentation does not address how the Census Bureau understands participant consent. It is not clear if all participants understand how their data will be used. "		
}
```