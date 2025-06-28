# React JS

---

## **1. Installation using Vite + Tailwind**
### Step 1: Create a Vite + React project  
```bash
npm create vite@latest
```
- Name the project (e.g., `my-vite-app`).  
- Choose **React** and **JavaScript** or **TypeScript**.  

### Step 2: Navigate to the project directory  
```bash
cd my-vite-app
```

### Step 3: Install dependencies  
```bash
npm install
```

### Step 4: Install Tailwind CSS  
```bash
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p
```

### Step 5: Configure Tailwind  
- Update `tailwind.config.js`:  
```js
export default {
  content: ["./index.html", "./src/**/*.{js,ts,jsx,tsx}"],
  theme: { extend: {} },
  plugins: [],
};
```
- Add Tailwind directives to `index.css`:  
```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

### Step 6: Start the development server  
```bash
npm run dev
```

---

## **2. Props**

### Props Destructuring  
```jsx
function Greeting({ name }) {
  return <h1>Hello, {name}!</h1>;
}
// Usage: <Greeting name="Rijoan" />
```

### Props Default Value  
```jsx
function Greeting({ name = "Guest" }) {
  return <h1>Hello, {name}!</h1>;
}
// Usage: <Greeting /> → "Hello, Guest!"
```

### Passing Variables, Functions, and Components  
- **Variable**:  
  ```jsx
  <Avatar src="profile.jpg" />
  ```
- **Function**:  
  ```jsx
  <Button handleClick={() => alert("Clicked!")} />
  ```
- **Component (as children)**:  
  ```jsx
  <Card><h1>Content</h1></Card>
  ```

---

## **3. Conditional Rendering**

### `if-else`  
```jsx
function Greeting({ isLoggedIn }) {
  if (isLoggedIn) return <h1>Welcome back!</h1>;
  else return <h1>Please log in.</h1>;
}
```

### `&&` Operator (Short-circuiting)  
```jsx
{isLoggedIn && <Dashboard />}
```

### `||` Operator (Fallback)  
```jsx
{name || "Anonymous"}
```

### Ternary Operator  
```jsx
{isLoggedIn ? <LogoutButton /> : <LoginButton />}
```

---

## **4. `map()` for Rendering Lists**
```jsx
const numbers = [1, 2, 3];
return (
  <ul>
    {numbers.map((num) => (
      <li key={num}>{num}</li>
    ))}
  </ul>
);
```
- **Key**: Always use a unique `key` prop for list items to optimize React's rendering performance.

---

## **Event Handlers**

### **onClick**

### Non-Parameterized  
```jsx
function ButtonClick() {
  const handleClick = () => alert("Button clicked!");  
  return <button onClick={handleClick}>Click Me</button>;
}
```  

### Parameterized  
```jsx
function ButtonClick() {
  const handleClick = (message) => alert(message);  
  return <button onClick={() => handleClick("Hello!")}>Click Me</button>;
}
```  
- **Key Point**:  
  - Use **inline arrow** (`() => handleClick(param)`) for parameterized functions.  
  - Directly pass **non-parameterized** functions (`onClick={handleClick}`).  

---

## **useState Hook**  
### **Basic Usage**  
```jsx
import { useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0);  
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}
```  
- **Key Points**:  
  - `useState(initialValue)` returns `[state, setState]`.  
  - Always use `setState` to update (never modify directly).  

---

## **Data Fetching**  
### **Using `async/await`**  
```jsx
async function fetchData() {
  const response = await fetch('https://api.example.com/data');
  const data = await response.json();
  return data;
}
```  

### **Using `fetch` in `useEffect`**  
```jsx
import { useState, useEffect } from 'react';

function DataFetcher() {
  const [data, setData] = useState(null);
  
  useEffect(() => {
    fetch('https://api.example.com/data')
      .then(res => res.json())
      .then(data => setData(data));
  }, []); // Empty dependency array = runs once on mount
}
```  

### **Using `use` Hook**  
```jsx
import { use } from 'react';

function UserProfile({ userId }) {
  const data = use(fetch(`/api/users/${userId}`).then(res => res.json()));
  return <div>{data.name}</div>;
}
```  

### **Using `<Suspense>` for Loading States**  
```jsx
import { Suspense } from 'react';

function App() {
  return (
    <Suspense fallback={<div>Loading...</div>}>
      <UserProfile userId={1} />
    </Suspense>
  );
}
```  

### **Data Fetching with `useEffect` (Full Example)**  
```jsx
import { useState, useEffect } from 'react';

function DataFetcher() {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    fetch('https://jsonplaceholder.typicode.com/posts/1')
      .then(res => {
        if (!res.ok) throw new Error('Failed to fetch');
        return res.json();
      })
      .then(data => {
        setData(data);
        setLoading(false);
      })
      .catch(err => {
        setError(err.message);
        setLoading(false);
      });
  }, []);

  if (loading) return <div>Loading...</div>;
  if (error) return <div>Error: {error}</div>;
  return <div>{data.title}</div>;
}
```  


---

## **Local Storage in React**  
### **1. Storing Data**  
Use `localStorage.setItem(key, value)` to store data.  
- **Strings**:  
  ```jsx
  localStorage.setItem('username', 'JohnDoe');
  ```  
- **Objects (convert to JSON)**:  
  ```jsx
  const user = { name: 'John', age: 30 };
  localStorage.setItem('user', JSON.stringify(user));
  ```  

### **2. Loading Data**  
Use `localStorage.getItem(key)` to retrieve data.  
- **Strings**:  
  ```jsx
  const username = localStorage.getItem('username'); // 'JohnDoe'
  ```  
- **Objects (parse JSON)**:  
  ```jsx
  const storedUser = localStorage.getItem('user');
  const user = storedUser ? JSON.parse(storedUser) : null;
  ```  

### **3. Removing Data**  
```jsx
localStorage.removeItem('username'); // Removes a specific item
localStorage.clear(); // Clears all local storage
```  

### **4. Example: React Hook for Local Storage**  
```jsx
import { useState, useEffect } from 'react';

function useLocalStorage(key, initialValue) {
  // Load from localStorage on initial render
  const [value, setValue] = useState(() => {
    const storedValue = localStorage.getItem(key);
    return storedValue ? JSON.parse(storedValue) : initialValue;
  });

  // Update localStorage when value changes
  useEffect(() => {
    localStorage.setItem(key, JSON.stringify(value));
  }, [key, value]);

  return [value, setValue];
}

// Usage
function UserProfile() {
  const [user, setUser] = useLocalStorage('user', { name: '', age: 0 });

  return (
    <div>
      <input 
        value={user.name} 
        onChange={(e) => setUser({ ...user, name: e.target.value })} 
      />
    </div>
  );
}
```  

---

### **Key Takeaways**  
| Action               | Code Example                                  |
|----------------------|---------------------------------------------|
| **Store Data**       | `localStorage.setItem('key', JSON.stringify(data))` |  
| **Load Data**        | `JSON.parse(localStorage.getItem('key'))` |  
| **Remove Data**      | `localStorage.removeItem('key')` |  
| **Clear All Data**   | `localStorage.clear()` |  

**Best Practices**:  
- Always use `JSON.stringify()` for objects/arrays.  
- Handle `null` cases when loading data.  
- Use a **custom hook** (`useLocalStorage`) for reusability.  

--- 



## **React Router Notes**  

---

### **Setup (`createBrowserRouter`, `RouterProvider`)**  
```jsx
import { createBrowserRouter, RouterProvider } from 'react-router-dom';

// Define routes
const router = createBrowserRouter([
  {
    path: "/",
    element: <Home />, // Main component for this route
  },
  {
    path: "/about",
    element: <About />,
  },
]);

// Wrap your app with RouterProvider
function App() {
  return <RouterProvider router={router} />;
}
```  
✅ **Key Points**:  
- `createBrowserRouter` defines all routes.  
- `RouterProvider` makes routing available to the entire app.  

---

### **Nested Routes & `Outlet`**  
```jsx
const router = createBrowserRouter([
  {
    path: "/dashboard",
    element: <DashboardLayout />, // Parent layout
    children: [
      {
        path: "profile", // Becomes "/dashboard/profile"
        element: <Profile />,
      },
      {
        path: "settings", // Becomes "/dashboard/settings"
        element: <Settings />,
      },
    ],
  },
]);

// DashboardLayout.jsx
function DashboardLayout() {
  return (
    <div>
      <h1>Dashboard</h1>
      <Outlet /> {/* Renders nested routes here */}
    </div>
  );
}
```  
✅ **Key Points**:  
- `children` defines nested routes.  
- `<Outlet />` renders the matching child route.  

---

### **`Link` vs `NavLink`**  
```jsx
import { Link, NavLink } from 'react-router-dom';

function Navbar() {
  return (
    <nav>
      <Link to="/">Home</Link> {/* Basic navigation */}
      
      <NavLink
        to="/about"
        style={({ isActive }) => ({ // Style changes if active
          color: isActive ? "red" : "black",
        })}
      >
        About
      </NavLink>
    </nav>
  );
}
```  
✅ **Key Points**:  
- `Link` = Basic navigation.  
- `NavLink` = Adds `isActive` for styling active links.  

---

### **Data Loading (`loader`, `useLoaderData`)**  
```jsx
// Define a loader function (fetches data before rendering)
const router = createBrowserRouter([
  {
    path: "/user/:id",
    element: <UserProfile />,
    loader: async ({ params }) => {
      const res = await fetch(`/api/users/${params.id}`);
      return res.json(); // Data passed to component
    },
  },
]);

// UserProfile.jsx
import { useLoaderData } from 'react-router-dom';

function UserProfile() {
  const user = useLoaderData(); // Access loaded data
  return <div>{user.name}</div>;
}
```  
✅ **Key Points**:  
- `loader` fetches data **before** rendering.  
- `useLoaderData()` accesses the loaded data.  

---

### **Dynamic Routes & `useParams`**  
```jsx
// Route definition with dynamic segment
const router = createBrowserRouter([
  {
    path: "/products/:id", // Dynamic ":id"
    element: <ProductDetails />,
  },
]);

// ProductDetails.jsx
import { useParams } from 'react-router-dom';

function ProductDetails() {
  const { id } = useParams(); // Get URL params
  return <div>Product ID: {id}</div>;
}
```  
✅ **Key Points**:  
- `:id` = Dynamic segment in URL.  
- `useParams()` extracts URL parameters.  

---

### **Programmatic Navigation (`useNavigate`)**  
```jsx
import { useNavigate } from 'react-router-dom';

function LoginButton() {
  const navigate = useNavigate();

  const handleLogin = () => {
    navigate("/dashboard"); // Redirect after login
  };

  return <button onClick={handleLogin}>Login</button>;
}
```  
✅ **Key Points**:  
- `useNavigate()` returns a function for navigation.  
- Can pass state: `navigate("/dashboard", { state: { user } })`.  

---

### **`useNavigation` (Loading States)**  
```jsx
import { useNavigation } from 'react-router-dom';

function SubmitButton() {
  const navigation = useNavigation();
  const isSubmitting = navigation.state === "submitting";

  return (
    <button disabled={isSubmitting}>
      {isSubmitting ? "Submitting..." : "Submit"}
    </button>
  );
}
```  
✅ **Key Points**:  
- `navigation.state` = `"idle" | "loading" | "submitting"`.  
- Useful for loading spinners during navigation.  

---

### **`useLocation` (Access Current URL State)**  
```jsx
import { useLocation } from 'react-router-dom';

function CurrentPath() {
  const location = useLocation();
  return (
    <div>
      Current Path: {location.pathname} <br />
      Search Params: {location.search} <br />
      State: {JSON.stringify(location.state)}
    </div>
  );
}
```  
✅ **Key Points**:  
- `location.pathname` = Current URL path.  
- `location.state` = Passed via `navigate("/path", { state })`.  

---

### **Summary Table**  
| Hook/Component       | Purpose                                      |
|----------------------|---------------------------------------------|
| `createBrowserRouter` | Defines all routes.                         |
| `RouterProvider`     | Makes routing available in the app.         |
| `Outlet`            | Renders nested routes.                      |
| `Link` / `NavLink`  | Navigation with `isActive` support.         |
| `loader`            | Fetches data before rendering.              |
| `useLoaderData`     | Accesses loaded data.                       |
| `useParams`         | Extracts dynamic URL params (`:id`).        |
| `useNavigate`       | Programmatic navigation.                    |
| `useNavigation`     | Tracks loading/submitting states.           |
| `useLocation`       | Accesses current URL & state.               |

🚀 **Best Practices**:  
- Use `loader` for **pre-fetching data**.  
- Use `NavLink` for **active link styling**.  
- Use `useNavigation` for **loading indicators**.  
- Use `useParams` for **dynamic route data**.  




---


---

## **Form Event Handlers**

### **Controlled Form with `onSubmit`, `onChange`, `preventDefault`**
```jsx
import { useState } from 'react';

function LoginForm() {
  const [formData, setFormData] = useState({
    email: '',
    password: '',
  });

  // Handle input changes (controlled component)
  const handleChange = (e) => {
    const { name, value } = e.target;
    setFormData({ ...formData, [name]: value });
  };

  // Handle form submission
  const handleSubmit = (e) => {
    e.preventDefault(); // Prevent page reload
    console.log("Form submitted:", formData);
  };

  return (
    <form onSubmit={handleSubmit}>
      <input
        type="email"
        name="email"
        value={formData.email}
        onChange={handleChange}
        placeholder="Email"
      />
      <input
        type="password"
        name="password"
        value={formData.password}
        onChange={handleChange}
        placeholder="Password"
      />
      <button type="submit">Login</button>
    </form>
  );
}
```
✅ **Key Points**:
- `e.preventDefault()` stops page reload on submit.
- `e.target` accesses the form element.
- **Controlled inputs**: `value` is tied to state.

---

### **Uncontrolled Form with `useRef` & `defaultValue`**
```jsx
import { useRef } from 'react';

function SearchForm() {
  const searchInput = useRef(null);

  const handleSubmit = (e) => {
    e.preventDefault();
    console.log("Search term:", searchInput.current.value);
  };

  return (
    <form onSubmit={handleSubmit}>
      <input
        type="text"
        ref={searchInput}
        defaultValue="react" // Initial value (uncontrolled)
      />
      <button type="submit">Search</button>
    </form>
  );
}
```
✅ **Key Points**:
- `useRef` directly accesses DOM elements.
- `defaultValue` sets initial value (doesn't update state).

---

### **Custom Hook: `useInputField`**
```jsx
import { useState } from 'react';

// Reusable input field hook
function useInputField(initialValue) {
  const [value, setValue] = useState(initialValue);

  const handleChange = (e) => {
    setValue(e.target.value);
  };

  return {
    value,
    onChange: handleChange,
  };
}

function ProfileForm() {
  const name = useInputField("");
  const email = useInputField("");

  const handleSubmit = (e) => {
    e.preventDefault();
    console.log({ name: name.value, email: email.value });
  };

  return (
    <form onSubmit={handleSubmit}>
      <input type="text" placeholder="Name" {...name} />
      <input type="email" placeholder="Email" {...email} />
      <button type="submit">Save</button>
    </form>
  );
}
```
✅ **Key Points**:
- Encapsulates input logic in a reusable hook.
- Spread (`{...name}`) applies `value` and `onChange`.

---

## **Props Drilling Problem**
```jsx
// Grandparent → Parent → Child (prop passing becomes tedious)
function Grandparent() {
  const [user, setUser] = useState("John");

  return <Parent user={user} />;
}

function Parent({ user }) {
  return <Child user={user} />;
}

function Child({ user }) {
  return <p>User: {user}</p>;
}
```
⚠️ **Problem**: Passing props through multiple layers is messy.

---

## **Context API Solution**
### **Step 1: Create & Export Context**
```jsx
import { createContext, useState } from 'react';

// Create context with default value
const UserContext = createContext(null);

export default UserContext;
```

### **Step 2: Provide Context at Top Level**
```jsx
function App() {
  const [user, setUser] = useState("John");

  return (
    <UserContext.Provider value={{ user, setUser }}>
      <Grandparent />
    </UserContext.Provider>
  );
}
```

### **Step 3: Consume Context in Child**
```jsx
import { useContext } from 'react';
import UserContext from './UserContext';

function Child() {
  const { user } = useContext(UserContext); // Access context
  return <p>User: {user}</p>;
}
```

### **Full Example: Theme Switcher**
```jsx
// ThemeContext.js
import { createContext, useState } from 'react';

const ThemeContext = createContext();

export function ThemeProvider({ children }) {
  const [theme, setTheme] = useState("light");

  const toggleTheme = () => {
    setTheme(prev => prev === "light" ? "dark" : "light");
  };

  return (
    <ThemeContext.Provider value={{ theme, toggleTheme }}>
      {children}
    </ThemeContext.Provider>
  );
}

export default ThemeContext;
```

```jsx
// App.js
import { ThemeProvider } from './ThemeContext';

function App() {
  return (
    <ThemeProvider>
      <Header />
    </ThemeProvider>
  );
}
```

```jsx
// Header.js
import { useContext } from 'react';
import ThemeContext from './ThemeContext';

function Header() {
  const { theme, toggleTheme } = useContext(ThemeContext);

  return (
    <header style={{ background: theme === "light" ? "#fff" : "#333" }}>
      <button onClick={toggleTheme}>
        Toggle {theme === "light" ? "Dark" : "Light"} Mode
      </button>
    </header>
  );
}
```
✅ **Key Points**:
- `createContext()` initializes context.
- `Provider` wraps components needing access.
- `useContext` reads the context value.

---

### **Comparison: Props Drilling vs Context API**
| Approach        | Use Case                          | Code Complexity |
|----------------|----------------------------------|----------------|
| **Props Drilling** | Few component layers             | Simple, but messy for deep nesting |
| **Context API**   | App-wide state (theme, user)     | Cleaner, but slightly more setup |

Choose **Context API** when:
- State is needed across many components.
- Prop drilling becomes unmanageable.  
