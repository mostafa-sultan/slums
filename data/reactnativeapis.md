# API Integration in React Native

## Introduction

Integrating with APIs is common in React Native apps. This tutorial covers fetching data, handling responses, and error handling.

---

## Fetch API

    async function fetchData() {
      try {
        const response = await fetch('https://api.example.com/data');
        const data = await response.json();
        return data;
      } catch (error) {
        console.error('Error:', error);
      }
    }

---

## Axios

    import axios from 'axios';
    
    const response = await axios.get('https://api.example.com/data');
    const data = response.data;

---

## Conclusion

Use fetch or axios for API calls. Handle loading states and errors properly for better user experience.

