<template>
  <div class="content-users">
    <div class="sort-user">
      <div>Сортировка:</div>
      <div class="tab-select">
        <div
          @click="sortByDateRegistr('first')"
          :class="{ 'active-sort': isActive['first'] }"
          class="sort-user__selection"
        >
          Дата регистрации
        </div>
        <div
          @click="sortByRating('second')"
          :class="{ 'active-sort': isActive['second'] }"
          class="sort-user__selection"
        >
          Рейтинг
        </div>
      </div>
    </div>
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
    activeClassSort: {
      type: Boolean,
      required: true,
    },
  },
  emits: ["user", "open-modal", "sort-data-registr", "sort-rating"],
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
    activeSort(item) {
      this.isActive = { [item]: true };
    },
    pageClick(user) {
      this.pageNumber = user;
    },
    sortByDateRegistr(item) {
      this.activeSort(item);
      this.$emit("sort-data-registr", this.users);
    },
    sortByRating(item) {
      this.activeSort(item);
      this.$emit("sort-rating", this.users);
    },
  },
  watch: {
    activeClassSort() {
      if (this.activeClassSort === false) {
        this.isActive = {};
      }
    },
  },
};
</script>

<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Inter:wght@400;500;700&family=Quicksand&display=swap");

.content-users {
  width: 961px;
  margin-top: 72px;
}

.sort-user {
  display: flex;
  justify-content: space-between;
  width: 240px;
  font-family: "Inter", sans-serif;
  font-weight: 500;
  font-size: 11px;
  line-height: 14px;
  color: #9eaab4;
}

.tab-select {
  display: flex;
  width: 100%;
  justify-content: space-evenly;
}

.sort-user__selection {
  cursor: pointer;
  border-bottom: 1.7px #9eaab4 dashed;
}

.active-sort {
  color: #333333;
  border-bottom: 1.7px #333333 dashed;
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
