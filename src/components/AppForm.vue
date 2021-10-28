<template>
  <b-container>
    <div class="container">
      <b-modal class="modal" hide-footer ref="my-modal">
        <div class="d-block text-center">
          <h3>Are you sure that you want to delete the transaction?</h3>
        </div>
        <b-button class="mt-3" variant="secondary" pill block @click="hideModal"
          >Cancel</b-button
        >
        <b-button
          class="mt-2"
          variant="danger"
          pill
          block
          @click="confirmDelete()"
          >Confirm</b-button
        >
      </b-modal>
      <b-form @submit="onSubmit" @reset="onReset" v-if="show">
        <b-row align-v="center">
          <b-col cols="12" lg="3" md="12" sm="12">
            <b-form-group
              id="input-group-1"
              label="Transaction Amount:"
              label-for="input-1"
            >
              <b-form-input
                id="input-1"
                v-model="form.amount"
                type="number"
                step="0.01"
                pattern="[0-9]+"
                min="0"
                required
                placeholder="Enter amount"
              ></b-form-input>
            </b-form-group>
          </b-col>
          <b-col cols="12" lg="3" md="12" sm="12">
            <b-form-group
              id="input-group-2"
              label="Location:"
              label-for="input-2"
            >
              <b-form-input
                id="input-2"
                v-model="form.location"
                type="text"
                required
                placeholder="Enter Location"
              ></b-form-input>
            </b-form-group>
          </b-col>
          <b-col cols="12" lg="3" md="12" sm="12">
            <b-form-group
              id="input-group-3"
              label="Cryptocurrency type:"
              label-for="input-3"
            >
              <b-form-select
                id="input-3"
                v-model="form.currency"
                :options="currency"
                required
              ></b-form-select>
            </b-form-group>
          </b-col>
          <b-col cols="12" lg="3" md="12" sm="12">
            <b-form-group id="input-group-3" label="Date:" label-for="input-3">
              <b-form-datepicker
                id="datepicker-placeholder"
                v-model="form.date"
                placeholder="Choose a date"
                local="en"
              ></b-form-datepicker>
            </b-form-group>
          </b-col>
        </b-row>
        <b-button class="button-save" type="submit" pill variant="primary"
          >Save</b-button
        >
        <b-button class="button-reset" type="reset" pill variant="danger"
          >Reset</b-button
        >
      </b-form>
    </div>
    <br />
    <div class="container">
      <div class="shadow p-3 mb-5 rounded">
        <table class="table table-hover">
          <tr>
            <th>Cryptocurrency</th>
            <th>Amount</th>
            <th>Location</th>
            <th>Date(YYYY/DD/MM)</th>
            <th>Market Value</th>
            <th>Option</th>
          </tr>
          <tbody>
            <tr v-for="(item, index) of transaction" v-bind:key="index">
              <td>{{ item.currency }}</td>
              <td>{{ item.amount }}</td>
              <td>{{ item.location }}</td>
              <td>{{ item.date }}</td>
              <td>{{ item.market_value }} €</td>
              <td>
                <b-button
                  class="button-delete"
                  pill
                  variant="danger"
                  @click="setValues(item.id, index)"
                  >Delete</b-button
                >
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </b-container>
</template>
<script>
import axios from "axios";
export default {
  data() {
    return {
      API_URL: process.env.API_URL,
      modalShow: false,
      selected_id: null,
      selected_index: null,
      dialog: false,
      form: {
        amount: "",
        location: "",
        currency: null,
        date: ""
      },
      currency: [
        { text: "Select One", value: null },
        "Bitcoin",
        "Ethereum",
        "Ripple"
      ],
      transaction: [],
      show: true
    };
  },
  created() {
    axios
      .get(this.API_URL + "/api/v1/portfolio_transactions")
      .then(res => {
        const data = res.data.data;
        for (var i = 0; i < data.length; i++) {
          this.transaction.push({
            id: data[i].id,
            currency: data[i].currency.name,
            amount: data[i].amount,
            location: data[i].location,
            date: data[i].date,
            market_value: data[i].market_value
          });
        }
      })
      .catch(err => console.log(err));
  },
  methods: {
    onSubmit(evt) {
      console.log(this.form);
      evt.preventDefault();
      axios
        .post(this.API_URL + "/api/v1/portfolio_transactions", this.form)
        .then(res => {
          const data = res.data.data;
          this.transaction.push({
            id: data.id,
            currency: data.currency.name,
            amount: data.amount,
            location: data.location,
            date: data.date,
            market_value: data.market_value
          });
          this.onReset(evt);
        })
        .catch(err => console.log(err));
    },
    onReset(evt) {
      evt.preventDefault();
      // Reset our form values
      this.form.amount = "";
      this.form.location = "";
      this.form.currency = null;
      this.form.date = "";
      // Trick to reset/clear native browser form validation state
      this.show = false;
      this.$nextTick(() => {
        this.show = true;
      });
    },
    setValues(id, index) {
      this.selected_id = id;
      this.selected_index = index;
      this.$refs["my-modal"].show();
    },
    confirmDelete() {
      axios
        .delete(
          this.API_URL + "/api/v1/portfolio_transactions/" + this.selected_id
        )
        .then(this.transaction.splice(this.selected_index, 1))
        .catch(err => console.log(err));
      this.$refs["my-modal"].hide();
    },
    hideModal() {
      this.$refs["my-modal"].hide();
    }
  }
};
</script>
<style scoped>
.table.table-hover {
  margin-right: 0 !important;
  overflow: hidden;
}
.button-reset {
  background-color: #720632;
  width: 120px;
  border: none;
  text-decoration-color: bisque;
  margin-left: 10px;
  color: white;
}
.button-delete {
  background-color: #970a44;
  width: 80px;
  border: none;
  text-decoration-color: bisque;
  color: white;
}
.mt-2 {
  background-color: #720632;
  border: none;
  text-decoration-color: bisque;
  color: white;
}
.button-save {
  background-color: #1c4668;
  width: 120px;
  border: none;
  text-decoration-color: bisque;
  color: white;
}
.mt-3 {
  background-color: #1c4668;
  border: none;
  text-decoration-color: bisque;
  color: white;
}
@media (max-width: 403px) {
  .button-save {
    background-color: #1c4668;
    width: 95px;
    border: none;
    margin-left: 10px;
    text-decoration-color: bisque;
    color: white;
  }

  .button-reset {
    background-color: #720632;
    width: 95px;
    border: none;
    text-decoration-color: bisque;
    margin-left: 10px;
    color: white;
  }
}
@media (max-width: 394px) {
  .button-save {
    background-color: #1c4668;
    width: 95px;
    border: none;
    text-decoration-color: bisque;
    color: white;
  }

  .button-reset {
    background-color: #720632;
    width: 95px;
    border: none;
    text-decoration-color: bisque;
    margin-left: 10px;
    color: white;
  }
}
@media (max-width: 363px) {
  .button-save {
    background-color: #1c4668;
    width: 80px;
    border: none;
    margin-left: 20px;
    text-decoration-color: bisque;
    color: white;
  }

  .button-reset {
    background-color: #720632;
    width: 80px;
    border: none;
    text-decoration-color: bisque;
    margin-left: 10px;
    color: white;
  }
}
@media (max-width: 992px) {
  table,
  thead,
  tbody,
  th,
  td,
  tr {
    display: block;
  }

  /* Hide table headers (but not display: none;, for accessibility) */
  thead tr {
    position: absolute;
    top: -9999px;
    left: -9999px;
  }

  tr {
    border: 1px solid #ccc;
  }

  td {
    /* Behave  like a "row" */
    border: none;
    border-bottom: 1px solid #eee;
    position: relative;
    padding-left: 2%;
  }

  td:before {
    /* Now like a table header */
    position: absolute;
    /* Top/left values mimic padding */
    top: 6px;
    left: 6px;
    width: 45%;
    padding-right: 10px;
    white-space: nowrap;
  }
}
</style>
