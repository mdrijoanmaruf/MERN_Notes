# Firebase

## **1. Firebase Setup**

### **Step 1: Create a Firebase Project**
1. Go to [Firebase Console](https://console.firebase.google.com/)
2. Click "Create a project"
3. Enter project name and follow setup steps
4. Enable Google Analytics (optional)

### **Step 2: Add Firebase to Your Web App**
1. In the Firebase console, click "Add app" and select Web
2. Register your app with a nickname
3. Copy the Firebase configuration object

### **Step 3: Install Firebase SDK**
```bash
npm install firebase
```

### **Step 4: Initialize Firebase in Your Project**
```js
// firebase.js
import { initializeApp } from 'firebase/app';
import { getAuth } from 'firebase/auth';
import { getFirestore } from 'firebase/firestore';

const firebaseConfig = {
  apiKey: "your-api-key",
  authDomain: "your-project.firebaseapp.com",
  projectId: "your-project-id",
  storageBucket: "your-project.appspot.com",
  messagingSenderId: "123456789",
  appId: "your-app-id"
};

// Initialize Firebase
const app = initializeApp(firebaseConfig);

// Initialize Firebase Authentication and get a reference to the service
export const auth = getAuth(app);

// Initialize Cloud Firestore and get a reference to the service
export const db = getFirestore(app);

export default app;
```

---

## **2. Firebase Authentication**

### **Enable Authentication Methods**
1. Go to Firebase Console → Authentication → Sign-in method
2. Enable the providers you want to use:
   - Email/Password
   - Google
   - GitHub
   - etc.

### **Email/Password Registration**
```js
import { createUserWithEmailAndPassword, updateProfile } from 'firebase/auth';
import { auth } from './firebase';

const registerUser = async (email, password, displayName) => {
  try {
    // Create user account
    const userCredential = await createUserWithEmailAndPassword(auth, email, password);
    const user = userCredential.user;
    
    // Update user profile with display name
    await updateProfile(user, {
      displayName: displayName
    });
    
    console.log("User registered successfully:", user);
    return user;
  } catch (error) {
    console.error("Registration error:", error.message);
    throw error;
  }
};

// Usage
registerUser("user@example.com", "password123", "John Doe");
```

### **Email/Password Login**
```js
import { signInWithEmailAndPassword } from 'firebase/auth';
import { auth } from './firebase';

const loginUser = async (email, password) => {
  try {
    const userCredential = await signInWithEmailAndPassword(auth, email, password);
    const user = userCredential.user;
    console.log("User logged in successfully:", user);
    return user;
  } catch (error) {
    console.error("Login error:", error.message);
    throw error;
  }
};

// Usage
loginUser("user@example.com", "password123");
```

### **Google Authentication**
```js
import { signInWithPopup, GoogleAuthProvider } from 'firebase/auth';
import { auth } from './firebase';

const provider = new GoogleAuthProvider();

const signInWithGoogle = async () => {
  try {
    const result = await signInWithPopup(auth, provider);
    const credential = GoogleAuthProvider.credentialFromResult(result);
    const token = credential.accessToken;
    const user = result.user;
    
    console.log("Google sign-in successful:", user);
    return user;
  } catch (error) {
    console.error("Google sign-in error:", error.message);
    throw error;
  }
};

// Usage
signInWithGoogle();
```

### **GitHub Authentication**
```js
import { signInWithPopup, GithubAuthProvider } from 'firebase/auth';
import { auth } from './firebase';

const provider = new GithubAuthProvider();

const signInWithGitHub = async () => {
  try {
    const result = await signInWithPopup(auth, provider);
    const credential = GithubAuthProvider.credentialFromResult(result);
    const token = credential.accessToken;
    const user = result.user;
    
    console.log("GitHub sign-in successful:", user);
    return user;
  } catch (error) {
    console.error("GitHub sign-in error:", error.message);
    throw error;
  }
};

// Usage
signInWithGitHub();
```

### **Password Reset (Forgot Password)**
```js
import { sendPasswordResetEmail } from 'firebase/auth';
import { auth } from './firebase';

const resetPassword = async (email) => {
  try {
    await sendPasswordResetEmail(auth, email);
    console.log("Password reset email sent successfully");
    alert("Password reset email sent! Check your inbox.");
  } catch (error) {
    console.error("Password reset error:", error.message);
    throw error;
  }
};

// Usage
resetPassword("user@example.com");
```

### **User Logout**
```js
import { signOut } from 'firebase/auth';
import { auth } from './firebase';

const logoutUser = async () => {
  try {
    await signOut(auth);
    console.log("User logged out successfully");
  } catch (error) {
    console.error("Logout error:", error.message);
    throw error;
  }
};

// Usage
logoutUser();
```

### **Monitor Authentication State**
```js
import { onAuthStateChanged } from 'firebase/auth';
import { auth } from './firebase';

// Listen for authentication state changes
onAuthStateChanged(auth, (user) => {
  if (user) {
    // User is signed in
    console.log("User is logged in:", user);
    console.log("User ID:", user.uid);
    console.log("Email:", user.email);
    console.log("Display Name:", user.displayName);
  } else {
    // User is signed out
    console.log("User is logged out");
  }
});
```

---

## **3. Firestore Database**

### **Enable Firestore**
1. Go to Firebase Console → Firestore Database
2. Click "Create database"
3. Choose security rules (Start in test mode for development)

### **Basic Firestore Operations**

#### **Add Data to Collection**
```js
import { collection, addDoc, doc, setDoc } from 'firebase/firestore';
import { db } from './firebase';

// Add document with auto-generated ID
const addUser = async (userData) => {
  try {
    const docRef = await addDoc(collection(db, "users"), userData);
    console.log("Document written with ID: ", docRef.id);
    return docRef.id;
  } catch (error) {
    console.error("Error adding document: ", error);
    throw error;
  }
};

// Add document with custom ID
const addUserWithCustomId = async (userId, userData) => {
  try {
    await setDoc(doc(db, "users", userId), userData);
    console.log("Document written with custom ID: ", userId);
  } catch (error) {
    console.error("Error adding document: ", error);
    throw error;
  }
};

// Usage
addUser({
  name: "John Doe",
  email: "john@example.com",
  age: 30,
  createdAt: new Date()
});

addUserWithCustomId("user123", {
  name: "Jane Smith",
  email: "jane@example.com",
  age: 25
});
```

#### **Read Data from Collection**
```js
import { collection, getDocs, doc, getDoc, query, where, orderBy } from 'firebase/firestore';
import { db } from './firebase';

// Get all documents from a collection
const getAllUsers = async () => {
  try {
    const querySnapshot = await getDocs(collection(db, "users"));
    const users = [];
    querySnapshot.forEach((doc) => {
      users.push({ id: doc.id, ...doc.data() });
    });
    console.log("All users:", users);
    return users;
  } catch (error) {
    console.error("Error getting documents: ", error);
    throw error;
  }
};

// Get a specific document
const getUser = async (userId) => {
  try {
    const docRef = doc(db, "users", userId);
    const docSnap = await getDoc(docRef);
    
    if (docSnap.exists()) {
      console.log("Document data:", docSnap.data());
      return { id: docSnap.id, ...docSnap.data() };
    } else {
      console.log("No such document!");
      return null;
    }
  } catch (error) {
    console.error("Error getting document: ", error);
    throw error;
  }
};

// Query with conditions
const getUsersByAge = async (minAge) => {
  try {
    const q = query(
      collection(db, "users"), 
      where("age", ">=", minAge),
      orderBy("age")
    );
    const querySnapshot = await getDocs(q);
    const users = [];
    querySnapshot.forEach((doc) => {
      users.push({ id: doc.id, ...doc.data() });
    });
    return users;
  } catch (error) {
    console.error("Error querying documents: ", error);
    throw error;
  }
};

// Usage
getAllUsers();
getUser("user123");
getUsersByAge(25);
```

#### **Update Data**
```js
import { doc, updateDoc } from 'firebase/firestore';
import { db } from './firebase';

const updateUser = async (userId, updates) => {
  try {
    const userRef = doc(db, "users", userId);
    await updateDoc(userRef, updates);
    console.log("Document updated successfully");
  } catch (error) {
    console.error("Error updating document: ", error);
    throw error;
  }
};

// Usage
updateUser("user123", {
  age: 26,
  lastUpdated: new Date()
});
```

#### **Delete Data**
```js
import { doc, deleteDoc } from 'firebase/firestore';
import { db } from './firebase';

const deleteUser = async (userId) => {
  try {
    await deleteDoc(doc(db, "users", userId));
    console.log("Document deleted successfully");
  } catch (error) {
    console.error("Error deleting document: ", error);
    throw error;
  }
};

// Usage
deleteUser("user123");
```

#### **Real-time Listeners**
```js
import { collection, onSnapshot, doc } from 'firebase/firestore';
import { db } from './firebase';

// Listen to all documents in a collection
const listenToUsers = () => {
  const unsubscribe = onSnapshot(collection(db, "users"), (snapshot) => {
    const users = [];
    snapshot.forEach((doc) => {
      users.push({ id: doc.id, ...doc.data() });
    });
    console.log("Current users: ", users);
  });
  
  // Call unsubscribe() to stop listening
  return unsubscribe;
};

// Listen to a specific document
const listenToUser = (userId) => {
  const unsubscribe = onSnapshot(doc(db, "users", userId), (doc) => {
    if (doc.exists()) {
      console.log("Current user data: ", doc.data());
    } else {
      console.log("User not found");
    }
  });
  
  return unsubscribe;
};

// Usage
const stopListening = listenToUsers();
// Later, stop listening
// stopListening();
```

---

## **4. Firebase Hosting**

### **Install Firebase CLI**
```bash
npm install -g firebase-tools
```

### **Login to Firebase**
```bash
firebase login
```

### **Initialize Firebase Hosting**
```bash
# Navigate to your project directory
cd your-project

# Initialize Firebase
firebase init

# Select Hosting from the options
# Choose your Firebase project
# Set public directory (usually 'build' for React, 'dist' for Vite)
# Configure as single-page app (y/N): Yes (for React apps)
# Set up automatic builds and deploys with GitHub: Optional
```

### **Build Your Project**
```bash
# For React apps
npm run build

# For Vite apps
npm run build
```

### **Deploy to Firebase Hosting**
```bash
# Deploy to live channel
firebase deploy

# Deploy to preview channel
firebase hosting:channel:deploy preview-name

# Deploy only hosting
firebase deploy --only hosting
```

### **Firebase Hosting Configuration**
```json
// firebase.json
{
  "hosting": {
    "public": "build",
    "ignore": [
      "firebase.json",
      "**/.*",
      "**/node_modules/**"
    ],
    "rewrites": [
      {
        "source": "**",
        "destination": "/index.html"
      }
    ],
    "headers": [
      {
        "source": "**/*.@(eot|otf|ttf|ttc|woff|font.css)",
        "headers": [
          {
            "key": "Access-Control-Allow-Origin",
            "value": "*"
          }
        ]
      }
    ]
  }
}
```

### **Custom Domain Setup**
1. Go to Firebase Console → Hosting
2. Click "Add custom domain"
3. Enter your domain name
4. Follow DNS configuration instructions
5. Wait for SSL certificate provisioning

---

## **5. Complete React + Firebase Example with AuthContext**

### **AuthContext Setup**
```jsx
// AuthContext.js
import React, { createContext, useContext, useState, useEffect } from 'react';
import { 
  createUserWithEmailAndPassword,
  signInWithEmailAndPassword,
  signOut,
  onAuthStateChanged,
  sendPasswordResetEmail,
  signInWithPopup,
  GoogleAuthProvider,
  GithubAuthProvider,
  updateProfile
} from 'firebase/auth';
import { auth } from './firebase';

const AuthContext = createContext();

export const useAuth = () => {
  const context = useContext(AuthContext);
  if (!context) {
    throw new Error('useAuth must be used within an AuthProvider');
  }
  return context;
};

export const AuthProvider = ({ children }) => {
  const [user, setUser] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  // Authentication functions
  const register = async (email, password, displayName) => {
    setLoading(true);
    setError(null);
    try {
      const userCredential = await createUserWithEmailAndPassword(auth, email, password);
      const user = userCredential.user;
      
      if (displayName) {
        await updateProfile(user, { displayName });
      }
      
      return user;
    } catch (error) {
      setError(error.message);
      throw error;
    } finally {
      setLoading(false);
    }
  };

  const login = async (email, password) => {
    setLoading(true);
    setError(null);
    try {
      const userCredential = await signInWithEmailAndPassword(auth, email, password);
      return userCredential.user;
    } catch (error) {
      setError(error.message);
      throw error;
    } finally {
      setLoading(false);
    }
  };

  const logout = async () => {
    setLoading(true);
    setError(null);
    try {
      await signOut(auth);
    } catch (error) {
      setError(error.message);
      throw error;
    } finally {
      setLoading(false);
    }
  };

  const resetPassword = async (email) => {
    setError(null);
    try {
      await sendPasswordResetEmail(auth, email);
    } catch (error) {
      setError(error.message);
      throw error;
    }
  };

  const signInWithGoogle = async () => {
    setLoading(true);
    setError(null);
    try {
      const provider = new GoogleAuthProvider();
      const result = await signInWithPopup(auth, provider);
      return result.user;
    } catch (error) {
      setError(error.message);
      throw error;
    } finally {
      setLoading(false);
    }
  };

  const signInWithGitHub = async () => {
    setLoading(true);
    setError(null);
    try {
      const provider = new GithubAuthProvider();
      const result = await signInWithPopup(auth, provider);
      return result.user;
    } catch (error) {
      setError(error.message);
      throw error;
    } finally {
      setLoading(false);
    }
  };

  const clearError = () => setError(null);

  useEffect(() => {
    const unsubscribe = onAuthStateChanged(auth, (user) => {
      setUser(user);
      setLoading(false);
    });

    return unsubscribe;
  }, []);

  const value = {
    user,
    loading,
    error,
    register,
    login,
    logout,
    resetPassword,
    signInWithGoogle,
    signInWithGitHub,
    clearError
  };

  return (
    <AuthContext.Provider value={value}>
      {children}
    </AuthContext.Provider>
  );
};
```

### **App.js with AuthProvider**
```jsx
// App.js
import React from 'react';
import { AuthProvider } from './AuthContext';
import AuthComponent from './AuthComponent';
import Dashboard from './Dashboard';
import { useAuth } from './AuthContext';

function AppContent() {
  const { user, loading } = useAuth();

  if (loading) {
    return <div>Loading...</div>;
  }

  return user ? <Dashboard /> : <AuthComponent />;
}

function App() {
  return (
    <AuthProvider>
      <div className="App">
        <AppContent />
      </div>
    </AuthProvider>
  );
}

export default App;
```

### **Auth Component Using AuthContext**
```jsx
// AuthComponent.js
import React, { useState } from 'react';
import { useAuth } from './AuthContext';

function AuthComponent() {
  const { login, register, resetPassword, signInWithGoogle, signInWithGitHub, error, loading, clearError } = useAuth();
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');
  const [displayName, setDisplayName] = useState('');
  const [isRegistering, setIsRegistering] = useState(false);
  const [isResetting, setIsResetting] = useState(false);

  const handleSubmit = async (e) => {
    e.preventDefault();
    clearError();

    try {
      if (isResetting) {
        await resetPassword(email);
        alert('Password reset email sent! Check your inbox.');
        setIsResetting(false);
        setEmail('');
      } else if (isRegistering) {
        await register(email, password, displayName);
        alert('Registration successful!');
      } else {
        await login(email, password);
        alert('Login successful!');
      }
    } catch (error) {
      console.error('Authentication error:', error);
    }
  };

  const handleGoogleSignIn = async () => {
    clearError();
    try {
      await signInWithGoogle();
      alert('Google sign-in successful!');
    } catch (error) {
      console.error('Google sign-in error:', error);
    }
  };

  const handleGitHubSignIn = async () => {
    clearError();
    try {
      await signInWithGitHub();
      alert('GitHub sign-in successful!');
    } catch (error) {
      console.error('GitHub sign-in error:', error);
    }
  };

  const switchMode = (mode) => {
    clearError();
    setIsRegistering(mode === 'register');
    setIsResetting(mode === 'reset');
    setEmail('');
    setPassword('');
    setDisplayName('');
  };

  return (
    <div style={{ maxWidth: '400px', margin: '0 auto', padding: '20px' }}>
      <h2>
        {isResetting ? 'Reset Password' : isRegistering ? 'Register' : 'Login'}
      </h2>

      {error && (
        <div style={{ color: 'red', marginBottom: '10px' }}>
          {error}
        </div>
      )}

      <form onSubmit={handleSubmit}>
        <input
          type="email"
          placeholder="Email"
          value={email}
          onChange={(e) => setEmail(e.target.value)}
          required
          style={{ width: '100%', padding: '10px', marginBottom: '10px' }}
        />

        {!isResetting && (
          <input
            type="password"
            placeholder="Password"
            value={password}
            onChange={(e) => setPassword(e.target.value)}
            required
            style={{ width: '100%', padding: '10px', marginBottom: '10px' }}
          />
        )}

        {isRegistering && (
          <input
            type="text"
            placeholder="Display Name (optional)"
            value={displayName}
            onChange={(e) => setDisplayName(e.target.value)}
            style={{ width: '100%', padding: '10px', marginBottom: '10px' }}
          />
        )}

        <button 
          type="submit" 
          disabled={loading}
          style={{ width: '100%', padding: '10px', marginBottom: '10px' }}
        >
          {loading ? 'Processing...' : 
           isResetting ? 'Send Reset Email' : 
           isRegistering ? 'Register' : 'Login'}
        </button>
      </form>

      {!isResetting && (
        <div>
          <button 
            onClick={handleGoogleSignIn}
            disabled={loading}
            style={{ width: '100%', padding: '10px', marginBottom: '10px', backgroundColor: '#db4437', color: 'white' }}
          >
            {loading ? 'Processing...' : 'Sign in with Google'}
          </button>

          <button 
            onClick={handleGitHubSignIn}
            disabled={loading}
            style={{ width: '100%', padding: '10px', marginBottom: '10px', backgroundColor: '#333', color: 'white' }}
          >
            {loading ? 'Processing...' : 'Sign in with GitHub'}
          </button>
        </div>
      )}

      <div style={{ textAlign: 'center' }}>
        {!isResetting ? (
          <>
            <button 
              onClick={() => switchMode(isRegistering ? 'login' : 'register')}
              style={{ background: 'none', border: 'none', color: 'blue', cursor: 'pointer' }}
            >
              {isRegistering ? 'Switch to Login' : 'Switch to Register'}
            </button>
            <br />
            <button 
              onClick={() => switchMode('reset')}
              style={{ background: 'none', border: 'none', color: 'blue', cursor: 'pointer' }}
            >
              Forgot Password?
            </button>
          </>
        ) : (
          <button 
            onClick={() => switchMode('login')}
            style={{ background: 'none', border: 'none', color: 'blue', cursor: 'pointer' }}
          >
            Back to Login
          </button>
        )}
      </div>
    </div>
  );
}

export default AuthComponent;
```

### **Dashboard Component**
```jsx
// Dashboard.js
import React from 'react';
import { useAuth } from './AuthContext';

function Dashboard() {
  const { user, logout, loading } = useAuth();

  const handleLogout = async () => {
    try {
      await logout();
      alert('Logged out successfully!');
    } catch (error) {
      console.error('Logout error:', error);
      alert('Logout failed: ' + error.message);
    }
  };

  return (
    <div style={{ maxWidth: '600px', margin: '0 auto', padding: '20px' }}>
      <h2>Welcome to Dashboard!</h2>
      <div style={{ backgroundColor: '#f5f5f5', padding: '15px', borderRadius: '5px', marginBottom: '20px' }}>
        <h3>User Information:</h3>
        <p><strong>Email:</strong> {user?.email}</p>
        <p><strong>Display Name:</strong> {user?.displayName || 'Not set'}</p>
        <p><strong>User ID:</strong> {user?.uid}</p>
        <p><strong>Email Verified:</strong> {user?.emailVerified ? 'Yes' : 'No'}</p>
        <p><strong>Last Sign In:</strong> {user?.metadata?.lastSignInTime}</p>
      </div>
      
      <button 
        onClick={handleLogout}
        disabled={loading}
        style={{ padding: '10px 20px', backgroundColor: '#dc3545', color: 'white', border: 'none', borderRadius: '5px' }}
      >
        {loading ? 'Logging out...' : 'Logout'}
      </button>
    </div>
  );
}

export default Dashboard;
```

### **Benefits of Using AuthContext**

#### **Advantages:**
- **Centralized State Management**: All authentication logic in one place
- **Global Access**: Access user data from any component without prop drilling
- **Consistent Loading States**: Unified loading and error handling
- **Easy Testing**: Mock authentication state for testing
- **Scalability**: Easy to extend with additional auth features

#### **AuthContext Best Practices:**
```jsx
// 1. Always check if user is authenticated before accessing user data
const { user } = useAuth();
if (!user) return <LoginComponent />;

// 2. Handle loading states appropriately
const { user, loading } = useAuth();
if (loading) return <LoadingSpinner />;

// 3. Clear errors when switching between auth modes
const { clearError } = useAuth();
const switchToRegister = () => {
  clearError();
  setMode('register');
};

// 4. Use try-catch for async operations
const handleLogin = async () => {
  try {
    await login(email, password);
    // Success handling
  } catch (error) {
    // Error is already handled by context
    console.error('Login failed:', error);
  }
};
```

### **Advanced AuthContext Features**
```jsx
// Enhanced AuthContext with additional features
export const AuthProvider = ({ children }) => {
  const [user, setUser] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  // Email verification
  const sendEmailVerification = async () => {
    if (user) {
      await sendEmailVerification(user);
    }
  };

  // Update user profile
  const updateUserProfile = async (profileData) => {
    if (user) {
      await updateProfile(user, profileData);
      setUser({ ...user, ...profileData });
    }
  };

  // Delete user account
  const deleteAccount = async () => {
    if (user) {
      await deleteUser(user);
      setUser(null);
    }
  };

  // Check if user has specific role (if using custom claims)
  const hasRole = (role) => {
    return user?.customClaims?.[role] === true;
  };

  const value = {
    user,
    loading,
    error,
    register,
    login,
    logout,
    resetPassword,
    signInWithGoogle,
    signInWithGitHub,
    sendEmailVerification,
    updateUserProfile,
    deleteAccount,
    hasRole,
    clearError
  };

  return (
    <AuthContext.Provider value={value}>
      {children}
    </AuthContext.Provider>
  );
};
```

---

## **6. Security Rules**

### **Firestore Security Rules**
```javascript
// firestore.rules
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // Users can only read/write their own data
    match /users/{userId} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
    }
    
    // Public read access, authenticated write access
    match /posts/{document} {
      allow read: if true;
      allow write: if request.auth != null;
    }
  }
}
```

### **Firebase Auth Security**
```javascript
// Example: Verify user on server-side (Node.js)
const admin = require('firebase-admin');

const verifyIdToken = async (idToken) => {
  try {
    const decodedToken = await admin.auth().verifyIdToken(idToken);
    const uid = decodedToken.uid;
    return { success: true, uid };
  } catch (error) {
    return { success: false, error: error.message };
  }
};
```

---

## **7. Best Practices**

### **Environment Variables**
```env
# .env
REACT_APP_FIREBASE_API_KEY=your-api-key
REACT_APP_FIREBASE_AUTH_DOMAIN=your-project.firebaseapp.com
REACT_APP_FIREBASE_PROJECT_ID=your-project-id
REACT_APP_FIREBASE_STORAGE_BUCKET=your-project.appspot.com
REACT_APP_FIREBASE_MESSAGING_SENDER_ID=123456789
REACT_APP_FIREBASE_APP_ID=your-app-id
```

```js
// firebase.js with environment variables
const firebaseConfig = {
  apiKey: process.env.REACT_APP_FIREBASE_API_KEY,
  authDomain: process.env.REACT_APP_FIREBASE_AUTH_DOMAIN,
  projectId: process.env.REACT_APP_FIREBASE_PROJECT_ID,
  storageBucket: process.env.REACT_APP_FIREBASE_STORAGE_BUCKET,
  messagingSenderId: process.env.REACT_APP_FIREBASE_MESSAGING_SENDER_ID,
  appId: process.env.REACT_APP_FIREBASE_APP_ID
};
```

### **Error Handling**
```js
const handleFirebaseError = (error) => {
  switch (error.code) {
    case 'auth/user-not-found':
      return 'No user found with this email.';
    case 'auth/wrong-password':
      return 'Incorrect password.';
    case 'auth/email-already-in-use':
      return 'Email is already registered.';
    case 'auth/weak-password':
      return 'Password should be at least 6 characters.';
    case 'auth/invalid-email':
      return 'Invalid email address.';
    default:
      return error.message;
  }
};
```