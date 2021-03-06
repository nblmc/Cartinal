---
sidebar: none
---

<br>

[← Back to Schema Home](./)

# resources

<template>
   <table v-if="this.schema.resources" id ="property-table">
     <p class="larger-text">{{this.schema.resources.description}}</p>
  <tr>
    <th>Property</th>
    <th>Expected Type</th>
    <th>Required</th>
    <th>Description</th>
  </tr>
  <tr v-for="item, index in this.resources" :key="index">
    <td><a :href="index + '.html'" >{{index}}</a></td>
    <td>{{item.type}}</td>
    <td id="required">{{checkRequired(index, schema.resources.required)}}</td>
    <td>{{item.description}}</td>
  </tr>
</table> 
</template>

## Example use

```json
	"resources": {
		"documentation": [{
			"$id": "https://nhgis.org/sites/www.nhgis.org/files/tract-availability.pdf",
			"title": "Census Tract Boundary File Availability Chart by U.S. County",
			"maintainedBy": "National Historic GIS (NHGIS)"
		},
		{
			"$id": "https://www.nhgis.org/user-resources/data-availability#gis-files",
			"title": "Data Availability: GIS Files",
			"maintainedBy": "National Historic GIS (NHGIS)"
		},
		{
			"$id": "https://www2.census.gov/geo/pdfs/reference/GARM/glosGARM.pdf",
			"title": "Geographic Areas Reference Manual: Glossary",
			"maintainedBy": "United States Census Bureau"
		},
		{
			"$id": "https://www2.census.gov/geo/pdfs/reference/GARM/Ch10GARM.pdf",
			"title": "Geographic Areas Reference Manual: Census Tracts",
			"maintainedBy": "United States Census Bureau"
		}],
		"tutorials": [{
			"$id": "https://www.youtube.com/watch?v=P1znKKm8vX4",
			"title": "NHGIS Data Finder Tutorial",
			"author": "IPUMS",
			"datePublished": "2013-09-18",
			"notes": "Two minute YouTube video",
			"suggested": true
		},
		{
			"$id": "http://steppingintothemap.com/mappinghistory/wp-content/uploads/2018/03/Working-with-NHGIS-and-Joins.pdf",
			"title": "Working with NHGIS and Joins",
			"author": "Creighton University Digital Humanities",
			"datePublished": "2018-03-01",
			"notes": "Learn how to join census data with demographic data in QGIS",
			"suggested": true
		}]
	}
```

<script>
import axios from 'axios'


export default {

    data() {
        return {
          schema: [],
          core: [],
          access: [],
          tags: [],
          considerations: [],
          resources: [],
          lifecycle: []
        }
    },
    methods: {
        whatsUp(){
          console.log(this.core)
        },
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
                this.core = response.data.properties.core.properties
                this.access = response.data.properties.access
                this.tags = response.data.properties.tags.properties
                this.considerations = response.data.properties.considerations.properties
                this.resources = response.data.properties.resources.properties
                this.lifecycle = response.data.properties.lifecycle.properties
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