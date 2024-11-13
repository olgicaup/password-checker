<template>
    <div class="checker-card">
      <h2>Password Strength Checker</h2>
      <label>Username:</label>
      <input v-model="username" type="text" placeholder="Enter username">
      <label>Password:</label>
      <input v-model="password" type="password" @input="evaluatePassword" placeholder="Enter password">
      <div v-if="password">
        <p>Password Strength: <span :class="strengthClass">{{ strengthLevel }}</span></p>
        <p>Score: {{ score }}/10</p>
        <p v-if="feedback">{{ feedback }}</p>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        password: '',
        username: '',
        score: 0,
        feedback: '',
        commonPasswords: ['password', '12345', 'qwerty', 'admin', 'letmein', '12345678', 'p@ssw0rd', 'master']
      };
    },
    computed: {
      strengthClass() {
        if (this.score >= 8) return 'strong';
        if (this.score >= 5) return 'moderate';
        return 'weak';
      },
      strengthLevel() {
        if (this.score >= 8) return 'Strong';
        if (this.score >= 5) return 'Moderate';
        return 'Weak';
      }
    },
    methods: {
      evaluatePassword() {
        this.score = 0;
        this.feedback = '';
  
        if (this.password.toLowerCase() === this.username.toLowerCase()) {
          this.feedback += 'Password should not match the username.';
          this.score = Math.min(this.score, 3);
        } else{
          this.score += 1;
        }

        const containsCommonWord = this.commonPasswords.some((word) =>
        this.password.toLowerCase().includes(word)
        );
        if (!containsCommonWord) {
            this.score += 2;
        } else {
            this.score -=1;
            this.feedback += 'Avoid using common words or patterns like "password" or "12345". ';
        }

        if (this.password.length < 8) {
          this.feedback += 'Password should be at least 8 characters long. ';
          return; 
        } else {
          this.score += 2;
        }
  
        if (/[a-z]/.test(this.password)) {
          this.score += 1;
        } else {
          this.feedback += 'Add at least one lowercase letter. ';
        }
  
        if (/[A-Z]/.test(this.password)) {
          this.score += 1;
        } else {
          this.feedback += 'Add at least one uppercase letter. ';
        }
  
        if (/[0-9]/.test(this.password)) {
          this.score += 1;
        } else {
          this.feedback += 'Add at least one digit. ';
        }
  
        if (/[!@#$%^&*(),.?":{}|<>]/.test(this.password)) {
          this.score += 2;
        } else {
          this.feedback += 'Add at least one special character. ';
        }        
      }
    }
  };
  </script>
  
  <style scoped>
.checker-card {
  max-width: 400px;
  margin: 50px auto;
  padding: 30px;
  background: beige;
  font-family: Arial, sans-serif;
}

.checker-card h2 {
  color: saddlebrown;
  font-size: 1.8rem;
  text-align: center;
  margin-bottom: 20px;
  font-weight: 600;
}

.checker-card label {
  font-size: 1rem;
  font-weight: 500;
  color: saddlebrown;
  display: block;
  margin-top: 15px;
}

.checker-card input {
  width: 90%;
  padding: 10px;
  border: 1px solid s;
  border-radius: 6px;
  margin-top: 5px;
  font-size: 1rem;
}

.checker-card input:focus {
  outline: none;
  border-color: blue;
  box-shadow: 0 0 4px rgba(0, 85, 179, 0.2);
}

.strong {
  color: green;
  font-weight: 700;
}

.moderate {
  color: orange;
  font-weight: 700;
}

.weak {
  color: red;
  font-weight: 700;
}

.checker-card p {
  font-size: 1rem;
  color: grey;
  margin: 10px 0;
}

.checker-card p span {
  display: inline-block;
  padding: 2px 8px;
  border-radius: 4px;
  background-color: white;
}

.checker-card p span.strong {
  background-color: whitesmoke;
}

.checker-card p span.moderate {
  background-color: whitesmoke;
}

.checker-card p span.weak {
  background-color: whitesmoke;
}
</style>

  