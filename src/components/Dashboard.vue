<template>
  <div class="ui two column stackable grid">
    <div class="four wide column">
      <div class="ui segment">
        <h2>{{ $t('Total Sales', {locale: this.locale}) }}</h2>

        <p>
          <icon name="area-chart" scale="2"></icon>
          <br><br>
          
          {{ dashboard.orders }}
        </p>
      </div>
    </div>
    <div class="four wide column">
      <div class="ui segment">
        <h2>{{ $t('Total Clients', {locale: this.locale}) }}</h2>

        <p>
          <icon name="users" scale="2"></icon>
          <br><br>
          {{ dashboard.clients }}
        </p>
      </div>
    </div>
    <div class="four wide column">
      <div class="ui segment">
        <h2>{{ $t('Total Products', {locale: this.locale}) }}</h2>

        <p>
          <icon name="barcode" scale="2"></icon>
          <br><br>
          {{ dashboard.products }}
        </p>
      </div>
    </div>
    <div class="four wide column">
      <div class="ui segment">
        <h2>{{ $t('Cust All Products', {locale: this.locale}) }}</h2>

        <p>
          <icon name="credit-card" scale="2"></icon>
          <br><br>
          {{ dashboard.cust }}
        </p>
      </div>
    </div>
      <div class="sixteen wide column"></div>
      <line-chart :data="chartData"></line-chart>
    </div>
  </div>
</template>

<style type="text/css" scoped>
  .ui.grid {
    text-align: left;
    margin: 15px;
  }
</style>

<script>
export default {
  name: 'dashboard',
  data () {
    return {
      locale: process.env.LOCALE,
      chartData: [],
      dashboard: {
        orders: 'R$ 0,00',
        clients: 0,
        products: 0,
        cust: 0
      }
    }
  },
  created: function () {
    this.loadChart()
    this.totalOrders()
    this.countClients()
    this.countProducts()
    this.custProducts()
  },
  methods: {
    totalOrders () {
      this.$http.get(process.env.API + '/v1/orders/total').then(({ data }) => {
        this.dashboard.orders = data.total
      })
    },
    countClients () {
      this.$http.get(process.env.API + '/v1/clients/count').then(({ data }) => {
        this.dashboard.clients = data.count
      })
    },
    countProducts () {
      this.$http.get(process.env.API + '/v1/products/count').then(({ data }) => {
        this.dashboard.products = data.count
      })
    },
    custProducts () {
      this.$http.get(process.env.API + '/v1/products/cust').then(({ data }) => {
        this.dashboard.cust = data.cust !== null ? data.cust : 0
      })
    },
    loadChart () {
      this.$http.get(process.env.API + '/v1/orders/load').then(({ data }) => {
        this.chartData = data
      })
    }
  }
}
</script>
