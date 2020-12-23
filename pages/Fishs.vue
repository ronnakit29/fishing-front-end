
<template>
  <div>
    <v-row>
      <v-col
        cols="12"
        lg="6"
      >
        <v-card-text>
          <h2>
            <v-icon>mdi-card-search</v-icon> ค้นหาข้อมูลประเภทปลา
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
              <v-card>
                <v-card-title>
                  <span class="headline">
                    <v-icon>mdi-plus-box</v-icon> เพิ่มประเภทปลา
                  </span>
                </v-card-title>
                <v-card-text>
                  <v-autocomplete
                    label="ผู้จัดการ"
                    :items="users"
                    item-text="user_fullname"
                    item-value="_id"
                    v-model="form.user_id"
                    :disabled="true"
                  >
                  </v-autocomplete>
                  <v-text-field
                    label="ชื่อประเภทปลา"
                    required
                    v-model="form.fish_name"
                  ></v-text-field>
                  <v-text-field
                    label="ระยะเวลาการเติบโต"
                    required
                    v-model="form.fish_growth_time"
                  ></v-text-field>
                  <v-text-field
                    label="ราคาลูกปลา (ต่อ 1 ตัว)"
                    required
                    v-model="form.fish_price"
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
                    @click="editFish"
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
                  เพิ่มประเภทปลา <v-icon>mdi-plus-box</v-icon>
                </v-btn>
              </template>
              <v-card>
                <v-card-title>
                  <span class="headline">
                    <v-icon>mdi-plus-box</v-icon> เพิ่มประเภทปลา
                  </span>
                </v-card-title>
                <v-card-text>
                  <v-autocomplete
                    label="ผู้จัดการ"
                    :items="users"
                    item-text="user_fullname"
                    item-value="_id"
                    v-model="form.user_id"
                    :disabled="true"
                  >
                  </v-autocomplete>
                  <v-text-field
                    label="ชื่อประเภทปลา"
                    required
                    v-model="form.fish_name"
                  ></v-text-field>
                  <v-text-field
                    label="ระยะเวลาการเติบโต"
                    required
                    v-model="form.fish_growth_time"
                  ></v-text-field>
                  <v-text-field
                    label="ราคาลูกปลา (ต่อ 1 ตัว)"
                    required
                    v-model="form.fish_price"
                  ></v-text-field>
                </v-card-text>
                <v-card-actions>
                  <v-spacer></v-spacer>
                  <v-btn
                    color="red darken-1"
                    text
                    @click="dialog.add = false"
                  >
                    <v-icon left>mdi-close</v-icon> ยกเลิก
                  </v-btn>
                  <v-btn
                    color="blue darken-1"
                    text
                    @click="addFish"
                  >
                    <v-icon left>mdi-content-save</v-icon>บันทึกข้อมูล
                  </v-btn>
                </v-card-actions>
              </v-card>
            </v-dialog>
            <v-btn
              color="primary"
              large
              @click=""
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
              <v-icon>mdi-clipboard-list</v-icon> รายการประเภทปลา
            </h2>
          </v-card-text>
          <v-card-text>

            <v-data-table
              :items="fish"
              :loading="datatable.loading"
              :search="search"
              :headers="datatable.headers"
            >
              <template v-slot:item._id="{ item }">
                <v-btn
                  color="success"
                  icon
                  @click="getFish(item._id);dialog.edit = true;"
                >
                  <v-icon>mdi-pencil</v-icon>
                </v-btn>
                <v-btn
                  color="red"
                  icon
                  @click="dialogDelFish(item._id)"
                >
                  <v-icon>mdi-trash-can</v-icon>
                </v-btn>
              </template>
            </v-data-table>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
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
                @click="deleteFish"
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
import Cookies from 'vue-cookie';
export default {
  middleware: 'authen',
  name: 'Fishs',
  async created () {
    await this.loadFish();
  },
  data () {
    return {
      users: [],
      fish: [],
      search: null,
      manage: {
        del: null
      },
      select: {},
      form: {
        user_id: null,
        fish_name: null,
        fish_growth_time: null,
        fish_price: null
      },
      datatable: {
        loading: false,
        headers: [{ text: 'ชื่อประเภท', value: 'fish_name' }, { text: 'ระยะเวลาเติบโต', value: 'fish_growth_time' }, { text: 'ราคาลูกปลา (ตัว)', value: 'fish_price' }, { text: '', value: '_id' }]
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
    async editFish () {
      const fish = await (this.$axios.$post(`fish/edit`, this.form));
      this.result = fish.result;
      await this.loadFish();
      this.resetForm();
      this.dialog.edit = false;
    },
    async getFish (_id) {
      const fish = await (this.$axios.$get(`fish/${_id}`));
      this.form = fish.result;
    },
    async dialogDelFish (_id) {
      this.manage.del = _id;
      this.dialog.confirm_del = true;
    },
    async deleteFish () {
      const fish = await (this.$axios.$delete(`fish/delete/${this.manage.del}`))
      this.dialog.confirm_del = false;
      this.loadFish();
    },
    async loadFish () {
      const login = JSON.parse(Cookies.get("user"));
      this.form.user_id = login._id;
      console.log(login);
      const users = await (this.$axios.$get("/users"));
      const fish = await (this.$axios.$get(`/fish/t/${login._id}`));
      this.users = users.result;
      this.users.forEach(item => {
        item.user_fullname = item.user_firstname + " " + item.user_lastname;
      })
      this.fish = fish.result;
    },
    async addFish () {
      const fish = await (this.$axios.$post("/fish/add", this.form));
      await this.loadFish();
      this.dialog.add = false;
      this.resetForm();
      this.result = fish.result;
    },
    resetForm () {
      const login = JSON.parse(Cookies.get("user"));
      this.form = {
        user_id: login._id,
        fish_name: null,
        fish_growth_time: null,
        fish_price: null
      };
    }
  },
}
</script>

<style lang="scss" scoped>
</style>

