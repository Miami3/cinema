<template>
    <v-container class="cinema__layout" fluid align-center>
        <v-layout align-center row wrap>
            <v-flex class="seat" :class="{ booked: seat.booked, selected: seat.selected }"
                    v-for="seat in seats" :key="seat.number" @click="selectSeat(seat)">
                <span>{{ seat.id }}</span>
            </v-flex>
        </v-layout>
        <v-divider></v-divider>
        <v-flex class="text-xs-center confirm__booking__wrapper" v-if="totalSeatsSelected > 0">
            <v-btn color="info" @click="show_cart = true">
                <span class="confirm__booking__text">Confirm Booking</span>
                <v-badge right color="error">
                    <span slot="badge">{{totalSeatsSelected ? totalSeatsSelected : 0}}</span>
                    <v-icon large color="white">shopping_cart</v-icon>
                </v-badge>
            </v-btn>
        </v-flex>
        <v-bottom-sheet v-model="show_cart">
            <v-card>
                <v-card-text>
                    <p class="text-xs-center">
                        You selected seats:
                        <span v-for="item in cart"> #{{item.id}} in {{item.id | getRow}} row; </span>
                    </p>
                </v-card-text>
                <v-card-title class="justify-center" primary-title>
                <h2>Total: {{totalSeatsSelected*100}}</h2>
                </v-card-title>

                <v-card-actions class="justify-center">
                    <v-btn color="success" @click="buySelectedSeats">Buy</v-btn>
                </v-card-actions>
            </v-card>
        </v-bottom-sheet>
        <v-alert v-if="alert" value="true" outline type="success">Thank you for your order!</v-alert>
    </v-container>
</template>

<script>
    export default {
        name: "CinemaComponent",
        data () {
            return {
                seats: [],
                show_cart: false,
                cart: [],
                number: 0,
                alert: false
            }
        },
        methods: {
          buySelectedSeats () {
              let self = this;
              if(self.cart.length){
                self.cart.forEach((item) => {
                   self.seats[item.id - 1].booked = true;
                   self.seats[item.id - 1].selected = false;
                });
              }
              self.show_cart = false;
              self.alert = true;
          },
          selectSeat (seat) {
            this.alert = false;
            if(!seat.booked){
                seat.selected = !seat.selected;
            }
          },
          fillSeats () {
              let self = this;
              // Create cinema
              for(let i=1; i<=100; i++){
                  self.seats.push({
                      id: i,
                      selected: false,
                      booked: false
                  });
              }

              // Random seats booking
              let j= 1;
              while(j<=10){
                  let random = self.randomNum(100);
                  if(self.seats[random].booked === false){
                      self.seats[random].booked = true;
                      j++;
                  }
              }
          },
          randomNum (num) {
              return Math.floor(Math.random() * num + 1);
          }
        },
        computed: {
            totalSeatsSelected () {
                this.cart = this.seats.filter(seat => seat.selected);
                return this.cart.length;
            }
        },
        filters: {
            getRow: function (value) {
                if (!value) return '';
                if(value < 10){
                    return 1
                }
                value = value.toString();
                return +value.charAt(0)+1;
            }
        },
        created () {
            this.fillSeats();
        }
    }
</script>

<style scoped>
    .cinema__layout {
        max-width: 460px;
    }
    .seat {
        width: 30px;
        height: 30px;
        background-color: #67CC45;
        text-align: center;
        line-height: 30px;
        margin: 5px;
        transition: all 0.3s ease;
    }
    .seat:hover {
        cursor: pointer;
        background-color: palegreen;
        transform: scale(1.25,1.25);
    }
    .booked, .booked:hover {
        background-color: #D45555;
    }
    .selected, .selected:hover {
        background-color: #EBDF42;
    }
    .confirm__booking__wrapper {
        margin: 25px 0;
    }
    .confirm__booking__text {
        margin: 0 15px 0 0;
    }
</style>