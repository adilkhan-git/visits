<template>
  <div id="app">
    <div class="sidebar">
      <div class="user-profile">
        <img class="user-photo" src="@/assets/photo.png" alt="User Photo" />
        <div class="user-info">
          <div class="user-name">Adilkhan Zamanbek</div>
          <div class="online-status">Online</div>
        </div>
      </div>
      <div class="tabs">
        <div class="tab" v-for="tab in tabs" :key="tab.id">{{ tab.name }}</div>
      </div>
    </div>
    <div class="main-content">
      <h1>Карточки посетителей</h1>
      <hr class="hr" />
      <button class="add-visit-button" @click="openModal">
        Добавить посещение
      </button>
      <div class="search-bar">
        <input type="text" placeholder="Имя" v-model="searchName" />
        <input type="text" placeholder="Фамилия" v-model="searchLastName" />
        <input type="text" placeholder="ID" v-model="searchId" />
        <input type="text" placeholder="Телефон" v-model="searchPhone" />
        <input type="text" placeholder="ИИН" v-model="searchIin" />
      </div>

      <div class="visitor-cards">
        <VisitorCard
          v-for="visitor in filteredVisitors"
          :key="visitor.id"
          :visitor="visitor"
          @delete="deleteVisitor"
        />
      </div>
    </div>

    <!-- Модальное окно -->
    <div class="modal" v-if="showModal">
      <div class="modal-content">
        <h2>Заполните данные посещения</h2>
        <form @submit.prevent="addVisit">
          <input
            type="text"
            v-model="newVisit.firstName"
            placeholder="Имя"
            required
          />
          <input
            type="text"
            v-model="newVisit.lastName"
            placeholder="Фамилия"
            required
          />
          <input
            type="text"
            v-model="newVisit.iin"
            placeholder="ИИН"
            required
          />
          <input
            type="text"
            v-model="newVisit.phoneNumber"
            placeholder="Телефон"
            required
          />
          <input
            type="text"
            v-model="newVisit.company"
            placeholder="Компания"
            required
          />
          <input
            type="text"
            v-model="newVisit.position"
            placeholder="Должность"
            required
          />
          <input
            type="text"
            v-model="newVisit.type"
            placeholder="Тип"
            required
          />
          <button type="submit">Сохранить</button>
          <button @click="closeModal">Отмена</button>
        </form>
      </div>
    </div>
  </div>
</template>

<script>
import VisitorCard from "./components/VisitorCard.vue";

export default {
  name: "App",
  components: {
    VisitorCard,
  },
  data() {
    return {
      visitors: [
        {
          id: 1,
          firstName: "Иван",
          lastName: "Петров",
          iin: "791225758965",
          phoneNumber: "+77478889977",
          company: "ООО Road Construction",
          position: "Менеджер",
          type: "Многоразовый",
          lastLogin: "2023-05-31",
        },
        {
          id: 2,
          firstName: "Анна",
          lastName: "Иванова",
          iin: "987654321098",
          phoneNumber: "+79876543210",
          company: "АО Kcell",
          position: "Разработчик",
          type: "Многоразовый",
          lastLogin: "2023-06-02",
        },
        {
          id: 3,
          firstName: "Мария",
          lastName: "Сидорова",
          iin: "456789012345",
          phoneNumber: "+74567890123",
          company: "ИП Семакина",
          position: "Бухгалтер",
          type: "Временный",
          lastLogin: "2023-06-04",
        },
      ],
      searchName: "",
      searchLastName: "",
      searchId: "",
      searchPhone: "",
      searchIin: "",
      tabs: [
        { id: 1, name: "Посещения" },
        { id: 2, name: "Посетители" },
        { id: 3, name: "Гости" },
        { id: 4, name: "Сотрудники" },
        { id: 5, name: "Компании" },
        { id: 6, name: "Отчет о посещениях" },
        { id: 7, name: "Отчет о гостях" },
        { id: 8, name: "Пользователи" },
        { id: 9, name: "Мероприятия" },
        { id: 10, name: "Настройки" },
      ],
      showModal: false,
      newVisit: {
        firstName: "",
        lastName: "",
        iin: "",
        phoneNumber: "",
        company: "",
        position: "",
        type: "",
      },
    };
  },
  computed: {
    filteredVisitors() {
      return this.visitors.filter((visitor) => {
        const name =
          visitor.firstName.toLowerCase() +
          " " +
          visitor.lastName.toLowerCase();
        const queryName = this.searchName.toLowerCase();
        const queryLastName = this.searchLastName.toLowerCase();
        const queryId = this.searchId.toLowerCase();
        const queryPhone = this.searchPhone.toLowerCase();
        const queryIin = this.searchIin.toLowerCase();
        return (
          name.includes(queryName) &&
          visitor.lastName.toLowerCase().includes(queryLastName) &&
          visitor.id.toString().includes(queryId) &&
          visitor.phoneNumber.toLowerCase().includes(queryPhone) &&
          visitor.iin.includes(queryIin)
        );
      });
    },
  },
  methods: {
    deleteVisitor(id) {
      this.visitors = this.visitors.filter((visitor) => visitor.id !== id);
    },
    openModal() {
      this.showModal = true;
    },
    closeModal() {
      this.showModal = false;
      this.resetForm();
    },
    resetForm() {
      this.newVisit = {
        firstName: "",
        lastName: "",
        iin: "",
        phoneNumber: "",
        company: "",
        position: "",
        type: "",
      };
    },
    addVisit() {
      const newVisitor = {
        id: this.visitors.length + 1,
        firstName: this.newVisit.firstName,
        lastName: this.newVisit.lastName,
        iin: this.newVisit.iin,
        phoneNumber: this.newVisit.phoneNumber,
        company: this.newVisit.company,
        position: this.newVisit.position,
        type: this.newVisit.type,
        lastLogin: new Date().toISOString().split("T")[0],
      };
      this.visitors.push(newVisitor);
      this.closeModal();
    },
  },
};
</script>

<style>
#app {
  display: flex;
}

.modal {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
}

.modal-content {
  background-color: white;
  padding: 20px;
  border-radius: 5px;
}

.modal-content h2 {
  margin-bottom: 10px;
}

.modal-content form input {
  margin-bottom: 10px;
  padding: 5px;
}

.modal-content form button {
  background-color: orange;
  color: white;
  border: none;
  border-radius: 5px;
  padding: 10px 20px;
  cursor: pointer;
  margin-right: 10px;
}

.modal-content form button:last-child {
  background-color: rgb(68, 68, 68);
}
.sidebar {
  width: 200px;
  background-color: #646464;
  padding: 20px;
  color: white;
  height: 100vh;
}

.user-profile {
  display: flex;
  align-items: center;
  margin-bottom: 20px;
}

.user-photo {
  width: 50px;
  height: 50px;
  border-radius: 50%;
}

.user-info {
  margin-left: 10px;
}

.user-name {
  font-weight: bold;
}

.online-status {
  display: flex;
  align-items: center;
  color: white;
  font-size: 12px;
}

.online-status::before {
  content: "";
  display: inline-block;
  width: 10px;
  height: 10px;
  border-radius: 50%;
  background-color: rgb(0, 184, 0);
  margin-right: 5px;
}

.tabs {
  margin-bottom: 20px;
}

.tab {
  cursor: pointer;
  margin-bottom: 10px;
}

.search-bar {
  margin-bottom: 20px;
  margin-left: 35px;
}

.add-visit-button {
  background-color: orange;
  color: white;
  border: none;
  border-radius: 5px;
  padding: 10px 20px;
  cursor: pointer;
  margin-bottom: 20px;
  margin-left: 35px;
}

.visitor-cards {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  padding: 30px;
}

h1 {
  margin-left: 35px;
}

.hr {
  background-color: orange;
  height: 4px;
  border: none;
}
</style>
