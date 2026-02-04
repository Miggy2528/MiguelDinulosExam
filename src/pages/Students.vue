<template>
  <div class="students">
    <h2>Student Information</h2>
    
    <!-- Loading State -->
    <div v-if="loading" class="loading">
      <p>Loading students data...</p>
      <div class="spinner"></div>
    </div>
    
    <!-- Error State -->
    <div v-else-if="error" class="error">
      <p>{{ error }}</p>
      <button @click="fetchStudents" class="btn retry">Retry</button>
    </div>
    
    <!-- Success State -->
    <div v-else class="students-content">
      <div class="controls">
        <button @click="refreshData" class="btn refresh">Refresh Data</button>
        <p>Total Students: {{ students.length }}</p>
      </div>
      
      <div class="students-grid">
        <StudentComponent 
          v-for="student in students" 
          :key="student.id"
          :student="student"
          @status-change="handleStatusChange"
        />
      </div>
      
      <!-- Status Updates -->
      <div v-if="statusUpdates.length > 0" class="status-updates">
        <h3>Recent Status Changes</h3>
        <ul>
          <li v-for="(update, index) in statusUpdates" :key="index">
            Student ID {{ update.studentId }} is now {{ update.isActive ? 'Active' : 'Inactive' }}
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
import StudentComponent from '../components/StudentComponent.vue'

export default {
  name: 'Students',
  components: {
    StudentComponent
  },
  data() {
    return {
      students: [],
      loading: true,
      error: null,
      statusUpdates: []
    }
  },
  async mounted() {
    await this.fetchStudents()
  },
  methods: {
    async fetchStudents() {
      this.loading = true
      this.error = null
      
      try {
        // Fetch users from JSONPlaceholder API
        const response = await fetch('https://jsonplaceholder.typicode.com/users')
        
        if (!response.ok) {
          throw new Error(`HTTP error! status: ${response.status}`)
        }
        
        const users = await response.json()
        
        // Transform API data to our student format
        this.students = users.slice(0, 8).map(user => ({
          id: user.id,
          name: user.name,
          course: this.getRandomCourse(),
          year: this.getRandomYear(),
          email: user.email
        }))
        
      } catch (err) {
        this.error = 'Failed to fetch student data. Please check your internet connection and try again.'
        console.error('Error fetching students:', err)
      } finally {
        this.loading = false
      }
    },
    
    getRandomCourse() {
      const courses = [
        'Computer Science',
        'Information Technology',
        'Software Engineering',
        'Data Science',
        'Cybersecurity',
        'Web Development',
        'Mobile Development',
        'Artificial Intelligence'
      ]
      return courses[Math.floor(Math.random() * courses.length)]
    },
    
    getRandomYear() {
      const years = ['1st Year', '2nd Year', '3rd Year', '4th Year']
      return years[Math.floor(Math.random() * years.length)]
    },
    
    refreshData() {
      this.fetchStudents()
    },
    
    handleStatusChange(update) {
      this.statusUpdates.unshift(update)
      // Keep only the last 5 updates
      if (this.statusUpdates.length > 5) {
        this.statusUpdates.pop()
      }
    }
  }
}
</script>

<style scoped>
.students {
  max-width: 1200px;
  margin: 0 auto;
  padding: 20px;
}

.students h2 {
  color: #42b983;
  font-size: 2.5rem;
  margin-bottom: 30px;
}

.loading, .error {
  text-align: center;
  padding: 40px;
  font-size: 1.2rem;
}

.spinner {
  border: 4px solid #f3f3f3;
  border-top: 4px solid #42b983;
  border-radius: 50%;
  width: 40px;
  height: 40px;
  animation: spin 1s linear infinite;
  margin: 20px auto;
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}

.error {
  color: #e74c3c;
}

.btn {
  background-color: #42b983;
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 4px;
  cursor: pointer;
  font-size: 1rem;
  transition: background-color 0.3s;
  margin: 10px;
}

.btn:hover {
  background-color: #369870;
}

.btn.retry {
  background-color: #e74c3c;
}

.btn.retry:hover {
  background-color: #c0392b;
}

.btn.refresh {
  background-color: #3498db;
}

.btn.refresh:hover {
  background-color: #2980b9;
}

.controls {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 30px;
  padding: 20px;
  background-color: #f8f9fa;
  border-radius: 8px;
}

.controls p {
  margin: 0;
  font-weight: bold;
  color: #2c3e50;
}

.students-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 20px;
  margin-bottom: 30px;
}

.status-updates {
  background-color: #e8f4f8;
  padding: 20px;
  border-radius: 8px;
  border-left: 4px solid #3498db;
}

.status-updates h3 {
  color: #2c3e50;
  margin-top: 0;
}

.status-updates ul {
  text-align: left;
  margin: 15px 0;
}

.status-updates li {
  margin: 8px 0;
  padding: 5px 0;
  border-bottom: 1px solid #d1e7f1;
}

.status-updates li:last-child {
  border-bottom: none;
}
</style>