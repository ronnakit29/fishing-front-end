
<template>
  <div>
    <v-row>
      <v-col
        cols="12"
        lg="6"
      >
        <v-card-text>
          <h2>
            <v-icon>mdi-card-search</v-icon> ค้นหาข้อมูลประเภทสินค้า
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
                  เพิ่มประเภทสินค้า <v-icon>mdi-plus-box</v-icon>
                </v-btn>
              </template>
              <v-card>
                <v-card-title>
                  <span class="headline">
                    <v-icon>mdi-plus-box</v-icon> เพิ่มประเภทสินค้า
                  </span>
                </v-card-title>
                <v-card-text>
                  <v-text-field
                    label="ชื่อประเภท"
                    required
                    v-model="form.category_text"
                  ></v-text-field>
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
                    @click="addCategories"
                  >
                    <v-icon left>mdi-content-save</v-icon>บันทึกข้อมูล
                  </v-btn>
                </v-card-actions>
              </v-card>
            </v-dialog>
            <v-btn
              color="info"
              large
              @click="loadCategories"
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
              <v-icon>mdi-clipboard-list</v-icon> รายการประเภทสินค้า
            </h2>
          </v-card-text>
          <v-card-text>
            <v-data-table
              :items="categories"
              :loading="datatable.loading"
              :search="search"
              :headers="datatable.headers"
            >
              <template v-slot:item._id="{ item }">
                <div class="float-right">
                  <v-btn
                    color="success"
                    icon
                    @click="getCategories(item._id);dialog.edit = true;"
                  >
                    <v-icon>mdi-pencil</v-icon>
                  </v-btn>
                  <v-btn
                    color="red"
                    icon
                    @click="dialogDelCategories(item._id)"
                  >
                    <v-icon>mdi-trash-can</v-icon>
                  </v-btn>
                </div>

              </template>
            </v-data-table>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
    <v-dialog
      v-model="dialog.edit"
      max-width="600px"
    >
      <template v-slot:activator="{ on, attrs }">
      </template>
      <v-card>
        <v-card-title>
          <span class="headline">
            <v-icon>mdi-plus-box</v-icon> แก้ไขประเภทสินค้า
          </span>
        </v-card-title>
        <v-card-text>
          <v-text-field
            label="ชื่อประเภท"
            required
            v-model="form.category_text"
          ></v-text-field>
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
            @click="editCategories"
          >
            <v-icon left>mdi-content-save</v-icon>บันทึกข้อมูล
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
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
              กด "ยืนยัน" เพื่อลบข้อมูล
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
                @click="deleteCategories"
              >
                ยืนยัน
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
      </v-row>
    </template>
  </div>
</template>

<script>
import moduleName from 'vue-cookie';
export default {
  name: 'Categorys',
  middleware: 'authen',
  async created () {
    await this.loadCategories();
  },
  data () {
    return {
      categories: [],
      search: null,
      manage: {
        del: null
      },
      select: {},
      form: {
        category_text: null,
      },
      datatable: {
        loading: false,
        headers: [{
          text: 'ชื่อประเภทสินค้า', value: 'category_text'
        }, {
          text: '', value: '_id'
        }]
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
    async editCategories () {
      const categories = await (this.$axios.$post(`categories/edit`, this.form));
      this.result = categories.result;
      await this.loadCategories();
      this.resetForm();
      this.dialog.edit = false;
    },
    async getCategories (_id) {
      const categories = await (this.$axios.$get(`categories/${_id}`));
      this.form = categories.result;
    },
    async dialogDelCategories (_id) {
      this.manage.del = _id;
      this.dialog.confirm_del = true;
    },
    async deleteCategories () {
      const categories = await (this.$axios.$delete(`categories/delete/${this.manage.del}`))
      this.dialog.confirm_del = false;
      this.loadCategories();
    },
    async loadCategories () {
      const categories = await (this.$axios.$get("/categories"));
      this.categories = categories.result;
    },
    async addCategories () {
      const categories = await (this.$axios.$post("/categories/add", this.form));
      await this.loadCategories();
      this.dialog.add = false;
      this.resetForm();
      this.result = categories.result;
    },
    resetForm () {
      this.form = { category_text: null, };
    }
  },
}
</script>

<style lang="scss" scoped>
</style>

