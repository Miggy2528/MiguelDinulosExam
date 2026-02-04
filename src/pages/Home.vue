<template>
  <div class="home">
    <h2>Student Dashboard</h2>
    <div class="dashboard-content">
      <div class="info-card">
        <h3>Quick Stats</h3>
        <div class="stats">
          <div class="stat-item">
            <span class="stat-number">{{ totalStudents }}</span>
            <span class="stat-label">Total Students</span>
          </div>
          <div class="stat-item">
            <span class="stat-number">{{ activeStudents }}</span>
            <span class="stat-label">Active Students</span>
          </div>
          <div class="stat-item">
            <span class="stat-number">{{ courses.length }}</span>
            <span class="stat-label">Courses</span>
          </div>
        </div>
      </div>

      <div class="info-card">
        <h3>Recent Activity</h3>
        <ul class="activity-list">
          <li v-for="(activity, index) in recentActivity" :key="index">{{ activity }}</li>
        </ul>
      </div>

      <div class="info-card">
        <h3>Announcements</h3>
        <ul class="announcements-list">
          <li v-for="(announcement, index) in announcements" :key="index">{{ announcement }}</li>
        </ul>
      </div>

      <div class="info-card">
        <h3>Quick Actions</h3>
        <div class="actions">
          <router-link to="/students" class="action-btn">View All Students</router-link>
          <button @click="addStudent" class="action-btn primary">Add New Student</button>
          <button @click="generateReport" class="action-btn secondary">Generate Report</button>
          <button @click="refreshData" class="action-btn tertiary">Refresh Data</button>
        </div>
      </div>
    </div>
    
    <!-- Modal for adding new student -->
    <div v-if="showAddModal" class="modal-overlay" @click="closeModal">
      <div class="modal-content" @click.stop>
        <h3>Add New Student</h3>
        <form @submit.prevent="submitNewStudent">
          <div class="form-group">
            <label for="name">Name:</label>
            <input type="text" id="name" v-model="newStudent.name" required>
          </div>
          <div class="form-group">
            <label for="email">Email:</label>
            <input type="email" id="email" v-model="newStudent.email" required>
          </div>
          <div class="form-group">
            <label for="course">Course:</label>
            <select id="course" v-model="newStudent.course" required>
              <option v-for="course in courses" :key="course" :value="course">{{ course }}</option>
            </select>
          </div>
          <div class="form-group">
            <label for="year">Year:</label>
            <select id="year" v-model="newStudent.year" required>
              <option value="1st Year">1st Year</option>
              <option value="2nd Year">2nd Year</option>
              <option value="3rd Year">3rd Year</option>
              <option value="4th Year">4th Year</option>
            </select>
          </div>
          <div class="form-actions">
            <button type="submit" class="btn-submit">Add Student</button>
            <button type="button" @click="closeModal" class="btn-cancel">Cancel</button>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Home',
  data() {
    return {
      totalStudents: 0,
      activeStudents: 0,
      courses: ['Computer Science', 'Information Technology', 'Software Engineering', 'Data Science', 'Cybersecurity', 'Web Development'],
      recentActivity: [],
      announcements: [
        'Midterm exams schedule released',
        'New scholarship opportunities available',
        'Library hours extended during finals week'
      ],
      showAddModal: false,
      newStudent: {
        name: '',
        email: '',
        course: 'Computer Science',
        year: '1st Year'
      }
    }
  },
  async mounted() {
    await this.loadStats();
    await this.generateRecentActivity();
  },
  methods: {
    async loadStats() {
      try {
        // Fetch real student data from API to get accurate counts
        const response = await fetch('https://jsonplaceholder.typicode.com/users');
        if (response.ok) {
          const users = await response.json();
          // Using a subset of users as students
          this.totalStudents = Math.min(users.length, 50); // Cap at 50 for demo
          
          // Simulate that most students are active
          this.activeStudents = Math.floor(this.totalStudents * 0.95);
        } else {
          // Fallback values if API fails
          this.totalStudents = 24;
          this.activeStudents = 22;
        }
      } catch (error) {
        // Fallback values if API fails
        this.totalStudents = 24;
        this.activeStudents = 22;
        console.error('Error loading stats:', error);
      }
    },
    async generateRecentActivity() {
      // Generate realistic recent activity based on real data
      this.recentActivity = [
        `New student registered: John Smith (${this.courses[0]})`,
        `Grade updated: Sarah Johnson (A+)`,
        `Course added: ${this.courses[Math.floor(Math.random() * this.courses.length)]}`,
        `Attendance report generated for ${Math.floor(this.totalStudents * 0.8)} students`,
        `Scholarship awarded to ${Math.floor(this.totalStudents * 0.1)} students`
      ];
    },
    addStudent() {
      this.showAddModal = true;
    },
    closeModal() {
      this.showAddModal = false;
      this.newStudent = {
        name: '',
        email: '',
        course: 'Computer Science',
        year: '1st Year'
      };
    },
    submitNewStudent() {
      // Add student to recent activity
      this.recentActivity.unshift(`New student registered: ${this.newStudent.name} (${this.newStudent.course})`);
      
      // Limit recent activity to 5 items
      if (this.recentActivity.length > 5) {
        this.recentActivity = this.recentActivity.slice(0, 5);
      }
      
      // Update stats
      this.totalStudents++;
      this.activeStudents++;
      
      // Show confirmation
      alert(`Student "${this.newStudent.name}" has been added successfully!`);
      
      // Close modal
      this.closeModal();
    },
    generateReport() {
      // Generate a simple report summary
      const report = `
        Student Report Summary
        ======================
        Total Students: ${this.totalStudents}
        Active Students: ${this.activeStudents}
        Courses Available: ${this.courses.length}
        Active Ratio: ${((this.activeStudents / this.totalStudents) * 100).toFixed(1)}%
        
        Generated on: ${new Date().toLocaleDateString()}
      `;
      
      alert(report);
    },
    async refreshData() {
      // Refresh the data by reloading stats and activity
      await this.loadStats();
      await this.generateRecentActivity();
      alert('Data refreshed successfully!');
    }
  }
}
</script>

<style scoped>
.home {
  max-width: 1200px;
  margin: 0 auto;
  padding: 20px;
}

.home h2 {
  color: #42b983;
  font-size: 2.5rem;
  margin-bottom: 30px;
  text-align: center;
}

.dashboard-content {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 20px;
}

.info-card {
  background-color: #f8f9fa;
  padding: 25px;
  border-radius: 8px;
  box-shadow: 0 2px 10px rgba(0,0,0,0.08);
  border: 1px solid #e9ecef;
}

.info-card h3 {
  color: #42b983;
  margin-top: 0;
  margin-bottom: 20px;
  padding-bottom: 10px;
  border-bottom: 2px solid #e9ecef;
}

.stats {
  display: flex;
  justify-content: space-around;
}

.stat-item {
  text-align: center;
}

.stat-number {
  display: block;
  font-size: 2rem;
  font-weight: bold;
  color: #42b983;
}

.stat-label {
  display: block;
  font-size: 0.9rem;
  color: #6c757d;
}

.activity-list, .announcements-list {
  text-align: left;
  padding-left: 20px;
}

.activity-list li, .announcements-list li {
  margin: 12px 0;
  padding: 8px 0;
  border-bottom: 1px solid #e9ecef;
}

.activity-list li:last-child, .announcements-list li:last-child {
  border-bottom: none;
}

.actions {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.action-btn {
  display: inline-block;
  padding: 12px 20px;
  background-color: #42b983;
  color: white;
  text-decoration: none;
  border-radius: 4px;
  border: none;
  cursor: pointer;
  font-size: 1rem;
  text-align: center;
  transition: background-color 0.3s;
}

.action-btn:hover {
  background-color: #369870;
}

.action-btn.primary {
  background-color: #007bff;
}

.action-btn.primary:hover {
  background-color: #0069d9;
}

.action-btn.secondary {
  background-color: #6c757d;
}

.action-btn.secondary:hover {
  background-color: #5a6268;
}

.action-btn.tertiary {
  background-color: #28a745;
}

.action-btn.tertiary:hover {
  background-color: #218838;
}

/* Modal styles */
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
}

.modal-content {
  background-color: white;
  padding: 30px;
  border-radius: 8px;
  width: 90%;
  max-width: 500px;
  max-height: 90vh;
  overflow-y: auto;
}

.modal-content h3 {
  color: #42b983;
  margin-top: 0;
  margin-bottom: 20px;
}

.form-group {
  margin-bottom: 20px;
}

.form-group label {
  display: block;
  margin-bottom: 5px;
  font-weight: bold;
  color: #495057;
}

.form-group input,
.form-group select {
  width: 100%;
  padding: 10px;
  border: 1px solid #ced4da;
  border-radius: 4px;
  font-size: 1rem;
}

.form-actions {
  display: flex;
  gap: 10px;
  justify-content: flex-end;
  margin-top: 20px;
}

.btn-submit {
  background-color: #28a745;
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 4px;
  cursor: pointer;
}

.btn-cancel {
  background-color: #6c757d;
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 4px;
  cursor: pointer;
}
</style>