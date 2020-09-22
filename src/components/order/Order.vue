<template>
  <div>
    <h1>Orders</h1>
    <table>
      <tr>
        <th>Customer Name</th>
        <th>No. of items</th>
        <th>Total amount</th>
        <th>Status</th>
        <th>Change Status</th>
      </tr>
      <tr v-for="order of orders" :key="order.id">
        <td>{{ order.customerName }}</td>
        <td>{{ order.noOfItems }}</td>
        <td>{{ order.total }}</td>
        <td>{{ order.status }}</td>
        <td>
          <button
            @click="changeStatus(order)"
            :disabled="order.status == 'Ready to serve'"
            :class="[
              order.status == 'Order Received'
                ? 'button red-button'
                : order.status == 'Preparing'
                ? 'button blue-button'
                : 'disable-button',
            ]"
          >
            Change Status
          </button>
        </td>
      </tr>
    </table>
  </div>
</template>

<script>
import HTTP from "../../http/httpConfig";
export default {
  name: "Orders",
  data() {
    return {
      orders: [],
    };
  },
  mounted() {
    this.getAllOrders();
  },
  methods: {
    
    /**
     * This method is use to fetch all orders from backednd server.
     */
    getAllOrders() {
      HTTP.get("/orders").then((res) => {
        console.log("oooooooooo", res);
        this.orders = res.data;
      });
    },

    /**
     * This method is use to change the status of specific order.
     */
    changeStatus(order) {
      const url = `/orders/${order.id}`;
      const updatedOrder = this.updateStatus(order);
      HTTP.put(url, updatedOrder).then((res) => {
        console.log("oooooooooo", res);
      });
    },

    /**
     * This method is use to find out the next status for oreder.
     */
    updateStatus(order) {
      switch (order.status) {
        case "Order Received":
          console.log("case 1");
          order.status = "Preparing";
          break;
        case "Preparing":
          console.log("case 2");
          order.status = "Ready to serve";
          break;
        default:
          console.log(`Sorry, didnt match`);
      }
      return order;
    },
  },
};
</script>

<style scoped>
table {
  border-collapse: collapse;
  width: 100%;
}

th,
td {
  text-align: left;
  padding: 8px;
}

tr:nth-child(even) {
  background-color: #f2f2f2;
}

th {
  background-color: #4caf50;
  color: white;
}

.button {
  border: none;
  color: white;
  padding: 7px 10px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
  margin: 4px 2px;
  cursor: pointer;
}
.disable-button {
  border: none;
  padding: 7px 10px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
  margin: 4px 2px;
  cursor: no-drop;
}

.blue-button {
  background-color: #008cba;
}
.red-button {
  background-color: #f44336;
}
</style>
