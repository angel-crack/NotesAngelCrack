---
title: BDB Generic Request Documentation
description: Comprehensive guide to the bdbGenericRequest function with TypeScript generics and examples
---

import MermaidLoader from '@site/src/components/MermaidLoader';

## Table of Contents
1. [Overview](#overview)
2. [Architecture](#architecture)
3. [Core Function: `bdbGenericRequest`](#core-function-bdbgenericrequest)
4. [TypeScript Generic Types Explained](#typescript-generic-types-explained)
5. [Implementation in `api.service.ts`](#implementation-in-apiservicets)
6. [Response Flow](#response-flow)
7. [Practical Examples](#practical-examples)
8. [Best Practices](#best-practices)

---

## Overview

The `bdbGenericRequest` function is a **generic, reusable HTTP client** designed to communicate with a BDB (Backend Database) API. It provides a type-safe way to make POST requests to a remote function executor that processes Python functions on the backend.

### Key Features:
- **Type Safety**: Full TypeScript generic support for request/response types
- **Flexibility**: Single function handles all API calls
- **Centralized Configuration**: Environment-based URL and dev mode settings
- **Automatic Parsing**: Handles complex nested response structures
- **Error Handling**: Built on Axios for robust HTTP error management

---

## Architecture

---

## Core Function: `bdbGenericRequest`

### Function Signature

```typescript
async function bdbGenericRequest<T, U = unknown>(
  functionName: string, 
  input_data: U = [] as unknown as U
): Promise<T>
```

### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `functionName` | `string` | ✅ Yes | The name of the Python function to execute on the backend (e.g., `'read_deffective_rma_reports'`) |
| `input_data` | `U` | ❌ No | Input data to pass to the function. Defaults to empty array. Type is inferred from generic `U` |

### Return Type
- **`Promise<T>`**: Returns a promise that resolves to type `T`, which represents the expected response data structure

### Implementation Details

```typescript
export async function bdbGenericRequest<T, U = unknown>(
  functionName: string, 
  input_data: U = [] as unknown as U
): Promise<T> {
  // 1. Get API base URL from environment variables
  const url = import.meta.env.VITE_APP_API_BASE_URL;
  
  // 2. Set request headers
  const headers = {
    'accept': '*/*',
    'content-type': 'application/json;charset=utf-8',
  };
  
  // 3. Construct request body
  const body = {
    input: { 
      function: functionName,  // Backend function name
      input_data               // Function parameters
    },
    dev: import.meta.env.VITE_APP_DEV_MODE  // Environment flag
  };
  
  // 4. Make HTTP POST request
  const response = await axios.post<BDBResponseInterface>(url, body, { headers });
  
  // 5. Parse and return the result
  return GetParsedResult<T>(response.data);
}
```

### Request Body Structure

The function sends a POST request with the following JSON structure:

```json
{
  "input": {
    "function": "read_deffective_rma_reports",
    "input_data": []
  },
  "dev": true
}
```

---

## TypeScript Generic Types Explained

### What Are Generics?

Generics in TypeScript allow you to create reusable components that work with multiple types while maintaining type safety. Think of them as **type variables** or **placeholders** for actual types.

### Generic Type Parameters in `bdbGenericRequest`

#### **`T` - Response Type (Required)**
- **Purpose**: Defines the **expected return type** from the API
- **Usage**: Tells TypeScript what data structure to expect after parsing the response
- **Example**: `ReportsInterface[]`, `commentsInterface`, `DefectiveUpdateResponseInterface[]`

#### **`U` - Input Type (Optional, defaults to `unknown`)**
- **Purpose**: Defines the **type of input data** being sent to the backend
- **Default**: `unknown` (accepts any type)
- **Usage**: Ensures type safety for the `input_data` parameter
- **Example**: `DefectiveUpdateInterface[]`, `commentsCreateInterface`

### How Generics Work: Visual Example

```typescript
// Generic function definition
bdbGenericRequest<T, U>(functionName: string, input_data: U): Promise<T>
                  │  │                                    │           │
                  │  └────────────────────────────────────┘           │
                  │         Input data type                           │
                  └───────────────────────────────────────────────────┘
                            Response data type

// Concrete usage example
bdbGenericRequest<ReportsInterface[], DefectiveUpdateInterface[]>(
  'update_deffective_rma_reports',
  updateData
)

// TypeScript infers:
// T = ReportsInterface[]           ← What you GET back
// U = DefectiveUpdateInterface[]   ← What you SEND
```

### Type Inference

TypeScript can often **infer** generic types automatically:

```typescript
// Explicit (you specify types)
const reports = await bdbGenericRequest<ReportsInterface[]>('read_deffective_rma_reports');

// Implicit (TypeScript infers from context)
async function fetchReports(): Promise<ReportsInterface[]> {
  return bdbGenericRequest('read_deffective_rma_reports');
  // TypeScript knows the return type must be ReportsInterface[]
}
```

---

## Implementation in `api.service.ts`

The `api.service.ts` file provides **specific, named functions** that wrap `bdbGenericRequest` for each API operation. This creates a **clean, domain-specific API** for the rest of the application.

### Pattern Overview

```typescript
// General Pattern:
export const functionName = async (input?: InputType) => 
  bdbGenericRequest<ResponseType, InputType>('backend_function_name', input);
```

### Example 1: Simple GET Request (No Input)

```typescript
export const fetchDeffectiveRMAReport = async () => 
  bdbGenericRequest<ReportsInterface[]>('read_deffective_rma_reports');
```

**Breakdown:**
- **Function**: `fetchDeffectiveRMAReport`
- **Generic `T`**: `ReportsInterface[]` - Expects an array of report objects
- **Generic `U`**: Not specified (defaults to `unknown`, but not needed since no input)
- **Backend Function**: `'read_deffective_rma_reports'`
- **Input Data**: None (omitted)
- **Returns**: `Promise<ReportsInterface[]>`

**Usage:**
```typescript
const reports = await fetchDeffectiveRMAReport();
// reports is typed as ReportsInterface[]
console.log(reports[0].rmaNumber); // ✅ Type-safe access
```

### Example 2: POST Request with Input Data

```typescript
export const updateDefectiveRMAReport = async (input_data: DefectiveUpdateInterface[]) => 
  bdbGenericRequest<DefectiveUpdateResponseInterface[], DefectiveUpdateInterface[]>(
    'update_deffective_rma_reports', 
    input_data
  );
```

**Breakdown:**
- **Function**: `updateDefectiveRMAReport`
- **Generic `T`**: `DefectiveUpdateResponseInterface[]` - Response type
- **Generic `U`**: `DefectiveUpdateInterface[]` - Input type
- **Backend Function**: `'update_deffective_rma_reports'`
- **Input Data**: `input_data` parameter (required)
- **Returns**: `Promise<DefectiveUpdateResponseInterface[]>`

**Usage:**
```typescript
const updateData: DefectiveUpdateInterface[] = [{
  filter: { _id: '507f1f77bcf86cd799439011' },
  update: { status: 'closed' }
}];

const result = await updateDefectiveRMAReport(updateData);
// result is typed as DefectiveUpdateResponseInterface[]
console.log(result[0].modified_count); // ✅ Type-safe access
```

### Example 3: Single Object Input

```typescript
export const createDefectiveRMAComment = async (input_data: commentsCreateInterface) => 
  bdbGenericRequest<commentsInterface, commentsCreateInterface>(
    'create_comments', 
    input_data
  );
```

**Breakdown:**
- **Function**: `createDefectiveRMAComment`
- **Generic `T`**: `commentsInterface` - Single comment object response
- **Generic `U`**: `commentsCreateInterface` - Single comment creation object
- **Backend Function**: `'create_comments'`
- **Input Data**: `input_data` parameter (required)
- **Returns**: `Promise<commentsInterface>`

**Usage:**
```typescript
const newComment: commentsCreateInterface = {
  comments: 'Issue resolved after testing',
  report_id: '507f1f77bcf86cd799439011'
};

const createdComment = await createDefectiveRMAComment(newComment);
// createdComment is typed as commentsInterface
console.log(createdComment._id.$oid); // ✅ Type-safe access
```

### Type Definitions Reference

#### `ReportsInterface`
```typescript
interface ReportsInterface {
  _id: { $oid: string };
  rmaNumber: string;
  srNumber: string;
  partId: string;
  serialNumber: string;
  key: string;
  userid: string;
  status: 'open' | 'closed' | 'deleted';
  creation_date: string;
  last_update: string;
  lastComment: string;
  customerName: string;
  attachments: Array<{fileName: string, fileLink: string}> | [];
  comments: Array<{
    _id: { $oid: string };
    userid: string;
    comment: string;
    creation_date: string;
    last_update: string;
    report: { $oid: string };
  }>;
}
```

#### `DefectiveUpdateInterface`
```typescript
interface DefectiveUpdateInterface {
  filter: { _id: string };
  update: Partial<Omit<ReportsInterface, '_id'>>;
}
```

#### `DefectiveUpdateResponseInterface`
```typescript
interface DefectiveUpdateResponseInterface {
  matched_count: number;
  modified_count: number;
  filter: {
    _id: { $oid: string };
  };
}
```

#### `commentsCreateInterface`
```typescript
interface commentsCreateInterface {
  comments: string;
  report_id: string;
}
```

---

## Response Flow

### BDB Response Structure

The backend returns a complex nested structure:

```typescript
interface BDBResponseInterface {
  code: number;                    // HTTP status code
  data: {
    printables: Array<{
      type: string;
      key: string;
    }>;
    variables: {
      result: string;              // ⚠️ JSON string (needs parsing)
      stdout: string;
      _out_automation: any;
    };
    result: {
      key: string;
      type: string;
    };
  };
  worker_duration: number;         // Processing time
}
```

### Parsing with `GetParsedResult`

The `GetParsedResult` function extracts and parses the actual data:

```typescript
export function GetParsedResult<T>(response: BDBResponseInterface): T {
  // 1. Extract the 'result' string from nested variables
  const resultString = response.data.variables.result;
  
  // 2. Parse the JSON string
  const parsed: { consult: T } = JSON.parse(resultString);
  
  // 3. Extract and return the 'consult' property
  return parsed.consult as T;
}
```

### Complete Response Flow Diagram

```
Backend Response
    │
    ├─ code: 200
    ├─ worker_duration: 123
    └─ data
        └─ variables
            └─ result: '{"consult": [...actual data...]}'
                            │
                            ├─ JSON.parse()
                            ▼
                    { consult: T }
                            │
                            ├─ Extract .consult
                            ▼
                    Typed Result (T)
                            │
                            ▼
                Your Application Code
```

### Example Response Transformation

**Raw Backend Response:**
```json
{
  "code": 200,
  "data": {
    "variables": {
      "result": "{\"consult\": [{\"_id\": {\"$oid\": \"123\"}, \"rmaNumber\": \"RMA001\", ...}]}"
    }
  },
  "worker_duration": 145
}
```

**After `GetParsedResult<ReportsInterface[]>`:**
```typescript
[
  {
    _id: { $oid: '123' },
    rmaNumber: 'RMA001',
    // ... full ReportsInterface object
  }
]
```

---

## Practical Examples

### Example 1: Fetching All Reports

```typescript
import { fetchDeffectiveRMAReport } from './services/api.service';

async function loadReports() {
  try {
    const reports = await fetchDeffectiveRMAReport();
    
    // TypeScript knows 'reports' is ReportsInterface[]
    console.log(`Loaded ${reports.length} reports`);
    
    reports.forEach(report => {
      console.log(`${report.rmaNumber} - ${report.status}`);
      // ✅ Full autocomplete and type checking
    });
    
  } catch (error) {
    console.error('Failed to fetch reports:', error);
  }
}
```

### Example 2: Updating Multiple Reports

```typescript
import { updateDefectiveRMAReport } from './services/api.service';
import type { DefectiveUpdateInterface } from './services/defective.update.interface';

async function closeReports(reportIds: string[]) {
  const updates: DefectiveUpdateInterface[] = reportIds.map(id => ({
    filter: { _id: id },
    update: { 
      status: 'closed',
      last_update: new Date().toISOString()
    }
  }));
  
  try {
    const results = await updateDefectiveRMAReport(updates);
    
    // TypeScript knows 'results' is DefectiveUpdateResponseInterface[]
    results.forEach((result, index) => {
      console.log(`Report ${index + 1}: ${result.modified_count} modified`);
    });
    
    return results;
  } catch (error) {
    console.error('Failed to update reports:', error);
    throw error;
  }
}
```

### Example 3: Creating a Comment

```typescript
import { createDefectiveRMAComment } from './services/api.service';
import type { commentsCreateInterface } from './services/comments.response.interface';

async function addCommentToReport(reportId: string, commentText: string) {
  const commentData: commentsCreateInterface = {
    comments: commentText,
    report_id: reportId
  };
  
  try {
    const newComment = await createDefectiveRMAComment(commentData);
    
    // TypeScript knows 'newComment' is commentsInterface
    console.log(`Comment created with ID: ${newComment._id.$oid}`);
    console.log(`Created at: ${newComment.creation_date}`);
    
    return newComment;
  } catch (error) {
    console.error('Failed to create comment:', error);
    throw error;
  }
}
```

### Example 4: React Component Integration

```typescript
import { useState, useEffect } from 'react';
import { fetchDeffectiveRMAReport } from '../../services/api.service';
import type { ReportsInterface } from './reports.interface';

function ReportsTable() {
  const [reports, setReports] = useState<ReportsInterface[]>([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState<string | null>(null);

  useEffect(() => {
    async function loadData() {
      try {
        setLoading(true);
        const data = await fetchDeffectiveRMAReport();
        setReports(data);
        // ✅ TypeScript ensures 'data' matches ReportsInterface[]
      } catch (err) {
        setError('Failed to load reports');
        console.error(err);
      } finally {
        setLoading(false);
      }
    }

    loadData();
  }, []);

  if (loading) return <div>Loading...</div>;
  if (error) return <div>Error: {error}</div>;

  return (
    <table>
      <thead>
        <tr>
          <th>RMA Number</th>
          <th>Status</th>
          <th>Customer</th>
        </tr>
      </thead>
      <tbody>
        {reports.map(report => (
          <tr key={report._id.$oid}>
            <td>{report.rmaNumber}</td>
            <td>{report.status}</td>
            <td>{report.customerName}</td>
          </tr>
        ))}
      </tbody>
    </table>
  );
}
```

---

## Best Practices

### 1. Always Define Response Types

```typescript
// ❌ BAD - No type safety
const data = await bdbGenericRequest('some_function');

// ✅ GOOD - Full type safety
const data = await bdbGenericRequest<MyResponseType>('some_function');
```

### 2. Use Specific API Service Functions

```typescript
// ❌ BAD - Direct use in components
import { bdbGenericRequest } from './services/bdb-generic-request';
const reports = await bdbGenericRequest<ReportsInterface[]>('read_deffective_rma_reports');

// ✅ GOOD - Use wrapper functions
import { fetchDeffectiveRMAReport } from './services/api.service';
const reports = await fetchDeffectiveRMAReport();
```

**Benefits:**
- Centralized function names (easier to refactor)
- Better autocomplete and discoverability
- Cleaner imports

### 3. Handle Errors Appropriately

```typescript
async function safelyFetchReports() {
  try {
    return await fetchDeffectiveRMAReport();
  } catch (error) {
    if (axios.isAxiosError(error)) {
      console.error('API Error:', error.response?.status);
      console.error('Message:', error.message);
    } else {
      console.error('Unexpected error:', error);
    }
    throw error; // Re-throw for higher-level handling
  }
}
```

### 4. Use Type Guards for Runtime Safety

```typescript
function isValidReport(data: any): data is ReportsInterface {
  return (
    data &&
    typeof data._id?.$oid === 'string' &&
    typeof data.rmaNumber === 'string' &&
    ['open', 'closed', 'deleted'].includes(data.status)
  );
}

async function fetchValidatedReports() {
  const reports = await fetchDeffectiveRMAReport();
  
  // Additional runtime validation
  return reports.filter(isValidReport);
}
```

### 5. Create Domain-Specific Services

Instead of putting everything in `api.service.ts`, organize by domain:

```typescript
// services/reports.service.ts
export const fetchAllReports = () => 
  bdbGenericRequest<ReportsInterface[]>('read_deffective_rma_reports');

export const fetchReportById = (id: string) =>
  bdbGenericRequest<ReportsInterface>('read_report_by_id', { id });

// services/comments.service.ts
export const createComment = (data: commentsCreateInterface) =>
  bdbGenericRequest<commentsInterface, commentsCreateInterface>('create_comments', data);

export const updateComment = (id: string, text: string) =>
  bdbGenericRequest<commentsInterface>('update_comment', { id, text });
```

### 6. Environment Configuration

Ensure your `.env` file is properly configured:

```env
VITE_APP_API_BASE_URL=https://api.example.com/execute
VITE_APP_DEV_MODE=true
```

### 7. Type Safety for Input Data

Always define interfaces for input data:

```typescript
// ❌ BAD - Inline objects
await bdbGenericRequest('update_report', { filter: { _id: '123' }, update: { status: 'closed' }});

// ✅ GOOD - Typed interfaces
const updateData: DefectiveUpdateInterface = {
  filter: { _id: '123' },
  update: { status: 'closed' }
};
await updateDefectiveRMAReport([updateData]);
```

---

## Summary

The `bdbGenericRequest` system provides:

1. **Type-Safe API Communication**: Full TypeScript support for requests and responses
2. **Generic Flexibility**: Works with any backend function and data structure
3. **Clean Abstraction**: `api.service.ts` provides domain-specific wrappers
4. **Automatic Parsing**: Handles complex nested response structures
5. **Maintainability**: Centralized configuration and error handling

**Key Takeaway**: By combining TypeScript generics with a consistent API pattern, `bdbGenericRequest` provides a robust, type-safe foundation for all backend communication in the application.
