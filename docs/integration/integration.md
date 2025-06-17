# Redux Toolkit Integration Guide

## Table of Contents
- [Introduction](#introduction)
- [Installation](#installation)
- [Project Structure](#project-structure)
- [Core Concepts](#core-concepts)
- [Implementation Guide](#implementation-guide)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

## Introduction

Redux Toolkit is the official, opinionated, batteries-included toolset for efficient Redux development. It helps you write Redux logic with less boilerplate code and provides utilities to simplify common Redux use cases.

### Why Redux Toolkit?
- Reduces boilerplate code
- Built-in immutable update logic
- Includes Redux Thunk for async logic
- Enables powerful Redux DevTools Extension
- Simplifies Redux setup and configuration

## Installation

```bash
# Install Redux Toolkit and React-Redux
npm install @reduxjs/toolkit react-redux
```

## Project Structure

```
src/
├── redux/
│   ├── store.js
│   └── slices/
│       ├── authSlice.js
│       ├── leadSlice.js
│       └── employeeSlice.js
├── services/
│   └── api/
│       ├── auth.js
│       ├── leads.js
│       └── employees.js
└── components/
    └── ...
```

## Core Concepts

### 1. Store
The Redux store is the central state container that holds the application state.

```javascript
// src/redux/store.js
import { configureStore } from '@reduxjs/toolkit';
import authReducer from './slices/authSlice';
import leadReducer from './slices/leadSlice';
import employeeReducer from './slices/employeeSlice';

export const store = configureStore({
  reducer: {
    auth: authReducer,
    leads: leadReducer,
    employees: employeeReducer,
  },
});
```

### 2. Slices
A slice is a collection of Redux reducer logic and actions for a single feature.

```javascript
// src/redux/slices/leadSlice.js
import { createSlice, createAsyncThunk } from '@reduxjs/toolkit';
import { leadsApi } from '../../services/api/leads';

// Async thunk for fetching leads
export const fetchLeads = createAsyncThunk(
  'leads/fetchLeads',
  async () => {
    const response = await leadsApi.getLeads();
    return response.data;
  }
);

const leadSlice = createSlice({
  name: 'leads',
  initialState: {
    items: [],
    loading: false,
    error: null,
  },
  reducers: {
    // Synchronous reducers
    addLead: (state, action) => {
      state.items.push(action.payload);
    },
    removeLead: (state, action) => {
      state.items = state.items.filter(lead => lead.id !== action.payload);
    },
  },
  extraReducers: (builder) => {
    builder
      .addCase(fetchLeads.pending, (state) => {
        state.loading = true;
      })
      .addCase(fetchLeads.fulfilled, (state, action) => {
        state.loading = false;
        state.items = action.payload;
      })
      .addCase(fetchLeads.rejected, (state, action) => {
        state.loading = false;
        state.error = action.error.message;
      });
  },
});

export const { addLead, removeLead } = leadSlice.actions;
export default leadSlice.reducer;
```

### 3. API Integration
```javascript
// src/services/api/leads.js
import axios from 'axios';

const api = axios.create({
  baseURL: process.env.NEXT_PUBLIC_API_URL,
  headers: {
    'Content-Type': 'application/json',
  },
});

export const leadsApi = {
  getLeads: () => api.get('/leads'),
  createLead: (data) => api.post('/leads', data),
  updateLead: (id, data) => api.put(`/leads/${id}`, data),
  deleteLead: (id) => api.delete(`/leads/${id}`),
};
```

## Implementation Guide

### 1. Setting up the Store
```javascript
// src/redux/store.js
import { configureStore } from '@reduxjs/toolkit';
import { setupListeners } from '@reduxjs/toolkit/query';

export const store = configureStore({
  reducer: {
    // Add your reducers here
  },
  middleware: (getDefaultMiddleware) =>
    getDefaultMiddleware().concat(/* your middleware */),
});

setupListeners(store.dispatch);
```

### 2. Creating a Slice
```javascript
// src/redux/slices/exampleSlice.js
import { createSlice } from '@reduxjs/toolkit';

const initialState = {
  value: 0,
};

export const exampleSlice = createSlice({
  name: 'example',
  initialState,
  reducers: {
    increment: (state) => {
      state.value += 1;
    },
    decrement: (state) => {
      state.value -= 1;
    },
  },
});

export const { increment, decrement } = exampleSlice.actions;
export default exampleSlice.reducer;
```

### 3. Using Redux in Components
```javascript
// src/components/ExampleComponent.js
import { useSelector, useDispatch } from 'react-redux';
import { increment, decrement } from '../redux/slices/exampleSlice';

const ExampleComponent = () => {
  const count = useSelector((state) => state.example.value);
  const dispatch = useDispatch();

  return (
    <div>
      <button onClick={() => dispatch(decrement())}>-</button>
      <span>{count}</span>
      <button onClick={() => dispatch(increment())}>+</button>
    </div>
  );
};
```

## Best Practices

1. **State Structure**
   - Keep state normalized
   - Use selectors for derived data
   - Avoid duplicate data

2. **Async Logic**
   - Use createAsyncThunk for async operations
   - Handle loading and error states
   - Implement proper error handling

3. **Performance**
   - Use memoization for selectors
   - Implement proper cleanup
   - Avoid unnecessary re-renders

4. **Code Organization**
   - Keep slices focused and small
   - Use feature-based folder structure
   - Implement proper TypeScript types

## Troubleshooting

### Common Issues

1. **State Updates Not Reflecting**
   - Check if the reducer is properly connected
   - Verify action payload structure
   - Ensure proper state immutability

2. **Async Operations Not Working**
   - Verify API endpoints
   - Check error handling
   - Ensure proper middleware setup

3. **Performance Issues**
   - Implement proper memoization
   - Check for unnecessary re-renders
   - Use Redux DevTools for debugging

### Debugging Tools

1. **Redux DevTools**
   - Install Redux DevTools Extension
   - Monitor state changes
   - Debug actions and reducers

2. **React Developer Tools**
   - Monitor component re-renders
   - Check props and state
   - Profile performance

## Additional Resources

- [Redux Toolkit Documentation](https://redux-toolkit.js.org/)
- [React-Redux Documentation](https://react-redux.js.org/)
- [Redux Style Guide](https://redux.js.org/style-guide/)
