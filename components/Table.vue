<template>
  <el-container direction="vertical">
    <el-row :gutter="20">
      <el-col :span="16"><h3>Список пользователей</h3></el-col>
      <el-col :span="8"><h3>События</h3></el-col>
    </el-row>

    <el-row :gutter="20">
      <el-col :span="16"
        ><el-table
          v-loading="users.length === 0"
          :data="users"
          style="width: 100%"
        >
          <el-table-column label="ID" width="60">
            <template slot-scope="scope">
              <span style="margin-left: 10px">{{ scope.row.id }}</span>
            </template>
          </el-table-column>

          <el-table-column label="Дата" width="180">
            <template slot-scope="scope">
              <span style="margin-left: 10px">{{
                new Date(scope.row.ctime).toLocaleString('ru-ru', {
                  dateStyle: 'short',
                  timeStyle: 'short',
                })
              }}</span>
            </template>
          </el-table-column>
          <el-table-column label="Имя" width="180">
            <template slot-scope="scope">
              <p>{{ scope.row.name }}</p>
            </template>
          </el-table-column>
          <el-table-column label="Роль" width="180">
            <template slot-scope="scope">
              <p>{{ scope.row.role }}</p>
            </template>
          </el-table-column>
          <el-table-column label="Действия">
            <template slot-scope="scope">
              <el-button
                size="mini"
                type="danger"
                @click="handleDelete(scope.$index, scope.row)"
                >Удалить</el-button
              >
            </template>
          </el-table-column>
        </el-table>
        <el-pagination
          background
          @current-change="handleCurrentChange"
          :current-page.sync="page"
          :page-size="per_page"
          layout="prev, pager, next"
          :total="total"
        >
        </el-pagination>
      </el-col>
      <el-col :span="8"
        ><el-table
          v-loading="socketData.length === 0"
          :data="socketData"
          style="width: 100%"
        >
          <el-table-column label="Дата" width="180">
            <template slot-scope="scope">
              <span style="margin-left: 10px">{{
                new Date(scope.row.ctime).toLocaleString('ru-ru', {
                  dateStyle: 'short',
                  timeStyle: 'short',
                })
              }}</span>
            </template>
          </el-table-column>
          <el-table-column label="Событие" width="180">
            <template slot-scope="scope">
              <span style="margin-left: 10px">{{ scope.row.event }}</span>
            </template>
          </el-table-column>
        </el-table></el-col
      >
    </el-row>
  </el-container>
</template>

<script>
export default {
  data() {
    return {
      users: [],
      limit: null,
      total: 0,
      page: 0,
      per_page: null,
      offset: null,
      socketData: [],
    }
  },

  mounted() {
    let connection = new WebSocket('wss://test.relabs.ru/event')
    connection.onmessage = (event) => {
      this.socketData.push(JSON.parse(event.data))
    }
  },

  created() {
    this.handleCurrentChange(this.page)
  },

  methods: {
    handleDelete(index, row) {
      this.users.splice(index, 1)
    },
    async handleCurrentChange(pageNumber) {
      const response = await fetch(
        `https://test.relabs.ru/api/users/list?page=${pageNumber}`
      ).then((res) => res.json())
      console.log(response)
      this.users = response.items
      this.limit = response.limit
      this.per_page = response.per_page
      this.total = response.total
      this.offset = response.offset
    },
  },
}
</script>

<style>
.el-row {
  margin-bottom: 10px;
}

h3 {
  text-align: center;
}
</style>
