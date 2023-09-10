<template>
  <div class="py-4">
    <div class="container">
      <div class="title border-bottom d-flex align-items-center justify-content-between py-2">
        <h5>Task</h5>
        <div class="d-flex align-items-center">
          <input
            v-model="searchQuery"
            type="text"
            class="form-control"
            placeholder="Search"
          />
          <div class="d-flex align-item-center justify-content-end w-100">
            <span class="me-2">View As</span>
            <button
              class="btn btn-outline-secondary py-1 px-3"
              @click="isGrid = !isGrid"
            >
              {{ isGrid ? 'Grid' : 'List' }}
            </button>
          </div>
          <div class="dropdown">
            <button
              id="categoryDropdown"
              class="btn btn-outline-secondary dropdown-toggle"
              type="button"
              data-bs-toggle="dropdown"
              aria-expanded="false"
            >
              Filter Category
            </button>
            <ul
              class="dropdown-menu"
              aria-labelledby="categoryDropdown"
            >
              <li @click="filterByCategory('All')"><a class="dropdown-item">All</a></li>
              <li @click="filterByCategory('Sangat Penting')"><a class="dropdown-item">Sangat Penting</a></li>
              <li @click="filterByCategory('Penting')"><a class="dropdown-item">Penting</a></li>
              <li @click="filterByCategory('Tidak Penting')"><a class="dropdown-item">Tidak Penting</a></li>
              <!-- Tambahkan pilihan untuk kategori lainnya sesuai kebutuhan -->
            </ul>
          </div>
        </div>
      </div>
      <div class="list-task row">
        <CardItem
          :task="tasks[0]"
          :is-grid="isGrid"
        />
        <CardItem
          :task="tasks[1]"
          :is-grid="isGrid"
        />
        <CardItem
          :task="tasks[2]"
          :is-grid="isGrid"
        />
        <CardItem
          v-for="(task, i) in filteredTasks"
          :key="i"
          :task="task"
          :is-grid="isGrid"
        />
      </div>
      <div class="action py-2">
        <a
          v-if="!isCreating"
          href="#"
          class="add-button"
          @click="isCreating = !isCreating"
          >Add Task</a
        >
        <div
          v-else
          class="add-card"
        >
          <div class="card mb-2">
            <div class="card-body d-flex flex-column p-0">
              <input
                class="form-control border-0 mb-2"
                placeholder="Title"
                type="text"
              />
              <textarea
                class="form-control border-0 small"
                placeholder="Description"
                rows="3"
              ></textarea>
            </div>
          </div>
          <div class="button-wrapper d-flex">
            <button class="btn btn-primary me-2">Save</button>
            <button
              class="btn btn-outline-secondary"
              @click="isCreating = !isCreating"
            >
              Cancel
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import CardItem from '@/components/Card/CardItem.vue';
export default {
  components: {
    CardItem,
  },
  layout(context) {
    return 'custom';
  },
  data() {
    return {
      searchQuery: '',
      isGrid: true,
      isCreating: false,
      selectedCategory: 'All', // Default: Menampilkan semua tugas
      tasks: [
        {
          title: 'Task 1',
          description: 'ini deskripsi 1',
          isDone: false,
          category: 'Sangat Penting',
        },
        {
          title: 'Tugas 2',
          description: 'ini deskripsi 2',
          isDone: false,
          category: 'Penting',
        },
        {
          title: 'Kerja 3',
          description: 'ini deskripsi 3',
          isDone: false,
          category: 'Tidak Penting',
        },
      ],
    };
  },
  computed: {
    filteredTasks() {
      if (this.searchQuery) {
        return this.tasks.filter((item) => {
          return this.searchQuery
            .toLowerCase()
            .split(' ')
            .every((v) => item.title.toLowerCase().includes(v));
        });
      } else if (this.selectedCategory === 'All') {
        return this.tasks;
      } else {
        return this.tasks.filter((item) => item.category === this.selectedCategory);
      }
    },
  },
  methods: {
    filterByCategory(category) {
      this.selectedCategory = category;
    },
  },
};
</script>

<style></style>
