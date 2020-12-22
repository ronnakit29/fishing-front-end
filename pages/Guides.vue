
<template>
  <div>
    <v-row>
      <v-col
        cols="12"
        lg="6"
      >
        <v-card-text>
          <h2>
            <v-icon>mdi-card-search</v-icon> ค้นหาข้อมูลคำแนะนำ
          </h2>
        </v-card-text>
        <v-card-text>
          <v-text-field
            name="search"
            label="ค้นหาข้อมูล"
            id="txtSearch"
            placeholder="ข้อมูลที่ต้องการค้นหา"
            outlined
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
                    <v-icon>mdi-plus-box</v-icon> เพิ่มข้อมูลคู่มือคำแนะนำ
                  </span>
                </v-card-title>
                <v-card-text>
                  <v-autocomplete
                    label="ผู้อัพโหลด"
                    :items="users"
                    item-text="user_fullname"
                    item-value="_id"
                    v-model="form.user_id"
                  ></v-autocomplete>
                  <v-text-field
                    label="หัวข้อ"
                    required
                    v-model="form.guide_header"
                  ></v-text-field>
                  <v-text-field
                    label="คำอธิบาย"
                    required
                    v-model="form.guide_detail"
                  ></v-text-field>
                  <v-text-field
                    label="ลำดับความสำคัญ"
                    required
                    v-model="form.guide_priority"
                    type="number"
                  ></v-text-field>
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
                    @click="editGuides"
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
                  เพิ่มข้อมูลคู่มือคำแนะนำ <v-icon>mdi-plus-box</v-icon>
                </v-btn>
              </template>
              <v-card>
                <v-card-title>
                  <span class="headline">
                    <v-icon>mdi-plus-box</v-icon> เพิ่มข้อมูลคู่มือคำแนะนำ
                  </span>
                </v-card-title>
                <v-card-text>
                  <v-autocomplete
                    label="ผู้อัพโหลด"
                    :items="users"
                    item-text="user_fullname"
                    item-value="_id"
                    v-model="form.user_id"
                  ></v-autocomplete>
                  <v-text-field
                    label="หัวข้อ"
                    required
                    v-model="form.guide_header"
                  ></v-text-field>
                  <v-text-field
                    label="คำอธิบาย"
                    required
                    v-model="form.guide_detail"
                  ></v-text-field>
                  <v-text-field
                    label="ลำดับความสำคัญ"
                    required
                    v-model="form.guide_priority"
                    type="number"
                  ></v-text-field>
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
                    @click="addGuides"
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
              <v-icon>mdi-clipboard-list</v-icon> รายการคำแนะนำ
            </h2>
          </v-card-text>
          <v-card-text>
            <v-data-table
              :items="guides"
              :loading="datatable.loading"
              :search="search"
              :headers="datatable.headers"
            >
              <template v-slot:item.guide_detail="{ item }">
                <span
                  class="text-truncate"
                  style="word-wrap: break-word;max-width:400px;display:block"
                >{{ item.guide_detail }}</span>
              </template>
              <template v-slot:item._id="{ item }">
                <v-btn
                  color="success"
                  icon
                  @click="getGuides(item._id);dialog.edit = true;"
                >
                  <v-icon>mdi-pencil</v-icon>
                </v-btn>
                <v-btn
                  color="red"
                  icon
                  @click="dialogDelGuides(item._id)"
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
                @click="deleteGuides"
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
export default {
  name: 'Guides',
  async created () {
    await this.loadGuides();
  },
  data () {
    return {
      guides: [],
      search: null,
      manage: {
        del: null
      },
      select: {},
      form: {
        user_id: null,
        guide_priority: null,
        guide_header: null,
        guide_detail: null,
      },
      datatable: {
        loading: false,
        headers: [{ text: 'หัวข้อ', value: 'guide_header' }, { text: 'คำอธิบาย', value: 'guide_detail' }, { text: '', value: '_id' }]
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
    async editGuides () {
      const guides = await (this.$axios.$post(`guides/edit`, this.form));
      this.result = guides.result;
      await this.loadGuides();
      this.resetForm();
      this.dialog.edit = false;
    },
    async getGuides (_id) {
      const guides = await (this.$axios.$get(`guides/${_id}`));
      this.form = guides.result;
    },
    async dialogDelGuides (_id) {
      this.manage.del = _id;
      this.dialog.confirm_del = true;
    },
    async deleteGuides () {
      const guides = await (this.$axios.$delete(`guides/delete/${this.manage.del}`))
      this.dialog.confirm_del = false;
      this.loadGuides();
    },
    async loadGuides () {
      const users = await (this.$axios.$get("/users"));
      const guides = await (this.$axios.$get("/guides"));
      this.guides = guides.result;
      this.users = users.result;
      this.users.forEach(item => {
        item.user_fullname = item.user_firstname + " " + item.user_lastname;
      })
    },
    async addGuides () {
      const guides = await (this.$axios.$post("/guides/add", this.form));
      await this.loadGuides();
      this.dialog.add = false;
      this.resetForm();
      this.result = guides.result;
    },
    resetForm () {
      this.form = {
        user_id: null,
        guide_priority: null,
        guide_header: null,
        guide_detail: null,
      };
    }
  },
}
</script>

<style lang="scss" scoped>
</style>

