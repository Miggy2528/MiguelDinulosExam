<template>
  <div class="student-card">
    <h3>{{ student.name }}</h3>
    <p><strong>Course:</strong> {{ student.course }}</p>
    <p><strong>Year:</strong> {{ student.year }}</p>
    <p><strong>Email:</strong> {{ student.email }}</p>
    <button @click="handleClick" class="btn">
      {{ isActive ? 'Deactivate' : 'Activate' }} Student
    </button>
    <p class="status" :class="{ active: isActive, inactive: !isActive }">
      Status: {{ isActive ? 'Active' : 'Inactive' }}
    </p>
  </div>
</template>

<script>
export default {
  name: 'StudentComponent',
  props: {
    student: {
      type: Object,
      required: true,
      validator: (value) => {
        return value.name && value.course && value.year && value.email
      }
    }
  },
  data() {
    return {
      isActive: true
    }
  },
  methods: {
    handleClick() {
      this.isActive = !this.isActive
      this.$emit('status-change', {
        studentId: this.student.id,
        isActive: this.isActive
      })
    }
  }
}
</script>

<style scoped>
.student-card {
  border: 1px solid #ddd;
  border-radius: 8px;
  padding: 20px;
  margin: 15px;
  background-color: #f9f9f9;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
  transition: transform 0.2s;
}

.student-card:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 8px rgba(0,0,0,0.15);
}

.student-card h3 {
  color: #42b983;
  margin-top: 0;
}

.student-card p {
  margin: 8px 0;
  text-align: left;
}

.btn {
  background-color: #42b983;
  color: white;
  border: none;
  padding: 10px 15px;
  border-radius: 4px;
  cursor: pointer;
  margin-top: 10px;
  transition: background-color 0.3s;
}

.btn:hover {
  background-color: #369870;
}

.status {
  margin-top: 15px;
  font-weight: bold;
}

.status.active {
  color: #42b983;
}

.status.inactive {
  color: #e74c3c;
}
</style>