
<template>
  <div>
    <v-row>
      <v-col
        cols="12"
        lg="6"
      >
        <v-card-text>
          <h2>
            <v-icon>mdi-card-search</v-icon> ค้นหาข้อมูลสินค้า
          </h2>
        </v-card-text>
        <v-card-text>
          <v-text-field
            name="search"
            label="ค้นหาข้อมูล"
            id="txtSearch"
            placeholder="ข้อมูลที่ต้องการค้นหา"
            outlined
            v-model="search"
          >
          </v-text-field>
        </v-card-text>
      </v-col>
      <v-col
        cols="12"
        lg="6"
      >
        <v-card flat>
          <v-card-text>
            <h2>
              <v-icon>mdi-cogs</v-icon> เมนูการจัดการ
            </h2>
          </v-card-text>
          <v-card-text>
            <v-dialog
              v-model="dialog.edit"
              max-width="600px"
            >
              <template v-slot:activator="{ on, attrs }">
              </template>
              <v-card>
                <v-card-title>
                  <span class="headline">
                    <v-icon>mdi-plus-box</v-icon> เพิ่มสินค้า
                  </span>
                </v-card-title>
                <v-card-text>
                  <v-row>
                    <v-col
                      cols="12"
                      sm="12"
                    >
                      <v-text-field
                        label="ชื่อสินค้า"
                        required
                        v-model="form.pro_name"
                      ></v-text-field>
                    </v-col>
                    <v-col
                      cols="12"
                      sm="12"
                    >
                      <v-text-field
                        label="รายละเอียดสินค้า"
                        required
                        v-model="form.pro_detail"
                      ></v-text-field>
                    </v-col>
                    <v-col
                      cols="12"
                      sm="6"
                    >
                      <v-autocomplete
                        :items="select.category"
                        v-model="form.category_id"
                        item-value="_id"
                        item-text="category_text"
                        label="ประเภทสินค้า"
                      ></v-autocomplete>
                    </v-col>
                    <v-col
                      cols="12"
                      sm="6"
                    >
                      <v-text-field
                        label="จำนวนสินค้าในสต๊อก"
                        required
                        v-model="form.pro_stock"
                        type="number"
                      ></v-text-field>
                    </v-col>
                    <v-col
                      cols="12"
                      sm="6"
                    >
                      <v-text-field
                        label="ราคาต้นทุน"
                        required
                        v-model="form.pro_cost"
                      ></v-text-field>
                    </v-col>
                    <v-col
                      cols="12"
                      sm="6"
                    >
                      <v-text-field
                        label="ราคาขาย"
                        required
                        v-model="form.pro_price"
                      ></v-text-field>
                    </v-col>
                    <v-col
                      cols="12"
                      sm="12"
                    >
                      <v-select
                        :items="select.status"
                        v-model="form.pro_status"
                        label="สถานะการขาย"
                      ></v-select>
                    </v-col>
                  </v-row>
                </v-card-text>
                <v-card-actions>
                  <v-spacer></v-spacer>
                  <v-btn
                    color="red darken-1"
                    text
                    @click="dialog.edit = false"
                  >
                    <v-icon left>mdi-close</v-icon> ยกเลิก
                  </v-btn>
                  <v-btn
                    color="blue darken-1"
                    text
                    @click="editProducts"
                  >
                    <v-icon left>mdi-content-save</v-icon>บันทึกข้อมูล
                  </v-btn>
                </v-card-actions>
              </v-card>
            </v-dialog>
            <v-dialog
              v-model="dialog.add"
              max-width="600px"
            >
              <template v-slot:activator="{ on, attrs }">
                <v-btn
                  color="success"
                  dark
                  v-bind="attrs"
                  v-on="on"
                  large
                >
                  เพิ่มสินค้า <v-icon>mdi-plus-box</v-icon>
                </v-btn>
              </template>
              <v-card>
                <v-card-title>
                  <span class="headline">
                    <v-icon>mdi-plus-box</v-icon> เพิ่มสินค้า
                  </span>
                </v-card-title>
                <v-card-text>
                  <v-row>
                    <v-col
                      cols="12"
                      sm="12"
                    >
                      <v-text-field
                        label="ชื่อสินค้า"
                        required
                        v-model="form.pro_name"
                      ></v-text-field>
                    </v-col>
                    <v-col
                      cols="12"
                      sm="12"
                    >
                      <v-text-field
                        label="รายละเอียดสินค้า"
                        required
                        v-model="form.pro_detail"
                      ></v-text-field>
                    </v-col>
                    <v-col
                      cols="12"
                      sm="6"
                    >
                      <v-autocomplete
                        :items="select.category"
                        v-model="form.category_id"
                        item-value="_id"
                        item-text="category_text"
                        label="ประเภทสินค้า"
                      ></v-autocomplete>
                    </v-col>
                    <v-col
                      cols="12"
                      sm="6"
                    >
                      <v-text-field
                        label="จำนวนสินค้าในสต๊อก"
                        required
                        v-model="form.pro_stock"
                        type="number"
                      ></v-text-field>
                    </v-col>
                    <v-col
                      cols="12"
                      sm="6"
                    >
                      <v-text-field
                        label="ราคาต้นทุน"
                        required
                        v-model="form.pro_cost"
                      ></v-text-field>
                    </v-col>
                    <v-col
                      cols="12"
                      sm="6"
                    >
                      <v-text-field
                        label="ราคาขาย"
                        required
                        v-model="form.pro_price"
                      ></v-text-field>
                    </v-col>
                    <v-col
                      cols="12"
                      sm="12"
                    >
                      <v-select
                        :items="select.status"
                        v-model="form.pro_status"
                        label="สถานะการขาย"
                      ></v-select>
                    </v-col>
                  </v-row>
                </v-card-text>
                <v-card-actions>
                  <v-spacer></v-spacer>
                  <v-btn
                    color="red darken-1"
                    text
                    @click="dialog.add = false"
                  >
                    <v-icon left>mdi-close</v-icon>ยกเลิก
                  </v-btn>
                  <v-btn
                    color="blue darken-1"
                    text
                    @click="addProduct"
                  >
                    <v-icon left>mdi-content-save</v-icon>บันทึกข้อมูล
                  </v-btn>
                </v-card-actions>
              </v-card>
            </v-dialog>
            <v-btn
              color="primary"
              large
              @click="loadProducts"
            >รีเฟรช <v-icon>mdi-refresh</v-icon>
            </v-btn>
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
              <v-icon>mdi-clipboard-list</v-icon> รายการสินค้า
            </h2>
          </v-card-text>
          <v-card-text>
            <v-data-table
              :items="products"
              :loading="datatable.loading"
              :search="search"
              :headers="datatable.headers"
            >
              <template v-slot:item.pro_status="{ item }">
                <v-chip
                  v-if="item.pro_status == true"
                  color="success"
                >เปิดขาย</v-chip>
                <v-chip
                  v-else
                  color="red"
                  class="white--text"
                >ปิดขาย</v-chip>
              </template>
              <template v-slot:item._id="{ item }">
                <v-btn
                  color="success"
                  icon
                  @click="getProducts(item._id);dialog.edit = true;"
                >
                  <v-icon>mdi-pencil</v-icon>
                </v-btn>
                <v-btn
                  color="red"
                  icon
                  @click="dialogDelProduct(item._id)"
                >
                  <v-icon>mdi-trash-can</v-icon>
                </v-btn>
              </template>
            </v-data-table>
            <template>
              <v-row justify="center">
                <v-dialog
                  v-model="dialog.confirm_del"
                  max-width="290"
                >
                  <v-card>
                    <v-card-title class="headline">
                      คุณต้องการลบใช่หรือไม่
                    </v-card-title>

                    <v-card-text>
                      กด "ยืนยันเพื่อลบข้อมูล"
                    </v-card-text>

                    <v-card-actions>
                      <v-spacer></v-spacer>

                      <v-btn
                        color="green darken-1"
                        text
                        @click="dialog.confirm_del = false"
                      >
                        ยกเลิก
                      </v-btn>

                      <v-btn
                        color="red darken-1"
                        text
                        @click="deleteProducts"
                      >
                        ยืนยัน
                      </v-btn>
                    </v-card-actions>
                  </v-card>
                </v-dialog>
              </v-row>
            </template>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
  </div>
</template>

<script>
import Cookies from 'vue-cookie';
export default {
  middleware: 'authen',
  name: 'Products',
  created () {
    this.loadProducts()
  },
  data () {
    return {
      manage: {
        del: null
      },
      select: {
        category: [],
        status: [{ text: 'เปิดขาย', value: true }, { text: 'ปิดขาย', value: false }]
      },
      products: [],
      search: null,
      form: {
        pro_stock: null,
        pro_status: true,
        pro_cost: null,
        pro_price: null,
        tra_id: null,
        pro_name: null,
        pro_detail: null,
        category_id: null,
      },
      datatable: {
        loading: false,
        headers: [{
          text: 'ชื่อสินค้า',
          value: 'pro_name'
        },
        {
          text: 'รายการสินค้า',
          value: 'pro_detail'
        },
        {
          text: 'ราคาต้นทุน',
          value: 'pro_cost'
        },
        {
          text: 'ราคาขาย',
          value: 'pro_price'
        },
        {
          text: 'สินค้าคงเหลือ',
          value: 'pro_stock'
        },
        {
          text: 'สถานะ',
          value: 'pro_status'
        },
        {
          text: 'Manage',
          value: '_id'
        }]
      },
      dialog: {
        add: false,
        confirm_del: false,
        edit: false
      }
    }
  },
  props: {

  },
  methods: {
    async deleteProducts () {
      const products = await (this.$axios.$delete(`products/delete/${this.manage.del}`));
      await this.loadProducts();
      this.dialog.confirm_del = false;
    },
    async editProducts () {
      const products = await (this.$axios.$post('products/edit', this.form));
      this.loadProducts();
      this.dialog.edit = false;
    },
    async dialogDelProduct (_id) {
      this.manage.del = _id;
      this.dialog.confirm_del = true;
    },
    async getProducts (_id) {
      const products = await (this.$axios.$get(`products/${_id}`));
      this.form = products.result;
    },
    async loadCategory () {
      const category = await (this.$axios.$get("categories"));
      this.select.category = category.result;
    },
    async loadProducts () {
      const login = JSON.parse(Cookies.get("user"));
      await this.loadCategory();
      this.datatable.loading = true;
      this.products = (await this.$axios.$get('products/t/' + login._id)).result;
      this.datatable.loading = false;
    },
    async addProduct () {
      const products = await (this.$axios.$post("products/add", this.form));
      this.loadProducts();
      this.dialog.add = false;
    },
    resetForm () {
      const login = JSON.parse(Cookie.get("user"));
      this.form = {
        pro_stock: null,
        pro_status: true,
        pro_cost: null,
        pro_price: null,
        tra_id: login._id,
        pro_name: null,
        pro_detail: null,
        category_id: null,
      }
    }
  },
}
</script>

<style lang="scss" scoped>
</style>

