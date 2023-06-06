<template>
  <div class="card">
    <div class="card-header">
      <div class="visitor-info">
        <div class="visitor-name">
          {{ visitor.firstName }} {{ visitor.lastName }}
        </div>
        <div class="visitor-iin">ИИН: {{ visitor.iin }}</div>
        <div class="visitor-phone">{{ visitor.phoneNumber }}</div>
      </div>
      <div class="company-info">
        <div class="company-name">{{ visitor.company }}</div>
        <div class="company-position">{{ visitor.position }}</div>
      </div>
    </div>

    <div class="visitor-id">ID: {{ visitor.id }}</div>
    <div class="visitor-type">{{ visitor.type }}</div>
    <div class="last-login">Последний вход: {{ visitor.lastLogin }}</div>
    <button class="delete-button" @click="deleteVisitor(visitor.id)">
      Удалить карточку
    </button>
  </div>
</template>

<script>
export default {
  name: "VisitorCard",
  props: {
    visitor: {
      type: Object,
      required: true,
    },
  },
  methods: {
    deleteVisitor(id) {
      this.$emit("delete", id);
    },
  },
};
</script>

<style>
.card {
  border: 1px solid #ccc;
  border-radius: 5px;
  padding: 10px;
  margin: 5px;
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  width: 1560px;
  background-color: rgb(245, 244, 244);
  height: 75px;
}

.card-header {
  display: flex;
  justify-content: space-between;
  margin-bottom: 10px;
  flex-basis: 16%;
  align-items: center;
}

.visitor-info {
  flex-grow: 1;
  flex-basis: 20%;
  margin-right: 190px;
  color: rgb(255, 166, 0);
}

.visitor-name {
  font-weight: bold;
}

.visitor-iin,
.visitor-phone {
  color: #666;
  white-space: nowrap;
}

.company-info {
  flex-grow: 1;
  text-align: left;
  flex-basis: 20%;
  margin-left: 100px;
  margin-right: 200px;
}

.company-name {
  font-weight: bold;
}

.company-position {
  color: #666;
}

.card-body {
  font-size: 14px;
  flex-basis: 20%;
}

.visitor-id {
  margin-bottom: 5px;
  flex-basis: 16%;
}

.visitor-type {
  font-weight: bold;
  flex-basis: 16%;
}

.last-login {
  color: #666;
  margin-bottom: 10px;
  flex-basis: 16%;
}

.delete-button {
  background-color: #f44336;
  color: #fff;
  border: none;
  border-radius: 5px;
  padding: 5px 10px;
  cursor: pointer;
  flex-basis: 5%;
  margin-top: 10px;
}
</style>
