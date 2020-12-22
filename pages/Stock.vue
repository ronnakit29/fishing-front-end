
<template>
  <div>
    <v-row>
      <v-col
        cols="12"
        lg="12"
      >
        <v-card flat>
          <v-card-text>
            <h2>
              <v-icon>mdi-cogs</v-icon> เมนูการจัดการ
            </h2>
          </v-card-text>
          <v-card-text>
            <v-row>
              <v-col
                sm="6"
                cols="12"
              >
                <v-autocomplete
                  label="รายการสินค้า"
                  :items="products"
                  item-text="name_and_detail"
                  item-value="_id"
                  v-model="search"
                  outlined
                  @change="loadStock"
                ></v-autocomplete>
              </v-col>
              <v-col
                sm="3"
                cols="6"
              >
                <v-text-field
                  label="จำนวนสินค้า"
                  required
                  v-model="form.qty"
                  outlined
                  type="number"
                ></v-text-field>
              </v-col>
              <v-col
                cols="6"
                sm="3"
              >
                <v-btn
                  color="success"
                  large
                  @click="addStock"
                  block
                >เพิ่มสินค้า<v-icon right>mdi-check-circle</v-icon>
                </v-btn>
              </v-col>
            </v-row>
          </v-card-text>
        </v-card>
      </v-col>
      <v-col
        cols="12"
        lg="12"
      >
        <v-divider></v-divider>
        <v-card flat>
          <v-card-text>
            <h2>
              <v-icon>mdi-clipboard-list</v-icon> รายการสต๊อกสินค้าล่าสุด
            </h2>
          </v-card-text>
          <v-card-text>
            <v-data-table
              :items="stock"
              :loading="datatable.loading"
              :headers="datatable.headers"
            >
            </v-data-table>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
  </div>
</template>

<script>
export default {
  name: 'Stock',
  async created () {
    await this.loadProduct();
  },
  data () {
    return {
      stock: [],
      products: [],
      search: null,
      manage: {
        del: null
      },
      select: {},
      form: {
        qty: null,
        pro_id: null
      },
      datatable: {
        loading: false,
        headers: [{ text: 'วัน/เวลา', value: 'stock_date' }, { text: 'จำนวน', value: 'stock_qty' }]
      },
      dialog: {
        add: false,
        confirm_del: false,
        edit: false
      },
      result: null,
    }
  },
  props: {

  },
  methods: {
    async addStock () {
      const stock = await (this.$axios.$post("stock/add", this.form));
      await this.loadStock();
      this.resetForm();
    },
    async loadStock () {
      this.form.pro_id = this.search;
      const stock = await (this.$axios.$get("stock/p/" + this.search));
      this.stock = stock.result;
    },
    async loadProduct () {
      const product = await (this.$axios.$get("products"));
      this.products = product.result;
      this.products.forEach(item => {
        item.name_and_detail = item.pro_name + " - " + item.pro_detail;
      })
    },
    resetForm () {
      this.form = {
        qty: null,
        pro_id: null
      };
    }
  },
}
</script>

<style lang="scss" scoped>
</style>

