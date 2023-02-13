<template>
  <div class="main-container">
    <div>
      <h2 class="title">Список пользователей</h2>
    </div>
    <div class="form-search">
      <div class="logo">
        <svg
          class="search-logo"
          xmlns="http://www.w3.org/2000/svg"
          width="800"
          height="800"
          viewBox="0 0 1920 1920"
        >
          <path
            fill-rule="evenodd"
            d="M1458.948 1305.626c104.637-136.95 167.527-307.187 167.527-492.388C1626.475 364.764 
						1261.711 0 813.238 0 364.764 0 0 364.764 0 813.238c0 448.473 364.764 813.237 813.238 
						813.237 185.201 0 355.547-62.89 492.496-167.527L1766.678 1920 1920 1766.678l-461.052-461.052Zm-645.71 
						103.986c-328.874 0-596.375-267.61-596.375-596.374 0-328.765 267.501-596.375 596.375-596.375 328.873 0 
						596.374 267.61 596.374 596.375s-267.501 596.374-596.374 596.374Z"
          />
        </svg>
      </div>
      <input
        class="input"
        type="text"
        placeholder="Поиск по имени или e-mail"
        v-model="searchUser"
      />
      <div v-show="active" class="clean-user">
        <svg
          class="clean-logo"
          xmlns="http://www.w3.org/2000/svg"
          width="16"
          height="16"
          fill="none"
        >
          <path
            fill="#4F4F4F"
            d="m7.834 6.893-.097.49.097-.49Zm.8.477a.5.5 0 1 0 .867.5l-.866-.5ZM1.87 10.619l-.307-.394a.5.5 0 0 0 .057.827l.25-.433Zm6.928 
						4-.25.433a.5.5 0 0 0 .746-.364l-.496-.07ZM4.98 8.43l4.157 2.4.5-.866-4.157-2.4-.5.866Zm4.876 2.142a3.997 
						3.997 0 0 0 .204-1.85c-.1-.734-.459-1.515-1.316-2.01l-.5.866c.528.305.756.774.826 1.28a3 3 0 0 1-.152 
						1.365l.938.349Zm-1.112-3.86a2.658 2.658 0 0 0-.812-.31l-.196.98c.163.033.333.095.508.196l.5-.866Zm-.812-.31c-.716-.143-1.378.046-1.9.32a4.04 
						4.04 0 0 0-1.187.957l.77.637c.178-.215.494-.503.883-.709.388-.204.814-.309 1.239-.224l.195-.98Zm.335.74 1.834-3.175-.866-.5L7.4 
						6.643l.866.5Zm1.834-3.175c.022-.038.046-.084.06-.112.019-.033.033-.06.047-.082.03-.049.045-.058.041-.055a.081.081 0 0 
						1-.023.01c-.01.004-.017.003-.013.003.01 0 .073.008.204.083l.5-.866a1.465 1.465 0 0 0-.655-.216.927.927 0 0 
						0-.595.172c-.15.107-.246.242-.308.34-.031.052-.058.1-.078.138l-.046.085.866.5Zm.316-.153a.766.766 0 0 1 .213.16c.007.009-.01-.012-.009-.05 
						0-.022.007-.017-.02.037a1.478 1.478 0 0 1-.05.093l-.076.129.866.5c.035-.06.108-.18.16-.283.054-.113.118-.274.12-.467.004-.446-.308-.756-.704-.985l-.5.866Zm.058.369L8.635 
						7.37l.866.5 1.84-3.187-.866-.5Zm-5.552 3.42-3.361 2.622.615.788 3.36-2.621-.614-.789Zm4.37 7.085.59-4.221-.991-.139-.59 
						4.222.99.138Zm-7.674-3.636 1.732 1 .5-.866-1.732-1-.5.866Zm1.732 1 1.732 1 .5-.866-1.732-1-.5.866Zm1.732 1 1.732 1 .5-.866-1.732-1-.5.866Zm1.732 1 1.732 1 .5-.866-1.732-1-.5.866Z"
          />
          <path
            stroke="#4F4F4F"
            stroke-linecap="round"
            d="m3.768 11.33 1.41-1.11M7.232 13.33l.256-1.777M5.5 12.33l.5-.866"
          />
        </svg>
        <span @click="cleanFilter">Очистить фильтр</span>
      </div>
    </div>
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
    <user-list
      :users="users"
      :searchUser="searchUser"
      @open-modal="activeModal"
      :pages="pages"
    ></user-list>

    <div class="dialog" :class="{ 'dialog-active': visibleModal }">
      <div @click.stop class="dialog__content">
        <div class="select">
          <span>Вы уверены, что хотите удалить пользователя?</span>
          <div class="btns">
            <button @click="deleteUser" class="btn">Да</button>
            <button @click="noDeleteUser" class="btn selected-no">Нет</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import UserList from "./UserList.vue";
export default {
  components: { UserList },
  name: "user-form",
  data() {
    return {
      sortRate: true,
      sortDate: true,
      isActive: {},
      active: false,
      visibleModal: false,
      searchUser: "",
      users: [],
      filterUser: [],
      currentUser: [],
      userItem: {},
      currentListUser: [],
      pages: [],
    };
  },
  methods: {
    async getUsers() {
      try {
        const response = await fetch(
          "https://5ebbb8e5f2cfeb001697d05c.mockapi.io/users"
        );
        const data = await response.json();
        this.users = data;
        this.users.forEach((user) => this.currentListUser.push(user));
      } catch (err) {
        alert("Error User", err);
      }
    },
    deleteUser() {
      this.users = this.users.filter(
        (item) => item.id !== this.currentUser[0].id
      );
      this.currentListUser = this.currentListUser.filter(
        (item) => item.id !== this.currentUser[0].id
      );
      this.visibleModal = false;
      this.currentUser = [];
    },
    noDeleteUser() {
      this.visibleModal = false;
      this.currentUser = [];
    },
    activeModal(bool, user) {
      this.currentUser.push(user);
      this.visibleModal = bool;
    },
    activeSort(item) {
      this.isActive = { [item]: true };
    },
    sortByDateRegistr(item) {
      this.activeSort(item);
      this.userItem = null;
      this.sortDate = !this.sortDate;
      if (!this.sortDate) {
        this.users.sort((a, b) =>
          b.registration_date.localeCompare(a.registration_date)
        );
      } else {
        this.users.sort((a, b) =>
          a.registration_date.localeCompare(b.registration_date)
        );
      }
      this.userItem = this.users;
    },
    sortByRating(item) {
      this.activeSort(item);
      this.userItem = null;
      this.sortRate = !this.sortRate;
      if (!this.sortRate) {
        this.users.sort((a, b) => b.rating - a.rating);
      } else {
        this.users.sort((a, b) => a.rating - b.rating);
      }
      this.userItem = this.users;
    },
    cleanFilter() {
      this.isActive = {};
      this.searchUser = "";
      this.userItem = null;
      this.users = [];
      this.currentListUser.forEach((user) => {
        this.users.push(user);
      });
    },
  },
  watch: {
    searchUser() {
      this.searchUser ? (this.active = true) : (this.active = false);
    },
    userItem() {
      this.userItem ? (this.active = true) : (this.active = false);
    },
  },
  mounted() {
    this.getUsers();
  },
};
</script>

<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Inter:wght@400;500;700&family=Quicksand&display=swap");

.main-container {
  width: 961px;
  margin: 0 auto;
  padding: 12px;
}

.title {
  font-family: "Inter", sans-serif;
  font-style: normal;
  font-weight: 700;
  font-size: 24px;
  line-height: 28px;
}

.form-search {
  position: relative;
  width: 961px;
  background: #ffffff;
  box-shadow: 0px 18px 15px rgba(148, 148, 148, 0.15);
  border-radius: 7px;
  color: #9eaab4;
  margin-top: 20px;
}

.input {
  margin: 12px 16px;
  width: 901px;
  height: 34px;
  background: #eceff0;
  border-radius: 4px;
  border: 1px;
  outline: none;
  padding: 9px 33px;
  font-family: "Inter", sans-serif;
  font-style: normal;
  font-weight: 500;
  font-size: 12px;
  line-height: 16px;
}

::-webkit-input-placeholder {
  color: #9eaab4;
}
:-moz-placeholder {
  color: #9eaab4;
}
::-moz-placeholder {
  color: #9eaab4;
}
:-ms-input-placeholder {
  color: #9eaab4;
}

.logo {
  position: absolute;
  left: 24px;
  top: 21px;
}

.search-logo {
  position: absolute;
  fill: #9eaab4;
  height: 16px;
  width: 16px;
}

.clean-user {
  display: flex;
  align-items: center;
  justify-content: space-around;
  margin: 12px 18px;
  color: #4f4f4f;
  font-family: "Inter", sans-serif;
  font-size: 12px;
  line-height: 16px;
  font-weight: 500;
  width: 130px;
  padding-bottom: 16px;
}

.clean-user span {
  cursor: pointer;
}

.clean-logo {
  height: 16px;
  width: 16px;
}

.dialog {
  position: fixed;
  z-index: 9998;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  transition: all 0.3s ease;
  opacity: 0;
  visibility: hidden;
}

.dialog__content {
  margin: auto;
  background: white;
  border-radius: 12px;
  min-height: 50px;
  min-width: 300px;
}

.dialog-active {
  opacity: 1;
  visibility: visible;
  transition: opacity 0.5s ease;
}

.select {
  font-family: "Inter", sans-serif;
  font-size: 14px;
  line-height: 16px;
  font-weight: 500;
  display: flex;
  padding: 36px;
  flex-direction: column;
  align-items: center;
}

.btns {
  display: flex;
  width: 100%;
  justify-content: space-evenly;
  margin-top: 30px;
}

.btn {
  width: 88px;
  height: 27px;
  border: 1px;
  border-radius: 3px;
  color: #828282;
  cursor: pointer;
  background-color: #e0e0e0;
}

.selected-no {
  background-color: #0cb4f1;
  color: #ffffff;
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
  margin-top: 72px;
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
</style>