
<template>
  <div>
    <v-row>
      <v-col
        cols="12"
        lg="6"
      >
        <v-card-text>
          <h2>
            <v-icon>mdi-card-search</v-icon> ค้นหาข้อมูลช่วงเวลาการเลี้ย
          </h2>
        </v-card-text>
        <v-card-text>
          <v-autocomplete
            name="search"
            label="ประเภทปลา"
            id="txtSearch"
            placeholder=""
            outlined
            :items="fish"
            item-text="fish_name"
            item-value="_id"
            v-model="search"
            ref="search"
            @change="loadTimelineFish"
          >
          </v-autocomplete>
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
                    <v-icon>mdi-plus-box</v-icon> เพิ่มช่วงเวลาการเลี้ยง
                  </span>
                </v-card-title>
                <v-card-text>
                  <v-autocomplete
                    label="ผู้เลี้ยง"
                    :items="users"
                    item-text="user_fullname"
                    item-value="_id"
                    v-model="form.user_id"
                  ></v-autocomplete>
                  <v-text-field
                    label="วันที่"
                    required
                    v-model="form.timeline_day"
                    type="number"
                  ></v-text-field>
                  <v-text-field
                    label="จะเกิดอะไรขึ้น?"
                    required
                    v-model="form.timeline_title"
                  ></v-text-field>
                  <v-text-field
                    label="คำอธิบาย"
                    required
                    v-model="form.timeline_detail"
                  ></v-text-field>
                  <v-text-field
                    label="วิดีโอแนะนำ (Youtube Link)"
                    required
                    v-model="form.timeline_youtube"
                  ></v-text-field>
                  <v-select
                    label="ประเภทปลา"
                    required
                    :items="fish"
                    item-value="_id"
                    item-text="fish_name"
                    v-model="form.fish_type"
                  ></v-select>
                  <v-autocomplete
                    label="สินค้าแนะนำตามช่วงเวลา"
                    item-text="pro_name"
                    item-value="_id"
                    :items="products"
                    multiple
                    v-model="form.product_recommend"
                  ></v-autocomplete>
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
                    @click="editTimeline();dialog.edit = false"
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
                  เพิ่มช่วงเวลาการเลี้ยง <v-icon>mdi-plus-box</v-icon>
                </v-btn>
              </template>
              <v-card>
                <v-card-title>
                  <span class="headline">
                    <v-icon>mdi-plus-box</v-icon> เพิ่มช่วงเวลาการเลี้ยง
                  </span>
                </v-card-title>
                <v-card-text>
                  <v-autocomplete
                    label="ผู้เลี้ยง"
                    :items="users"
                    item-text="user_fullname"
                    item-value="_id"
                    v-model="form.user_id"
                  ></v-autocomplete>
                  <v-text-field
                    label="วันที่"
                    required
                    v-model="form.timeline_day"
                    type="number"
                  ></v-text-field>
                  <v-text-field
                    label="จะเกิดอะไรขึ้น?"
                    required
                    v-model="form.timeline_title"
                  ></v-text-field>
                  <v-text-field
                    label="คำอธิบาย"
                    required
                    v-model="form.timeline_detail"
                  ></v-text-field>
                  <v-text-field
                    label="วิดีโอแนะนำ (Youtube Link)"
                    required
                    v-model="form.timeline_youtube"
                  ></v-text-field>
                  <v-select
                    label="ประเภทปลา"
                    required
                    :items="fish"
                    item-value="_id"
                    item-text="fish_name"
                    v-model="form.fish_type"
                  ></v-select>
                  <v-autocomplete
                    label="สินค้าแนะนำตามช่วงเวลา"
                    item-text="pro_name"
                    item-value="_id"
                    :items="products"
                    multiple
                    v-model="form.product_recommend"
                  ></v-autocomplete>
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
                    @click="addTimeline"
                  >
                    <v-icon left>mdi-content-save</v-icon>บันทึกข้อมูล
                  </v-btn>
                </v-card-actions>
              </v-card>
            </v-dialog>
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
              <v-icon>mdi-clipboard-list</v-icon> รายการช่วงเวลาการเลี้ยง
            </h2>
          </v-card-text>
          <v-data-table
            :items="timeline"
            :loading="datatable.loading"
            :headers="datatable.headers"
          >
            <template v-slot:item._id="{ item }">
              <v-btn
                color="success"
                icon
                @click="getTimeline(item._id);dialog.edit = true;"
              >
                <v-icon>mdi-pencil</v-icon>
              </v-btn>
              <v-btn
                color="red"
                icon
                @click="dialogDelTimeline(item._id)"
              >
                <v-icon>mdi-trash-can</v-icon>
              </v-btn>
            </template>
          </v-data-table>
        </v-card>
      </v-col>
    </v-row>
    <template>
      <v-row justify="center">
        <v-dialog
          v-model="dialog.confirm_del"
          max-width="320"
        >
          <v-card>
            <v-card-title class="headline">
              คุณต้องการลบใช่หรือไม่?
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
                @click="deleteTimeline"
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
  name: 'Timeline',
  async created () {
    await this.loadTimeline();
  },
  data () {
    return {
      users: [],
      products: [],
      fish: [],
      timeline: [],
      search: null,
      manage: {
        del: null
      },
      select: {},
      form: {
        product_recommend: [],
        user_id: null,
        timeline_day: null,
        timeline_title: null,
        timeline_detail: null,
        timeline_youtube: null,
        fish_type: null,
      },
      datatable: {
        loading: false,
        headers: [
          { text: 'วันที่', value: 'timeline_day' },
          { text: 'จะเกิดอะไรขึ้น', value: 'timeline_title' },
          { text: 'คำอธิบาย', value: 'timeline_detail' },
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
    async loadTimelineFish () {
      this.form.fish_type = this.search;
      const timeline = await (this.$axios.$get("timeline/t/5fdf4cb8ccaa3d167cf4daa6/f/" + this.search));
      this.timeline = timeline.result;
      console.log(timeline.result)
    },
    async editTimeline () {
      const timeline = await (this.$axios.$post(`timeline/edit`, this.form));
      this.result = timeline.result;
      await this.loadTimelineFish();
      this.resetForm();
      this.dialog.edit = false;
    },
    async getTimeline (_id) {
      const timeline = await (this.$axios.$get(`timeline/${_id}`));
      this.form = timeline.result;
    },
    async dialogDelTimeline (_id) {
      this.manage.del = _id;
      this.dialog.confirm_del = true;
    },
    async deleteTimeline () {
      const timeline = await (this.$axios.$delete(`timeline/delete/${this.manage.del}`))
      this.dialog.confirm_del = false;
      this.loadTimelineFish();
    },
    async loadTimeline () {
      const users = await (this.$axios.$get("/users"));
      const products = await (this.$axios.$get("/products"));
      const fish = await (this.$axios.$get("/fish"));
      this.fish = fish.result;
      this.products = products.result;
      this.users = users.result;
      this.users.forEach(item => {
        item.user_fullname = item.user_firstname + " " + item.user_lastname;
      })
    },
    async addTimeline () {
      const timeline = await (this.$axios.$post("/timeline/add", this.form));
      await this.loadTimeline();
      this.dialog.add = false;
      await this.loadTimelineFish();
      this.resetForm();
      this.result = timeline.result;
    },
    resetForm () {
      this.form = {
        product_recommend: [],
        user_id: null,
        timeline_day: null,
        timeline_title: null,
        timeline_detail: null,
        timeline_youtube: null,
        fish_type: null,
      };
    }
  },
}
</script>

<style lang="scss" scoped>
</style>

