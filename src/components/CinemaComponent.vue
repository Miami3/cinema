<template>
    <v-container fluid>
        <v-layout align-center row wrap>
            <v-flex class="seat" v-for="seat in seats" :key="seat.number">
                <span>{{ seat.number }}</span>
            </v-flex>
        </v-layout>
    </v-container>
</template>

<script>
    export default {
        name: "CinemaComponent",
        data () {
            return {
                seats: []
            }
        },
        methods: {
          fillSeats () {
              console.log(this);
              let self = this;
              for(let i=1; i<=100; i++){
                  self.seats.push({
                      number: i,
                      booked: false
                  });
              }
          }
        },
        mounted () {
            this.fillSeats();
        }
    }
</script>

<style scoped>
    .seat {
        width: 30px;
        height: 30px;
        background-color: palegoldenrod;
        justify-content: center;
        align-items: center;
        margin: 5px;
    }
</style>