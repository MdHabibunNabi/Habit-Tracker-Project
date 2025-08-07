<template>
  <div>
    <h1 class="text-3xl font-bold mb-4 text-center">7-Day Habit Tracker</h1>
    <div class="mb-4">
      <label class="block font-semibold mb-1">Add a Habit:</label>
      <input v-model="newHabit" @keyup.enter="addHabit" class="w-full p-2 border rounded" placeholder="e.g. Drink water" />
      <button @click="addHabit" class="mt-2 px-4 py-2 bg-blue-600 text-white rounded">Add Habit</button>
    </div>

    <div v-if="habits.length" class="overflow-auto">
      <table class="table-auto w-full text-sm">
        <thead>
          <tr>
            <th class="p-2 border">Habit</th>
            <th v-for="day in days" :key="day" class="p-2 border">{{ day }}</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(habit, hIndex) in habits" :key="hIndex">
            <td class="p-2 border font-medium">{{ habit.name }}</td>
            <td v-for="(day, dIndex) in days" :key="dIndex" class="p-2 border text-center">
              <input type="checkbox" v-model="habit.days[dIndex]" @change="saveToStorage" />
            </td>
          </tr>
        </tbody>
      </table>
      <button @click="clearAll" class="mt-4 px-4 py-2 bg-red-500 text-white rounded">Clear All</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      days: ['Day 1', 'Day 2', 'Day 3', 'Day 4', 'Day 5', 'Day 6', 'Day 7'],
      newHabit: '',
      habits: []
    }
  },
  created() {
    this.loadFromStorage();
  },
  methods: {
    addHabit() {
      if (!this.newHabit.trim()) return;
      this.habits.push({ name: this.newHabit.trim(), days: Array(7).fill(false) });
      this.newHabit = '';
      this.saveToStorage();
    },
    clearAll() {
      if (confirm('Are you sure you want to clear all data?')) {
        this.habits = [];
        localStorage.removeItem('habitTrackerData');
      }
    },
    saveToStorage() {
      localStorage.setItem('habitTrackerData', JSON.stringify(this.habits));
    },
    loadFromStorage() {
      const data = localStorage.getItem('habitTrackerData');
      if (data) {
        this.habits = JSON.parse(data);
      }
    }
  }
}
</script>

<style scoped>
body {
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}
</style>
