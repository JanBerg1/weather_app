<template>
    <div class="col col-pv" v-show="location.latitude">
        <GmapMap
            :center="center"
            :zoom="15"
            :options="{disableDefaultUI : true}"
            map-type-id="terrain"
            style="width: 100%; height: 100%"
            >
            <GmapMarker
                :position="getPosition(location)"
                :clickable="true"
                :draggable="true"
                @click="center=getPosition(location)"
            />
            <GmapMarker
                :key="index"
                v-for="(pin, index) in pins"
                :position="pin.location"
                :clickable="true"
                @click="center=pin.location"
            />
        </GmapMap>
    </div>
</template>


<script>

    export default {
        mounted() {
            console.log('Component mounted.')
        },
        props  : {
            location: Object,
            pins : Array
        },
        data() {
            return {   
                center : {}
            }
        },
        watch : {
            pins: {
                handler() {
                    // Show pins on the map and center on "them", in this app we only use one pin
                    if(this.pins.length == 0) {
                        // Center on original location if pins are removed
                        this.center = this.getPosition(this.location);
                    } else {
                        // Center on the most recently added pin
                        this.center = this.pins[this.pins.length - 1].location;
                    }
                }
            },
            location: {
                handler() {
                    // Center on loaded location
                    this.center= this.getPosition(this.location);
                }
            }
        },
        methods : { 
            getPosition(location) {
                return {
                    lat : location.latitude,
                    lng : location.longitude
                }
            }
        }
    }
</script>