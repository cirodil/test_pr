<template>
  <el-container direction="vertical">
    <el-row :gutter="20">
      <el-col :span="16"><h3>Список пользователей</h3></el-col>
      <el-col :span="8"><h3>События</h3></el-col>
    </el-row>

    <el-row :gutter="20">
      <el-col :span="16"
        ><el-table v-loading="loading" :data="users" style="width: 100%">
          <el-table-column label="ID" width="60">
            <template slot-scope="scope">
              <span style="margin-left: 10px">{{ scope.row.id }}</span>
            </template>
          </el-table-column>

          <el-table-column label="Дата" width="180">
            <template slot-scope="scope">
              <span style="margin-left: 10px">{{
                formatDate(scope.row.ctime)
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
          class="pagination"
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
          v-loading="!socketData.length"
          :data="socketData"
          style="width: 100%"
        >
          <el-table-column label="Дата" width="180">
            <template slot-scope="scope">
              <span style="margin-left: 10px">{{
                formatDate(scope.row.ctime)
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

    <el-alert v-if="errorMessage" :title="errorMessage" type="error" />
  </el-container>
</template>

<script>
export default {
  data() {
    return {
      users: [],
      limit: 5,
      total: 0,
      page: 1,
      per_page: null,
      offset: null,
      loading: false,
      errorMessage: null,
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

    formatDate(ctime) {
      return new Date(ctime * 1000).toLocaleString('ru-ru', {
        dateStyle: 'short',
        timeStyle: 'short',
      })
    },

    async handleCurrentChange(pageNumber) {
      this.errorMessage = null
      this.loading = true

      try {
        const response = await fetch(
          `https://test.relabs.ru/api/users/list?limit=${this.limit}&offset=${
            this.limit * (pageNumber - 1)
          }`
        )

        const data = await response.json()
        const { items, per_page, total, offset } = data
        this.users = items
        this.per_page = per_page
        this.total = total
        this.offset = offset
      } catch (error) {
        this.errorMessage = error.message
      }

      this.loading = false
    },
  },
}
</script>

<style>
.el-row {
  margin-bottom: 10px;
}
.pagination {
  margin-top: 20px;
  text-align: center;
}
h3 {
  text-align: center;
}
</style>
