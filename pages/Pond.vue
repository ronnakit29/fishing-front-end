
<template>
  <div>
    <v-row>
      <v-col
        cols="12"
        lg="6"
      >
        <v-card-text>
          <h2>
            <v-icon>mdi-card-search</v-icon> ค้นหาข้อมูลบ่อเลี้ยงปลา
          </h2>
        </v-card-text>
        <v-card-text>
          <v-autocomplete
            name="search"
            label="เลือกลูกบ่อของคุณ"
            id="txtSearch"
            placeholder="ข้อมูลที่ต้องการค้นหา"
            :items="users"
            item-text="user_fullname"
            item-value="_id"
            v-model="search"
            @change="loadPond"
            outlined
          >
          </v-autocomplete>
        </v-card-text>
      </v-col>
      <v-col
        cols="12"
        lg="12"
      >
        <v-divider></v-divider>
        <v-card flat>
          <v-card-text>
            <h2>
              <v-icon>mdi-clipboard-list</v-icon> รายการบ่อเลี้ยงปลา
            </h2>
          </v-card-text>
          <v-card-text>
            <v-data-table
              :items="pond"
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
import Cookies from 'vue-cookie';
export default {
  name: 'Pond',
  async created () {
    await this.loadUser();
  },
  data () {
    return {
      pond: [],
      users: [],
      search: null,
      manage: {
        del: null
      },
      select: {},
      form: {},
      datatable: {
        loading: false,
        headers: [{ text: 'ชื่อบ่อ', value: 'p_name' }, { text: 'ประเภทปลา', value: 'fish_type_data.fish_name' }, { text: 'จำนวนปลา', value: 'p_number_fish' }, { text: 'จำนวนปลาตาย', value: 'p_number_dead' }, { text: 'กว้าง (เมตร)', value: 'p_width' }, { text: 'ยาว (เมตร)', value: 'p_length' }, { text: 'วันที่เริ่ม', value: 'p_date' }]
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
    async loadPond () {
      const pond = await (this.$axios.$get("ponds/u/" + this.search));
      this.pond = pond.result;
      this.pond.forEach(item => {
        item.p_date = new Date(item.p_date).toLocaleString('th-TH');
      })
    },
    async loadUser () {
      const login = JSON.parse(Cookies.get("user"));
      const users = await (this.$axios.$get("/users/t/" + login._id));
      this.users = users.result;
      this.users.forEach(item => {
        item.user_fullname = item.user_firstname + " " + item.user_lastname;
      })
    }
  },
}
</script>

<style lang="scss" scoped>
</style>

