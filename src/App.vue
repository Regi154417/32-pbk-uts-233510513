<template>
  <div id="app">
    <h1>🌴 Rencana Weekend</h1>

    <div class="input-container">
      <input v-model="newPlan" @keyup.enter="addPlan" type="text" placeholder="Tambahkan rencana..." />
      <button @click="addPlan">Tambah</button>
      <button @click="resetPlans" class="reset-button">Reset Semua</button>
    </div>


    <ul>
      <li v-for="(plan, index) in filteredPlans" :key="index" class="plan-item">
        <input type="checkbox" v-model="plan.done" />

        <template v-if="plan.editing">
          <div class="plan-content">
            <input v-model="plan.text" @keyup.enter="saveEdit(plan)" class="edit-input" />
          </div>
          <div class="plan-actions">
            <button @click="saveEdit(plan)">Simpan</button>
          </div>
        </template>

        <template v-else>
          <div class="plan-content">
            <span :style="{ textDecoration: plan.done ? 'line-through' : 'none', color: plan.done ? 'gray' : 'black' }">
              {{ plan.text }}
            </span>
            <small class="plan-date">🕒 {{ plan.date }}</small>
          </div>
          <div class="action-buttons">
            <button @click="editPlan(plan)">Edit</button>
            <button @click="confirmDelete(index)">Hapus</button>
          </div>

        </template>
      </li>


    </ul>

    <div class="filter-dropdown">
      <button @click="toggleDropdown">
        🔽 Filter
      </button>
      <ul v-if="dropdownOpen" class="dropdown-menu">
        <li @click="setFilter('all')">Tampilkan Semua Rencana</li>
        <li @click="setFilter('incomplete')">Tampilkan Belum Selesai</li>
        <li @click="setFilter('complete')">Tampilkan Sudah Selesai</li>
      </ul>
    </div>

  </div>

  <div class="footer-container">
    <footer class="footer">
      Made by Regi
    </footer>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';
import './style.css';

const newPlan = ref('');
const filterStatus = ref('all');

const plans = ref([
  { text: 'Jalan-jalan ke taman kota', done: false, editing: false, date: new Date().toLocaleString() },
  { text: 'Nonton film bareng teman', done: false, editing: false, date: new Date().toLocaleString() },
  { text: 'Belanja kebutuhan kos', done: false, editing: false, date: new Date().toLocaleString() },
  { text: 'Baca buku di kafe', done: false, editing: false, date: new Date().toLocaleString() },
  { text: 'Main game online', done: false, editing: false, date: new Date().toLocaleString() },
  { text: 'Main game online', done: false, editing: false, date: new Date().toLocaleString() }
]);

function addPlan() {
  if (newPlan.value.trim() === '') {
    alert('Rencana tidak boleh kosong!');
    return;
  }
  plans.value.push({
    text: newPlan.value,
    date: new Date().toLocaleString(),
    done: false,
    editing: false
  });
  newPlan.value = '';
}

function removePlan(index) {
  plans.value.splice(index, 1);
}

function confirmDelete(index) {
  if (confirm('Yakin ingin menghapus rencana ini?')) {
    removePlan(index);
  }
}

function resetPlans() {
  if (confirm('Yakin ingin menghapus semua rencana?')) {
    plans.value = [];
  }
}

function editPlan(plan) {
  plan.editing = true;
}

function saveEdit(plan) {
  if (plan.text.trim() === '') {
    alert('Teks rencana tidak boleh kosong!');
    return;
  }
  plan.editing = false;
}

const filteredPlans = computed(() => {
  if (filterStatus.value === 'incomplete') {
    return plans.value.filter(plan => !plan.done);
  } else if (filterStatus.value === 'complete') {
    return plans.value.filter(plan => plan.done);
  } else {
    return plans.value;
  }
});

const dropdownOpen = ref(false);

function toggleDropdown() {
  dropdownOpen.value = !dropdownOpen.value;
}

function setFilter(status) {
  filterStatus.value = status;
  dropdownOpen.value = false;
}

</script>
