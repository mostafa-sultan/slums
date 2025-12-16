# React Router Complete Guide

## Introduction

React Router enables navigation in React applications. This tutorial covers routing, navigation, protected routes, and best practices.

---

## Basic Setup

    import { BrowserRouter, Routes, Route } from 'react-router-dom';
    
    function App() {
      return (
        <BrowserRouter>
          <Routes>
            <Route path="/" element={<Home />} />
            <Route path="/about" element={<About />} />
            <Route path="/users/:id" element={<User />} />
          </Routes>
        </BrowserRouter>
      );
    }

---

## Navigation

    import { Link, useNavigate } from 'react-router-dom';
    
    function Navigation() {
      const navigate = useNavigate();
      
      return (
        <nav>
          <Link to="/">Home</Link>
          <Link to="/about">About</Link>
          <button onClick={() => navigate('/users')}>Go to Users</button>
        </nav>
      );
    }

---

## Protected Routes

    function ProtectedRoute({ children }) {
      const isAuthenticated = useAuth();
      
      if (!isAuthenticated) {
        return <Navigate to="/login" />;
      }
      
      return children;
    }

---

## Conclusion

React Router is essential for single-page applications. Use it to create navigation and handle routing in your React apps.

