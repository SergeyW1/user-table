<template>
  <td class="user-item username">{{ user.username }}</td>
  <td class="user-item">{{ user.email }}</td>
  <td class="user-item">
    {{ regDate(user.registration_date) }}
  </td>
  <td class="user-item">{{ user.rating }}</td>
  <div @click="$emit('open-modal', true, user)" class="btn"></div>
</template>

<script>
export default {
  name: "user-item",
  props: {
    user: {
      type: Object,
      required: true,
    },
  },
  emits: ["open-modal"],
  methods: {
    regDate(data) {
      let date = new Date(data);

      let day = date.getDate();
      if (day < 10) {
        day = "0" + day;
      }
      let month = date.getMonth() + 1;
      if (month < 10) {
        month = "0" + month;
      }
      let year = date.getFullYear() % 100;
      if (year < 10) {
        year = "0" + year;
      }
      return `${day}.${month}.${year}`;
    },
  },
};
</script>

<style scoped>
.user-item {
  font-family: "Inter", sans-serif;
  font-size: 12px;
  font-weight: 500;
  line-height: 16px;
  padding: 14px 0;
  width: 220px;
}

.username {
  color: #0cb4f1;
  font-weight: 700;
}

.btn {
  position: relative;
  height: 17px;
  width: 17px;
  cursor: pointer;
  top: 13px;
}

.btn::after,
.btn::before {
  position: absolute;
  height: 2px;
  width: 19px;
  background-color: #333333;
  right: -1px;
  top: 8px;
}

.btn::before {
  content: "";
  transform: rotate(-45deg);
}

.btn::after {
  content: "";
  transform: rotate(45deg);
}
</style>