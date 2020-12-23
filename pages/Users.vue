
<template>
  <div>
    <v-row>
      <v-col
        cols="12"
        lg="6"
      >
        <v-card-text>
          <h2>
            <v-icon>mdi-account-search</v-icon> ค้นหาข้อมูลสมาชิก
          </h2>
        </v-card-text>
        <v-card-text>
          <v-text-field
            name="search"
            label="ค้นหาข้อมูล"
            id="txtSearch"
            placeholder="ข้อมูลที่ต้องการค้นหา"
            outlined
            append-icon="mdi-card-search"
            v-model="search"
          ></v-text-field>
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
                    <v-icon>mdi-pencil</v-icon> แก้ไขสมาชิก
                  </span>
                </v-card-title>
                <v-card-text>
                  <v-container>
                    <v-row>
                      <v-col
                        cols="12"
                        sm="6"
                        md="4"
                      >
                        <v-select
                          :items="select.prefix"
                          v-model="form.user_prefix"
                          label="คำนำหน้า"
                        ></v-select>
                      </v-col>
                      <v-col
                        cols="12"
                        sm="6"
                        md="4"
                      >
                        <v-text-field
                          label="ชื่อจริง"
                          hint="ชื่อจริงระบุเป็นภาษาไทย"
                          v-model="form.user_firstname"
                        ></v-text-field>
                      </v-col>
                      <v-col
                        cols="6"
                        sm="6"
                        md="4"
                      >
                        <v-text-field
                          label="ชื่อนามสกุล"
                          hint="นามสกุลระบุเป็นภาษาไทย"
                          v-model="form.user_lastname"
                        ></v-text-field>
                      </v-col>
                      <v-col cols="6">
                        <v-text-field
                          label="ชื่อผู้ใช้งาน*"
                          required
                          v-model="form.user_username"
                        ></v-text-field>
                      </v-col>
                      <v-col cols="6">
                        <v-text-field
                          label="รหัสผ่าน*"
                          type="password"
                          required
                          v-model="form.user_password"
                        ></v-text-field>
                      </v-col>
                      <v-col
                        cols="12"
                        sm="6"
                      >
                        <v-text-field
                          label="เบอร์โทรศัพท์"
                          required
                          v-model="form.user_tel"
                        ></v-text-field>
                      </v-col>
                      <v-col
                        cols="12"
                        sm="6"
                      >
                        <v-select
                          :items="select.role"
                          v-model="form.user_role"
                          label="ระดับผู้ใช้งาน"
                          item-text="text"
                          item-value="value"
                        ></v-select>
                      </v-col>
                      <v-col
                        cols="12"
                        sm="12"
                      >
                        <v-select
                          :items="select.status"
                          v-model="form.user_status"
                          label="สถานะผู้ใช้งาน"
                          item-text="text"
                          item-value="value"
                        ></v-select>
                      </v-col>
                      <v-col
                        cols="12"
                        sm="12"
                      >
                        <h3>ข้อมูลที่อยู่</h3>
                        <v-text-field
                          label="ชื่อร้านค้า"
                          required
                          v-model="form.address.shopname"
                        ></v-text-field>
                      </v-col>
                      <v-col
                        sm="4"
                        cols="6"
                      >
                        <v-text-field
                          label="บ้านเลขที่"
                          required
                          v-model="form.address.house_no"
                        ></v-text-field>
                      </v-col>
                      <v-col
                        sm="4"
                        cols="6"
                      >
                        <v-text-field
                          label="หมู่ที่"
                          required
                          v-model="form.address.moo"
                        ></v-text-field>
                      </v-col>
                      <v-col
                        sm="4"
                        cols="6"
                      >
                        <v-text-field
                          label="ซอย"
                          required
                          v-model="form.address.soi"
                        ></v-text-field>
                      </v-col>
                      <v-col
                        sm="4"
                        cols="6"
                      >
                        <v-text-field
                          label="ถนน"
                          required
                          v-model="form.address.road"
                        ></v-text-field>
                      </v-col>
                      <v-col
                        sm="4"
                        cols="6"
                      >
                        <v-text-field
                          label="ตำบล"
                          required
                          v-model="form.address.sub_district"
                        ></v-text-field>
                      </v-col>
                      <v-col
                        sm="4"
                        cols="6"
                      >
                        <v-text-field
                          label="อำเภอ"
                          required
                          v-model="form.address.district"
                        ></v-text-field>
                      </v-col>
                      <v-col
                        sm="4"
                        cols="6"
                      >
                        <v-text-field
                          label="จังหวัด"
                          required
                          v-model="form.address.province"
                        ></v-text-field>
                      </v-col>
                      <v-col
                        sm="8"
                        cols="6"
                      >
                        <v-text-field
                          label="ที่อยู่ไปรษณีย์"
                          required
                          v-model="form.address.zip_code"
                        ></v-text-field>
                      </v-col>
                    </v-row>
                  </v-container>
                </v-card-text>
                <v-card-actions>
                  <v-spacer></v-spacer>
                  <v-btn
                    color="red darken-1"
                    text
                    @click="dialog.edit = false"
                  >
                    <v-icon left>mdi-close</v-icon>ปิด
                  </v-btn>
                  <v-btn
                    color="blue darken-1"
                    text
                    @click="editUser"
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
                  เพิ่มสมาชิก <v-icon>mdi-plus-box</v-icon>
                </v-btn>
              </template>
              <v-card>
                <v-card-title>
                  <span class="headline">
                    <v-icon>mdi-plus-box</v-icon> เพิ่มสมาชิก
                  </span>
                </v-card-title>
                <v-card-text>
                  <v-container>
                    <v-row>
                      <v-col
                        cols="12"
                        sm="6"
                        md="4"
                      >
                        <v-select
                          :items="select.prefix"
                          v-model="form.user_prefix"
                          label="คำนำหน้า"
                        ></v-select>
                      </v-col>
                      <v-col
                        cols="12"
                        sm="6"
                        md="4"
                      >
                        <v-text-field
                          label="ชื่อจริง"
                          hint="ชื่อจริงระบุเป็นภาษาไทย"
                          v-model="form.user_firstname"
                        ></v-text-field>
                      </v-col>
                      <v-col
                        cols="6"
                        sm="6"
                        md="4"
                      >
                        <v-text-field
                          label="ชื่อนามสกุล"
                          hint="นามสกุลระบุเป็นภาษาไทย"
                          v-model="form.user_lastname"
                        ></v-text-field>
                      </v-col>
                      <v-col cols="6">
                        <v-text-field
                          label="ชื่อผู้ใช้งาน*"
                          required
                          v-model="form.user_username"
                        ></v-text-field>
                      </v-col>
                      <v-col cols="6">
                        <v-text-field
                          label="รหัสผ่าน*"
                          type="password"
                          required
                          v-model="form.user_password"
                        ></v-text-field>
                      </v-col>
                      <v-col
                        cols="12"
                        sm="6"
                      >
                        <v-text-field
                          label="เบอร์โทรศัพท์"
                          required
                          v-model="form.user_tel"
                        ></v-text-field>
                      </v-col>
                      <v-col
                        cols="12"
                        sm="6"
                      >
                        <v-select
                          :items="select.role"
                          v-model="form.user_role"
                          label="ระดับผู้ใช้งาน"
                          item-text="text"
                          item-value="value"
                        ></v-select>
                      </v-col>
                      <v-col
                        cols="12"
                        sm="12"
                      >
                        <v-select
                          :items="select.status"
                          v-model="form.user_status"
                          label="สถานะผู้ใช้งาน"
                          item-text="text"
                          item-value="value"
                        ></v-select>
                      </v-col>
                      <v-col
                        cols="12"
                        sm="12"
                      >
                        <h3>ข้อมูลที่อยู่</h3>
                        <v-text-field
                          label="ชื่อร้านค้า"
                          required
                          v-model="form.address.shopname"
                        ></v-text-field>
                      </v-col>
                      <v-col
                        sm="4"
                        cols="6"
                      >
                        <v-text-field
                          label="บ้านเลขที่"
                          required
                          v-model="form.address.house_no"
                        ></v-text-field>
                      </v-col>
                      <v-col
                        sm="4"
                        cols="6"
                      >
                        <v-text-field
                          label="หมู่ที่"
                          required
                          v-model="form.address.moo"
                        ></v-text-field>
                      </v-col>
                      <v-col
                        sm="4"
                        cols="6"
                      >
                        <v-text-field
                          label="ซอย"
                          required
                          v-model="form.address.soi"
                        ></v-text-field>
                      </v-col>
                      <v-col
                        sm="4"
                        cols="6"
                      >
                        <v-text-field
                          label="ถนน"
                          required
                          v-model="form.address.road"
                        ></v-text-field>
                      </v-col>
                      <v-col
                        sm="4"
                        cols="6"
                      >
                        <v-text-field
                          label="ตำบล"
                          required
                          v-model="form.address.sub_district"
                        ></v-text-field>
                      </v-col>
                      <v-col
                        sm="4"
                        cols="6"
                      >
                        <v-text-field
                          label="อำเภอ"
                          required
                          v-model="form.address.district"
                        ></v-text-field>
                      </v-col>
                      <v-col
                        sm="4"
                        cols="6"
                      >
                        <v-text-field
                          label="จังหวัด"
                          required
                          v-model="form.address.province"
                        ></v-text-field>
                      </v-col>
                      <v-col
                        sm="8"
                        cols="6"
                      >
                        <v-text-field
                          label="ที่อยู่ไปรษณีย์"
                          required
                          v-model="form.address.zip_code"
                        ></v-text-field>
                      </v-col>
                    </v-row>
                  </v-container>
                </v-card-text>
                <v-card-actions>
                  <v-spacer></v-spacer>
                  <v-btn
                    color="red darken-1"
                    text
                    @click="dialog.add = false"
                  >
                    <v-icon left>mdi-close</v-icon>ปิด
                  </v-btn>
                  <v-btn
                    color="blue darken-1"
                    text
                    @click="addUser"
                  >
                    <v-icon left>mdi-content-save</v-icon>บันทึกข้อมูล
                  </v-btn>
                </v-card-actions>
              </v-card>
            </v-dialog>
            <v-btn
              color="primary"
              large
              @click="loadUser"
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
              <v-icon>mdi-clipboard-list</v-icon> รายชื่อสมาชิก
            </h2>
          </v-card-text>
          <v-data-table
            :items="users"
            :loading="datatable.loading"
            :search="search"
            :headers="datatable.headers"
          >
            <template v-slot:item.user_status="{ item }">
              <v-chip
                v-if="item.user_status == true"
                color="success"
              >เปิดใช้งาน</v-chip>
              <v-chip
                v-else
                color="red"
                class="white--text"
              >รอดำเนินการ</v-chip>
            </template>
            <template v-slot:item.user_role="{ item }">
              {{ item.user_role == 'admin' ? 'ผู้ดูแลระบบ' : item.user_role == 'trainer' ? 'สมาชิก (เจ้าบ่อ)' : 'สมาชิก'
          }}
            </template>
            <template v-slot:item._id="{ item }">
              <v-btn
                color="success"
                icon
                @click="getUser(item._id);dialog.edit = true;"
              >
                <v-icon>mdi-pencil</v-icon>
              </v-btn>
              <v-btn
                color="red"
                icon
                @click="dialogDelUser(item._id)"
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
                      @click="deleteUser();"
                    >
                      ยืนยัน
                    </v-btn>
                  </v-card-actions>
                </v-card>
              </v-dialog>
            </v-row>
          </template>
        </v-card>
      </v-col>
    </v-row>
  </div>
</template>

<script>
import Cookies from 'vue-cookie';
export default {
  middleware: 'authen',
  name: 'Users',
  async created () {
    await this.loadUser();
  },
  data () {
    return {
      users: [],
      search: null,
      manage: {
        del: null
      },
      select: {
        prefix: ['นาย', 'นาง', 'นางสาว'],
        role: [{ text: 'ผู้ดูแลระบบ', value: 'admin' }, { text: 'เจ้าบ่อ', value: 'trainer' }, { text: 'สมาชิกบ่อ', value: 'user' }],
        status: [{ text: 'เปิดใช้งาน', value: true }, { text: 'ปิดใช้งาน', value: false }]
      },
      form: {
        reference_user_id: null,
        user_prefix: null,
        user_firstname: null,
        user_surname: null,
        user_username: null,
        user_password: null,
        user_role: 'trainer',
        user_status: true,
        user_tel: null,
        address: {
          shopname: null,
          house_no: null,
          moo: null,
          soi: null,
          road: null,
          sub_district: null,
          district: null,
          province: null,
          zip_code: null
        }
      },
      datatable: {
        loading: false,
        headers: [
          { text: 'ชื่อผู้ใช้งาน', value: 'user_username' },
          { text: 'ชื่อจริง', value: 'user_firstname' },
          { text: 'นามสกุล', value: 'user_lastname' },
          { text: 'เบอร์โทรศัพท์', value: 'user_tel' },
          { text: 'สถานะ', value: 'user_status' },
          { text: 'ระดับ', value: 'user_role' },
          { text: '', value: '_id' }
        ]
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
    async editUser () {
      const users = await (this.$axios.$post(`users/edit`, this.form));
      this.result = users.result;
      await this.loadUser();
      this.resetForm();
      this.dialog.edit = false;
    },
    async getUser (_id) {
      const users = await (this.$axios.$get(`users/${_id}`));
      this.form = users.result;
    },
    async dialogDelUser (_id) {
      this.manage.del = _id;
      this.dialog.confirm_del = true;
    },
    async deleteUser () {
      const users = await (this.$axios.$delete(`users/delete/${this.manage.del}`))
      this.dialog.confirm_del = false;
      this.loadUser();
    },
    async loadUser () {
      const login = JSON.parse(Cookies.get("user"));
      this.form.reference_user_id = login._id;
      const users = await (this.$axios.$get("/users/t/" + login._id));
      this.users = users.result;
    },
    async addUser () {
      const users = await (this.$axios.$post("/users/add", this.form));
      await this.loadUser();
      this.dialog.add = false;
      this.resetForm();
      this.result = users.result;
    },
    resetForm () {
      this.form = {
        reference_user_id: null,
        user_prefix: null,
        user_firstname: null,
        user_surname: null,
        user_username: null,
        user_password: null,
        user_tel: null,
        user_status: true,
        user_role: 'trainer',
        address: {
          shopname: null,
          house_no: null,
          moo: null,
          soi: null,
          road: null,
          sub_district: null,
          district: null,
          province: null,
          zip_code: null
        }
      };
    }
  },
}
</script>

<style lang="scss" scoped>
</style>

