---
title: "React Hooks: Từ Class Components đến Functional - Cuộc cách mạng tôi không ngờ tới"
date: 2025-10-14
draft: false
tags: ["JavaScript", "React", "Hooks", "Frontend", "Web Development"]
categories: ["JavaScript", "Lập Trình Web"]
description: "useState, useEffect, useContext, custom hooks - React Hooks đã thay đổi hoàn toàn cách tôi viết React. Không còn class, không còn this, chỉ còn pure functions."
summary: "Tôi từng viết 500 dòng class component. Sau khi học Hooks, refactor còn 200 dòng và code clean gấp 10 lần. Đây là những gì tôi ước mình biết sớm hơn."
feature: "https://images.unsplash.com/photo-1633356122544-f134324a6cee?w=800&h=600&fit=crop"
showTableOfContents: true
---

Năm 2018, React team công bố Hooks. Community phát cuồng. Tôi? Meh.

"Class components hoạt động tốt mà. Tại sao phải học cái mới?"

Fast forward 6 tháng. Tôi join một project mới. Tech lead bắt buộc dùng Hooks. Tôi reluctantly học.

1 tuần sau? **Mind = blown.**

Code ngắn hơn 50%. Logic dễ reuse hơn 100%. Bugs giảm 80%. Và quan trọng nhất - **không còn phải deal với `this`!**

![React Hooks Revolution](https://images.unsplash.com/photo-1633356122544-f134324a6cee?w=1200&h=600&fit=crop)

Hôm nay, tôi không thể tưởng tượng viết React mà không có Hooks. Đây là câu chuyện về journey đó - và những gì tôi học được.

---

## 🎯 Vấn đề với Class Components

### **Before Hooks - The Dark Ages**
```jsx
// Counter component - Class version
class Counter extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      count: 0,
      isEven: true
    };
    
    // ❌ Must bind methods
    this.increment = this.increment.bind(this);
    this.decrement = this.decrement.bind(this);
  }
  
  increment() {
    this.setState(prevState => ({
      count: prevState.count + 1,
      isEven: (prevState.count + 1) % 2 === 0
    }));
  }
  
  decrement() {
    this.setState(prevState => ({
      count: prevState.count - 1,
      isEven: (prevState.count - 1) % 2 === 0
    }));
  }
  
  // ❌ Lifecycle methods scattered
  componentDidMount() {
    document.title = `Count: ${this.state.count}`;
  }
  
  componentDidUpdate() {
    document.title = `Count: ${this.state.count}`;
  }
  
  componentWillUnmount() {
    document.title = 'React App';
  }
  
  render() {
    return (
      <div>
        <h1>{this.state.count}</h1>
        <p>{this.state.isEven ? 'Even' : 'Odd'}</p>
        <button onClick={this.increment}>+</button>
        <button onClick={this.decrement}>-</button>
      </div>
    );
  }
}
```

**Problems:**
- 50+ dòng code cho một counter đơn giản
- `this` everywhere - confusing và error-prone
- Phải bind methods trong constructor
- Logic bị scatter qua nhiều lifecycle methods
- Không thể reuse stateful logic dễ dàng
- Harder to test

![Class Component Problems](https://images.unsplash.com/photo-1555066931-4365d14bab8c?w=1200&h=600&fit=crop)

---

## ✨ Hook #1: useState - State Management Made Simple

### **After Hooks - The Renaissance**
```jsx
import { useState, useEffect } from 'react';

// ✅ Functional component with Hooks
function Counter() {
  const [count, setCount] = useState(0);
  const [isEven, setIsEven] = useState(true);
  
  const increment = () => {
    const newCount = count + 1;
    setCount(newCount);
    setIsEven(newCount % 2 === 0);
  };
  
  const decrement = () => {
    const newCount = count - 1;
    setCount(newCount);
    setIsEven(newCount % 2 === 0);
  };
  
  useEffect(() => {
    document.title = `Count: ${count}`;
  }, [count]);
  
  return (
    <div>
      <h1>{count}</h1>
      <p>{isEven ? 'Even' : 'Odd'}</p>
      <button onClick={increment}>+</button>
      <button onClick={decrement}>-</button>
    </div>
  );
}
```

**Benefits:**
- 25 dòng thay vì 50+ dòng (50% reduction!)
- Không có `this`
- Không cần bind
- Logic grouped together
- Dễ đọc, dễ maintain

### **useState Deep Dive**
```jsx
// Basic usage
const [state, setState] = useState(initialValue);

// With different types
const [name, setName] = useState('John');           // string
const [age, setAge] = useState(25);                 // number
const [isActive, setIsActive] = useState(true);     // boolean
const [items, setItems] = useState([]);             // array
const [user, setUser] = useState({});               // object

// Lazy initialization - expensive computation
const [data, setData] = useState(() => {
  const storedData = localStorage.getItem('data');
  return storedData ? JSON.parse(storedData) : [];
});

// Functional updates - when new state depends on previous
const [count, setCount] = useState(0);

// ❌ Wrong - may cause bugs with multiple updates
setCount(count + 1);
setCount(count + 1); // Still 1, not 2!

// ✅ Correct - use functional update
setCount(prevCount => prevCount + 1);
setCount(prevCount => prevCount + 1); // Now it's 2!
```

### **Real-world Example: Form Handling**
```jsx
function StudentForm() {
  const [formData, setFormData] = useState({
    name: '',
    email: '',
    gpa: '',
    enrollmentYear: new Date().getFullYear()
  });
  
  const [errors, setErrors] = useState({});
  const [isSubmitting, setIsSubmitting] = useState(false);
  
  const handleChange = (e) => {
    const { name, value } = e.target;
    
    // Update form data
    setFormData(prev => ({
      ...prev,
      [name]: value
    }));
    
    // Clear error for this field
    if (errors[name]) {
      setErrors(prev => ({
        ...prev,
        [name]: ''
      }));
    }
  };
  
  const validate = () => {
    const newErrors = {};
    
    if (!formData.name.trim()) {
      newErrors.name = 'Name is required';
    }
    
    if (!formData.email.includes('@')) {
      newErrors.email = 'Invalid email';
    }
    
    const gpa = parseFloat(formData.gpa);
    if (isNaN(gpa) || gpa < 0 || gpa > 4.0) {
      newErrors.gpa = 'GPA must be between 0 and 4.0';
    }
    
    setErrors(newErrors);
    return Object.keys(newErrors).length === 0;
  };
  
  const handleSubmit = async (e) => {
    e.preventDefault();
    
    if (!validate()) return;
    
    setIsSubmitting(true);
    
    try {
      const response = await fetch('/api/students', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(formData)
      });
      
      if (response.ok) {
        alert('Student created successfully!');
        // Reset form
        setFormData({
          name: '',
          email: '',
          gpa: '',
          enrollmentYear: new Date().getFullYear()
        });
      }
    } catch (error) {
      alert('Error: ' + error.message);
    } finally {
      setIsSubmitting(false);
    }
  };
  
  return (
    <form onSubmit={handleSubmit}>
      <div>
        <input
          name="name"
          value={formData.name}
          onChange={handleChange}
          placeholder="Name"
        />
        {errors.name && <span className="error">{errors.name}</span>}
      </div>
      
      <div>
        <input
          name="email"
          value={formData.email}
          onChange={handleChange}
          placeholder="Email"
        />
        {errors.email && <span className="error">{errors.email}</span>}
      </div>
      
      <div>
        <input
          name="gpa"
          value={formData.gpa}
          onChange={handleChange}
          placeholder="GPA"
          type="number"
          step="0.01"
        />
        {errors.gpa && <span className="error">{errors.gpa}</span>}
      </div>
      
      <button type="submit" disabled={isSubmitting}>
        {isSubmitting ? 'Submitting...' : 'Submit'}
      </button>
    </form>
  );
}
```

![Form Handling](https://images.unsplash.com/photo-1587620962725-abab7fe55159?w=1200&h=600&fit=crop)

---

## ⚡ Hook #2: useEffect - Side Effects Master

### **The Problem useEffect Solves**
```jsx
// ❌ Class component - scattered logic
class UserProfile extends React.Component {
  componentDidMount() {
    // Fetch user
    this.fetchUser();
    // Subscribe to status
    this.subscribeToStatus();
    // Start timer
    this.timerId = setInterval(this.updateTime, 1000);
  }
  
  componentDidUpdate(prevProps) {
    // Refetch if ID changed
    if (prevProps.userId !== this.props.userId) {
      this.fetchUser();
    }
  }
  
  componentWillUnmount() {
    // Cleanup
    this.unsubscribeFromStatus();
    clearInterval(this.timerId);
  }
  
  // Methods scattered everywhere...
}
```
```jsx
// ✅ Hooks - logic grouped by concern
function UserProfile({ userId }) {
  const [user, setUser] = useState(null);
  const [status, setStatus] = useState('offline');
  const [time, setTime] = useState(new Date());
  
  // Effect 1: Fetch user
  useEffect(() => {
    fetchUser(userId).then(setUser);
  }, [userId]); // Re-run when userId changes
  
  // Effect 2: Subscribe to status
  useEffect(() => {
    const unsubscribe = subscribeToStatus(userId, setStatus);
    return unsubscribe; // Cleanup
  }, [userId]);
  
  // Effect 3: Timer
  useEffect(() => {
    const timerId = setInterval(() => setTime(new Date()), 1000);
    return () => clearInterval(timerId); // Cleanup
  }, []); // Run once
  
  // Render...
}
```

### **useEffect Patterns**
```jsx
// Pattern 1: Run once on mount (like componentDidMount)
useEffect(() => {
  console.log('Component mounted');
}, []); // Empty dependency array

// Pattern 2: Run on every render (no dependency array)
useEffect(() => {
  console.log('Component rendered');
}); // No second argument

// Pattern 3: Run when specific values change
useEffect(() => {
  console.log('Count changed:', count);
}, [count]); // Re-run when count changes

// Pattern 4: Cleanup (like componentWillUnmount)
useEffect(() => {
  const subscription = subscribeToAPI();
  
  return () => {
    // This runs on cleanup
    subscription.unsubscribe();
  };
}, []);

// Pattern 5: Multiple dependencies
useEffect(() => {
  fetchData(userId, filter);
}, [userId, filter]); // Re-run when either changes
```

### **Real-world: Data Fetching**
```jsx
function StudentList() {
  const [students, setStudents] = useState([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);
  const [searchTerm, setSearchTerm] = useState('');
  
  useEffect(() => {
    // Flag to prevent state update on unmounted component
    let isMounted = true;
    
    const fetchStudents = async () => {
      try {
        setLoading(true);
        setError(null);
        
        const response = await fetch(
          `/api/students?search=${searchTerm}`
        );
        
        if (!response.ok) throw new Error('Failed to fetch');
        
        const data = await response.json();
        
        // Only update if component still mounted
        if (isMounted) {
          setStudents(data);
          setLoading(false);
        }
      } catch (err) {
        if (isMounted) {
          setError(err.message);
          setLoading(false);
        }
      }
    };
    
    // Debounce search
    const timeoutId = setTimeout(() => {
      fetchStudents();
    }, 500);
    
    // Cleanup
    return () => {
      isMounted = false;
      clearTimeout(timeoutId);
    };
  }, [searchTerm]); // Re-fetch when search changes
  
  if (loading) return <div>Loading...</div>;
  if (error) return <div>Error: {error}</div>;
  
  return (
    <div>
      <input
        type="text"
        value={searchTerm}
        onChange={(e) => setSearchTerm(e.target.value)}
        placeholder="Search students..."
      />
      
      <ul>
        {students.map(student => (
          <li key={student.id}>
            {student.name} - GPA: {student.gpa}
          </li>
        ))}
      </ul>
    </div>
  );
}
```

![Data Fetching](https://images.unsplash.com/photo-1551288049-bebda4e38f71?w=1200&h=600&fit=crop)

---

## 🎨 Hook #3: useContext - Props Drilling Killer

### **The Props Drilling Problem**
```jsx
// ❌ Passing props through many levels
function App() {
  const [user, setUser] = useState(null);
  
  return <Dashboard user={user} setUser={setUser} />;
}

function Dashboard({ user, setUser }) {
  return <Sidebar user={user} setUser={setUser} />;
}

function Sidebar({ user, setUser }) {
  return <UserMenu user={user} setUser={setUser} />;
}

function UserMenu({ user, setUser }) {
  // Finally use it here!
  return <div>{user?.name}</div>;
}

// Passed through 4 components! 😱
```

### **useContext Solution**
```jsx
// Create context
const UserContext = createContext(null);

// ✅ Provider at top level
function App() {
  const [user, setUser] = useState(null);
  
  return (
    <UserContext.Provider value={{ user, setUser }}>
      <Dashboard />
    </UserContext.Provider>
  );
}

// Components in between don't need props
function Dashboard() {
  return <Sidebar />;
}

function Sidebar() {
  return <UserMenu />;
}

// ✅ Use context directly where needed
function UserMenu() {
  const { user, setUser } = useContext(UserContext);
  
  const handleLogout = () => {
    setUser(null);
  };
  
  return (
    <div>
      {user ? (
        <>
          <p>Welcome, {user.name}!</p>
          <button onClick={handleLogout}>Logout</button>
        </>
      ) : (
        <p>Please login</p>
      )}
    </div>
  );
}
```

### **Real-world: Theme Context**
```jsx
// theme-context.js
const ThemeContext = createContext({
  theme: 'light',
  toggleTheme: () => {}
});

export function ThemeProvider({ children }) {
  const [theme, setTheme] = useState('light');
  
  const toggleTheme = () => {
    setTheme(prev => prev === 'light' ? 'dark' : 'light');
  };
  
  return (
    <ThemeContext.Provider value={{ theme, toggleTheme }}>
      {children}
    </ThemeContext.Provider>
  );
}

export function useTheme() {
  const context = useContext(ThemeContext);
  if (!context) {
    throw new Error('useTheme must be used within ThemeProvider');
  }
  return context;
}

// Usage
function App() {
  return (
    <ThemeProvider>
      <Header />
      <Main />
      <Footer />
    </ThemeProvider>
  );
}

function Header() {
  const { theme, toggleTheme } = useTheme();
  
  return (
    <header className={theme}>
      <h1>My App</h1>
      <button onClick={toggleTheme}>
        Switch to {theme === 'light' ? 'dark' : 'light'} mode
      </button>
    </header>
  );
}

function Main() {
  const { theme } = useTheme();
  
  return (
    <main className={theme}>
      <p>Content here...</p>
    </main>
  );
}
```

![Context API](https://images.unsplash.com/photo-1558618666-fcd25c85cd64?w=1200&h=600&fit=crop)

---

## 🚀 Hook #4: useMemo & useCallback - Performance Optimization

### **useMemo - Memoize Expensive Calculations**
```jsx
function StudentDashboard({ students }) {
  // ❌ Without useMemo - recalculates on every render
  const statistics = calculateStatistics(students);
  
  // ✅ With useMemo - only recalculate when students change
  const statistics = useMemo(() => {
    return calculateStatistics(students);
  }, [students]);
  
  return (
    <div>
      <p>Total Students: {statistics.total}</p>
      <p>Average GPA: {statistics.averageGpa}</p>
      <p>Top Student: {statistics.topStudent.name}</p>
    </div>
  );
}

function calculateStatistics(students) {
  console.log('Calculating...'); // See when this runs
  
  const total = students.length;
  const averageGpa = students.reduce((sum, s) => sum + s.gpa, 0) / total;
  const topStudent = students.reduce((top, s) => 
    s.gpa > top.gpa ? s : top
  );
  
  return { total, averageGpa, topStudent };
}
```

### **useCallback - Memoize Functions**
```jsx
function StudentList({ students }) {
  const [selectedId, setSelectedId] = useState(null);
  
  // ❌ Without useCallback - new function on every render
  const handleSelect = (id) => {
    setSelectedId(id);
  };
  
  // ✅ With useCallback - same function reference
  const handleSelect = useCallback((id) => {
    setSelectedId(id);
  }, []); // No dependencies - function never changes
  
  return (
    <div>
      {students.map(student => (
        <StudentCard
          key={student.id}
          student={student}
          onSelect={handleSelect} // Won't cause re-render if memoized
          isSelected={student.id === selectedId}
        />
      ))}
    </div>
  );
}

// Memoized component - only re-renders if props change
const StudentCard = React.memo(({ student, onSelect, isSelected }) => {
  console.log('Rendering:', student.name);
  
  return (
    <div
      className={isSelected ? 'selected' : ''}
      onClick={() => onSelect(student.id)}
    >
      <h3>{student.name}</h3>
      <p>GPA: {student.gpa}</p>
    </div>
  );
});
```

### **When to Use Performance Hooks?**
```jsx
// ✅ Good use case: Expensive calculation
const expensiveValue = useMemo(() => {
  return hugeArray.map(item => complexCalculation(item));
}, [hugeArray]);

// ❌ Overkill: Simple calculation
const sum = useMemo(() => a + b, [a, b]); // Just do: const sum = a + b;

// ✅ Good: Prevent child re-renders
const handleClick = useCallback(() => {
  doSomething();
}, []);

// ❌ Unnecessary: No child components
const handleClick = useCallback(() => {
  doSomething();
}, []); // If no children receiving this, don't bother
```

---

## 🎯 Custom Hooks - Reusable Logic

### **Why Custom Hooks?**

DRY principle. Reuse stateful logic across components.
```jsx
// ✅ Custom hook: useFetch
function useFetch(url) {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);
  
  useEffect(() => {
    let isMounted = true;
    
    const fetchData = async () => {
      try {
        setLoading(true);
        const response = await fetch(url);
        
        if (!response.ok) throw new Error('Fetch failed');
        
        const result = await response.json();
        
        if (isMounted) {
          setData(result);
          setError(null);
        }
      } catch (err) {
        if (isMounted) {
          setError(err.message);
        }
      } finally {
        if (isMounted) {
          setLoading(false);
        }
      }
    };
    
    fetchData();
    
    return () => {
      isMounted = false;
    };
  }, [url]);
  
  return { data, loading, error };
}

// Usage - Clean and reusable!
function StudentList() {
  const { data: students, loading, error } = useFetch('/api/students');
  
  if (loading) return <div>Loading...</div>;
  if (error) return <div>Error: {error}</div>;
  
  return (
    <ul>
      {students.map(s => (
        <li key={s.id}>{s.name}</li>
      ))}
    </ul>
  );
}

function TeacherList() {
  const { data: teachers, loading, error } = useFetch('/api/teachers');
  
  // Same logic, different data!
  if (loading) return <div>Loading...</div>;
  if (error) return <div>Error: {error}</div>;
  
  return (
    <ul>
      {teachers.map(t => (
        <li key={t.id}>{t.name}</li>
      ))}
    </ul>
  );
}
```

### **More Custom Hooks Examples**
```jsx
// useLocalStorage - Persist state
function useLocalStorage(key, initialValue) {
  const [value, setValue] = useState(() => {
    const stored = localStorage.getItem(key);
    return stored ? JSON.parse(stored) : initialValue;
  });
  
  useEffect(() => {
    localStorage.setItem(key, JSON.stringify(value));
  }, [key, value]);
  
  return [value, setValue];
}

// Usage
function App() {
  const [theme, setTheme] = useLocalStorage('theme', 'light');
  
  return (
    <div className={theme}>
      <button onClick={() => setTheme(theme === 'light' ? 'dark' : 'light')}>
        Toggle Theme
      </button>
    </div>
  );
}

// useDebounce - Delay value updates
function useDebounce(value, delay) {
  const [debouncedValue, setDebouncedValue] = useState(value);
  
  useEffect(() => {
    const handler = setTimeout(() => {
      setDebouncedValue(value);
    }, delay);
    
    return () => clearTimeout(handler);
  }, [value, delay]);
  
  return debouncedValue;
}

// Usage
function SearchBox() {
  const [searchTerm, setSearchTerm] = useState('');
  const debouncedSearch = useDebounce(searchTerm, 500);
  
  useEffect(() => {
    if (debouncedSearch) {
      // API call only after user stops typing for 500ms
      searchAPI(debouncedSearch);
    }
  }, [debouncedSearch]);
  
  return (
    <input
      value={searchTerm}
      onChange={(e) => setSearchTerm(e.target.value)}
      placeholder="Search..."
    />
  );
}

// useToggle - Boolean state toggle
function useToggle(initialValue = false) {
  const [value, setValue] = useState(initialValue);
  
  const toggle = useCallback(() => {
    setValue(prev => !prev);
  }, []);
  
  return [value, toggle];
}

// Usage
function Modal() {
  const [isOpen, toggleOpen] = useToggle(false);
  
  return (
    <>
      <button onClick={toggleOpen}>Open Modal</button>
      {isOpen && (
        <div className="modal">
          <p>Modal content</p>
          <button onClick={toggleOpen}>Close</button>
        </div>
      )}
    </>
  );
}
```

![Custom Hooks](https://images.unsplash.com/photo-1509228468518-180dd4864904?w=1200&h=600&fit=crop)

---

## ⚠️ Hooks Rules & Common Mistakes

### **Rules of Hooks**
```jsx
// ✅ Rule 1: Only call at top level
function MyComponent() {
  const [count, setCount] = useState(0); // ✅ Top level
  
  if (count > 5) {
    const [name, setName] = useState(''); // ❌ Inside condition
  }
  
  for (let i = 0; i < 10; i++) {
    const [items, setItems] = useState([]); // ❌ Inside loop
  }
  
  function handleClick() {
    const [clicked, setClicked] = useState(false); // ❌ Inside function
  }
}

// ✅ Rule 2: Only call from React functions
function MyComponent() {
  const [count, setCount] = useState(0); // ✅ React component
}

function useMyHook() {
  const [value, setValue] = useState(0); // ✅ Custom hook
}

function regularFunction() {
  const [value, setValue] = useState(0); // ❌ Regular JS function
}
```

### **Common Mistakes**
```jsx
// ❌ Mistake 1: Infinite loop
function BadComponent() {
  const [count, setCount] = useState(0);
  
  useEffect(() => {
    setCount(count + 1); // Runs on every render, causes infinite loop!
  }); // No dependency array
  
  return <div>{count}</div>;
}

// ✅ Fix: Add dependency array
useEffect(() => {
  // Only runs once on mount
}, []);

// ❌ Mistake 2: Stale closure
function Counter() {
  const [count, setCount] = useState(0);
  
  useEffect(() => {
    const interval = setInterval(() => {
      setCount(count + 1); // Always uses initial count value!
    }, 1000);
    
    return () => clearInterval(interval);
  }, []); // Missing 'count' dependency
  
  return <div>{count}</div>;
}

// ✅ Fix: Use functional update
useEffect(() => {
  const interval = setInterval(() => {
    setCount(prev => prev + 1); // Always gets latest value
  }, 1000);
  
  return () => clearInterval(interval);
}, []);

// ❌ Mistake 3: Missing cleanup
function BadComponent() {
  useEffect(() => {
    const subscription = subscribeToAPI();
    // No cleanup! Memory leak!
  }, []);
}

// ✅ Fix: Return cleanup function
useEffect(() => {
  const subscription = subscribeToAPI();
  
  return () => {
    subscription.unsubscribe(); // Cleanup
  };
}, []);
```

---

## 🏗️ Real Project: Before & After Hooks

### **Before Hooks - 300 lines Class Component**
```jsx
class StudentDashboard extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      students: [],
      loading: true,
      error: null,
      searchTerm: '',
      sortBy: 'name',
      filterGpa: null
    };
  }
  
  componentDidMount() {
    this.fetchStudents();
  }
  
  componentDidUpdate(prevProps, prevState) {
    if (
      prevState.searchTerm !== this.state.searchTerm ||
      prevState.sortBy !== this.state.sortBy ||
      prevState.filterGpa !== this.state.filterGpa
    ) {
      this.fetchStudents();
    }
  }
  
  async fetchStudents() {
    this.setState({ loading: true });
    
    try {
      const response = await fetch(
        `/api/students?search=${this.state.searchTerm}&sort=${this.state.sortBy}`
      );
      const data = await response.json();
      
      this.setState({
        students: data,
        loading: false
      });
    } catch (error) {
      this.setState({
        error: error.message,
        loading: false
      });
    }
  }
  
  handleSearch = (e) => {
    this.setState({ searchTerm: e.target.value });
  }
  
  handleSort = (sortBy) => {
    this.setState({ sortBy });
  }
  
  // ... 200 more lines
}
```

### **After Hooks - 150 lines Functional Component**
```jsx
function StudentDashboard() {
  const [searchTerm, setSearchTerm] = useState('');
  const [sortBy, setSortBy] = useState('name');
  const [filterGpa, setFilterGpa] = useState(null);
  
  // Custom hook handles all fetch logic
  const { data: students, loading, error } = useFetch(
    `/api/students?search=${searchTerm}&sort=${sortBy}`
  );
  
  // Debounce search
  const debouncedSearch = useDebounce(searchTerm, 500);
  
  // Filtered students
  const filteredStudents = useMemo(() => {
    if (!students) return [];
    
    return students.filter(s => 
      !filterGpa || s.gpa >= filterGpa
    );
  }, [students, filterGpa]);
  
  if (loading) return <Loader />;
  if (error) return <Error message={error} />;
  
  return (
    <div>
      <SearchBox value={searchTerm} onChange={setSearchTerm} />
      <SortButtons current={sortBy} onSort={setSortBy} />
      <GpaFilter value={filterGpa} onChange={setFilterGpa} />
      
      <StudentList students={filteredStudents} />
    </div>
  );
}

// 50% less code, much cleaner!
```

---

## 📊 Hooks Cheat Sheet

| Hook | Purpose | Example |
|------|---------|---------|
| **useState** | Local state | `const [count, setCount] = useState(0)` |
| **useEffect** | Side effects | `useEffect(() => { ... }, [deps])` |
| **useContext** | Global state | `const value = useContext(MyContext)` |
| **useReducer** | Complex state | `const [state, dispatch] = useReducer(reducer, initial)` |
| **useMemo** | Memoize value | `const value = useMemo(() => compute(), [deps])` |
| **useCallback** | Memoize function | `const fn = useCallback(() => { ... }, [deps])` |
| **useRef** | Mutable ref | `const ref = useRef(null)` |

---

## 💡 Lessons Learned

### **1. Hooks are not "optional"**

In modern React, Hooks are the standard. Class components are legacy.

### **2. Start with useState and useEffect**

Master these two first. 80% of your code will use just these.

### **3. Custom hooks are powerful**

Don't repeat yourself. Extract reusable logic into custom hooks.

### **4. Performance optimization is secondary**

Don't use `useMemo`/`useCallback` everywhere. Only when you have performance issues.

### **5. Rules of Hooks are strict**

ESLint plugin `eslint-plugin-react-hooks` is your friend. Use it!

---

## 📚 Resources

**Official:**
- [React Hooks Documentation](https://react.dev/reference/react)
- [Hooks API Reference](https://react.dev/reference/react/hooks)

**Learn:**
- [React Hooks Course - FreeCodeCamp](https://www.youtube.com/watch?v=dpw9EHDh2bM)
- [useHooks - Hook Recipes](https://usehooks.com/)

**Practice:**
- Build Todo App with Hooks
- Refactor old class components
- Create custom hooks library

---

## 💭 Kết luận

1 năm trước: "Class components work fine."  
6 tháng trước: "Hooks look complicated."  
Hôm nay: "How did I ever live without Hooks?"

**What changed my mind?**

Tôi refactor một 500-line class component thành 200-line functional component với Hooks. Code dễ đọc hơn, ít bugs hơn, dễ test hơn.

Hooks không phải magic. Chúng là:
- ✅ Cleaner way to manage state
- ✅ Better way to handle side effects
- ✅ Easier way to reuse logic
- ✅ Simpler mental model

**Bottom line:**

Nếu bạn đang học React, bỏ qua class components. Học Hooks từ đầu. Nếu bạn đang maintain code cũ, từ từ refactor sang Hooks. Life sẽ đơn giản hơn nhiều.

**From 'this.setState()' to 'useState()' - React has never been simpler. 🚀**

---

**Bạn đã chuyển sang Hooks chưa? Share experience nhé!**

**#React #Hooks #JavaScript #Frontend #WebDevelopment**