<template>
  <div class="grid-container-item">
    <h3 class="seat">
      Row: {{ getRow() }}, Seat: {{ seatIndexes[count] + 1 }}
    </h3>
    <h3 class="price">Price: {{ ticketPrice }} kr</h3>
    <div class="box">
      <Dropdown
        :title="placeHolder"
        :items="types"
        @update-price="updatePrice"
      />
    </div>
  </div>
</template>

<script>
import Dropdown from "./Dropdown.vue";
export default {
  emits: ["update-total"],
  props: ["count", "seatIndexes", "ticketTypes"],
  watch: {
    ticketTypes: {
      deep: true,
      handler(newVal) {
        this.newVal = newVal;
        this.setTicketInfo();
      },
    },
    seatIndexes: {
      deep: true,
      handler() {
        this.setTicketInfo();
      },
    },
  },
  components: {
    Dropdown,
  },
  data() {
    return {
      types: [
        {
          title: "Adult",
          price: "250",
        },
        {
          title: "Child (0-12)",
          price: "150",
        },
        {
          title: "Senior (65+)",
          price: "170",
        },
      ],
      ticketPrice: "-",
      placeHolder: "Choose ticket type",
      newVal: []
    };
  },
  methods: {
    setTicketInfo() {
      let obj = this.newVal.filter(
        (ticket) => ticket.seatIndex == this.seatIndexes[this.count]
      )[0];
      if (obj != undefined) {
        this.placeHolder = obj.ticketType;
        this.ticketPrice = obj.price;
      } else {
        this.placeHolder = "Choose ticket type";
        this.ticketPrice = "-";
      }
    },
    updatePrice(title, price) {
      this.$emit("update-total", title, price, this.seatIndexes[this.count]);
    },
    getRow() {
      const x = this.seatIndexes[this.count];
      return x < 10
        ? 1
        : x < 20
        ? 2
        : x < 30
        ? 3
        : x < 40
        ? 4
        : x < 50
        ? 5
        : x < 60
        ? 6
        : x < 70
        ? 7
        : x < 80
        ? 8
        : x < 90
        ? 9
        : 10;
    },
  },
};
</script>

<style scoped>
h3 {
  margin-top: 0;
}
.grid-container-item {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-template-rows: 30px;
  background-color: #6e1020;
  text-align: center;
  padding-left: 10px;
  margin-top: 10px;
  border-radius: 5px;
}
.box {
  background-color: rgb(94, 91, 87);
  cursor: pointer;
  grid-column: 3;
  grid-row: 1;
  border-radius: 5px;
  border: rgb(116, 109, 98);
}
.seat {
  grid-column: 1;
  grid-row: 1;
  border-radius: 5px;
  text-align: left;
  padding-top: 2px;
}
.price {
  grid-column: 2;
  grid-row: 1;
  border-radius: 5px;
  text-align: left;
  padding-top: 2px;
}
.sub-menu {
  grid-column: 3;
  top: 86px;
  left: 38.4%;
  width: max-content;
  border-radius: 5px;
}
</style>