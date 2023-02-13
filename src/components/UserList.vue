<template>
  <div class="content-users">
    <div class="user-list">
      <table class="user-list__item" v-if="filteredUser.length">
        <tbody>
          <tr class="tr-user">
            <th>Имя пользователя</th>
            <th>E-mail</th>
            <th>Дата регистрации</th>
            <th>Рейтинг</th>
          </tr>
          <tr class="tr-user" v-for="user in filteredUser" :key="user.id">
            <user-item
              :user="user"
              @open-modal="$emit('open-modal', true, user)"
            />
          </tr>
        </tbody>
      </table>
      <div v-else class="list-empty">Список пользователей пуст!</div>
    </div>
    <pagination-item
      :users="users"
      @page-click="pageClick"
      :pageNumber="pageNumber"
    />
  </div>
</template>
<script>
import PaginationItem from "./PaginationItem.vue";
import UserItem from "./UserItem.vue";
export default {
  components: { UserItem, PaginationItem },
  name: "user-list",
  data() {
    return {
      isActive: {},
      usersPerPage: 5,
      pageNumber: 1,
      usersSort: null,
      pagesCheck: [],
    };
  },
  props: {
    users: {
      type: Array,
      required: true,
    },
    searchUser: {
      type: String,
      required: true,
    },
    pages: {
      type: Array,
      required: true,
    },
  },
  emits: ["user", "open-modal"],
  computed: {
    filteredUser() {
      let from = (this.pageNumber - 1) * this.usersPerPage;
      let to = from + this.usersPerPage;
      return this.users
        .filter((item) => {
          return (
            item.username
              .toLowerCase()
              .includes(this.searchUser.toLowerCase()) ||
            item.email.toLowerCase().includes(this.searchUser.toLowerCase())
          );
        })
        .slice(from, to);
    },
  },
  methods: {
    pageClick(user) {
      this.pageNumber = user;
    },
  },
  mounted() {
    this.pages.forEach((user) => {
      this.pagesCheck.push(user);
    });
  },
  watch: {
    searchUser() {
      if (this.searchUser !== "") {
        this.pageNumber = 1;
      }
    },
  },
};
</script>

<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Inter:wght@400;500;700&family=Quicksand&display=swap");

.content-users {
  width: 961px;
}

.user-list {
  position: relative;
  width: 961px;
  background: #ffffff;
  box-shadow: 0px 18px 15px rgb(148 148 148 / 15%);
  border-top-left-radius: 7px;
  border-top-right-radius: 7px;
  color: #9eaab4;
  padding: 0 16px;
  margin-top: 15px;
}

.user-list__item {
  font-family: "Inter", sans-serif;
  width: 100%;
  text-align: left;
}

.user-list__item th {
  padding: 16px 0;
}

table {
  border-collapse: collapse;
}

tr {
  border-bottom: 1px solid #efefef;
}

.tr-user {
  font-size: 10px;
  line-height: 14px;
  color: #9eaab4;
  font-family: "Inter", sans-serif;
  font-weight: 500;
}

.list-empty {
  padding: 30px;
  color: red;
  font-weight: 500;
  font-size: 20px;
  text-align: center;
}
</style>
