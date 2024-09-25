## JSON Version

### Import Modules:
```
const { 
  initPool, 
  create, 
  read, 
  update, 
  remove, 
  readDetailJoin, 
  readDetailById,
  count,
  groupBy,
  batchInsert,
  paginate,
  search,
  distinct,
  dropTable 
} = require('opcrud-mysql/json');
```

### Path Json File:
```
let dbPath;
const initializeDatabase = async () => {
  dbPath = path.join(__dirname, 'database.json');
  await initPool(dbPath);
};
```

### JSON Formated:
```
{
  "table1": [
    {
      "value": "string",
      "id": 1 // fixed rules
    },
  "table2": [
    {
      "value": "string",
      "id": 1 // fixed rules
    }
	// more ...
  ]
}
```

### Created
```
app.post('/create/:table', async (req, res) => {
  const table = req.params.table;
  const data = req.body;

  try {
    const result = await create(table, data, dbPath);
    res.status(201).json(result);
  } catch (error) {
    res.status(400).json({ error: error.message });
  }
});
```

### Reading
```
app.get('/read/:table', async (req, res) => {
  const table = req.params.table;
  const conditions = req.query;

  try {
    const data = await read(table, conditions, dbPath);
    res.json(data);
  } catch (error) {
    res.status(400).json({ error: error.message });
  }
});
```

### Count
```
app.get('/count/:table', async (req, res) => {
  const table = req.params.table;
  const conditions = req.query;

  try {
    const result = await count(table, conditions, dbPath);
    res.json({ count: result });
  } catch (error) {
    res.status(400).json({ error: error.message });
  }
});
```

### Group By
```
app.get('/groupby/:table', async (req, res) => {
  const table = req.params.table;
  const { groupColumn, aggregateFunction } = req.query;

  try {
    const result = await groupBy(table, groupColumn, aggregateFunction, dbPath);
    res.json(result);
  } catch (error) {
    res.status(400).json({ error: error.message });
  }
});
```

### Batch Insert
```
app.post('/batch-insert/:table', async (req, res) => {
  const table = req.params.table;
  const dataArray = req.body;

  try {
    await batchInsert(table, dataArray, dbPath);
    res.status(201).send();
  } catch (error) {
    res.status(400).json({ error: error.message });
  }
});
```

### Pagination
```
app.get('/paginate/:table', async (req, res) => {
  const table = req.params.table;
  const { page = 1, pageSize = 10 } = req.query;

  try {
    const data = await paginate(table, page, pageSize, {}, dbPath);
    res.json(data);
  } catch (error) {
    res.status(400).json({ error: error.message });
  }
});
```

### Search
```
app.get('/search/:table', async (req, res) => {
  const table = req.params.table;
  const { column, searchTerm } = req.query;

  try {
    const data = await search(table, column, searchTerm, dbPath);
    res.json(data);
  } catch (error) {
    res.status(400).json({ error: error.message });
  }
});
```

### Distinct
```
app.get('/distinct/:table/:column', async (req, res) => {
  const table = req.params.table;
  const column = req.params.column;

  try {
    const result = await distinct(table, column, dbPath);
    res.json(result);
  } catch (error) {
    res.status(400).json({ error: error.message });
  }
});
```

### Get Detail Join By ID
```
app.get('/detail-join/:table1/:table2', async (req, res) => {
  const table1 = req.params.table1;
  const table2 = req.params.table2;
  const { joinCondition, id } = req.query;

  try {
    const data = await readDetailJoin(table1, table2, joinCondition, id, dbPath);
    res.json(data);
  } catch (error) {
    res.status(400).json({ error: error.message });
  }
});
```

### Get Detail by ID
```
app.get('/detail/:table/:id', async (req, res) => {
  const table = req.params.table;
  const id = parseInt(req.params.id);

  try {
    const data = await readDetailById(table, id, dbPath);
    res.json(data);
  } catch (error) {
    console.error(error);
    res.status(400).json({ error: error.message });
  }
});
```

### Update
```
app.put('/update/users/:id', async (req, res) => {
    const userId = parseInt(req.params.id);
    const data = req.body;

    try {
        const updatedUsers = await update('users', data, { id: userId }, dbPath);
        if (updatedUsers.length > 0) {
            res.json(updatedUsers);
        } else {
            res.status(404).json({ error: 'User not found' });
        }
    } catch (error) {
        res.status(400).json({ error: error.message });
    }
});
```

### Remove
```
app.delete('/remove/:table/:id', async (req, res) => {
  const table = req.params.table;
  const id = req.params.id;

  console.log(`Removing from table: ${table}, ID: ${id}`);

  try {
    const result = await remove(table, { id: Number(id) }, dbPath);
    res.status(204).send();
  } catch (error) {
    res.status(400).json({ error: error.message });
  }
});
```

### Drop Table
```
app.delete('/drop/:table', async (req, res) => {
  const table = req.params.table;

  try {
    await dropTable(table, dbPath);
    res.status(204).send();
  } catch (error) {
    res.status(400).json({ error: error.message });
  }
});
```

## Implementation Example
```
const express = require('express');
const path = require('path');
const app = express();
const PORT = process.env.PORT || 3000;

// Import Modules
const { 
  initPool, 
  create, 
  read, 
  update, 
  remove, 
  readDetailJoin, 
  readDetailById,
  count,
  groupBy,
  batchInsert,
  paginate,
  search,
  distinct,
  dropTable 
} = require('opcrud-mysql/json');

app.use(express.json());

// Path Json File
let dbPath;
const initializeDatabase = async () => {
  dbPath = path.join(__dirname, 'database.json');
  await initPool(dbPath);
};

// Create
app.post('/create/:table', async (req, res) => {
  const table = req.params.table;
  const data = req.body;

  try {
    const result = await create(table, data, dbPath);
    res.status(201).json(result);
  } catch (error) {
    res.status(400).json({ error: error.message });
  }
});

// Read
app.get('/read/:table', async (req, res) => {
  const table = req.params.table;
  const conditions = req.query;

  try {
    const data = await read(table, conditions, dbPath);
    res.json(data);
  } catch (error) {
    res.status(400).json({ error: error.message });
  }
});

// Update
app.put('/update/users/:id', async (req, res) => {
    const userId = parseInt(req.params.id);
    const data = req.body;

    try {
        const updatedUsers = await update('users', data, { id: userId }, dbPath);
        if (updatedUsers.length > 0) {
            res.json(updatedUsers);
        } else {
            res.status(404).json({ error: 'User not found' });
        }
    } catch (error) {
        res.status(400).json({ error: error.message });
    }
});

// Remove
app.delete('/remove/:table/:id', async (req, res) => {
  const table = req.params.table;
  const id = req.params.id;

  console.log(`Removing from table: ${table}, ID: ${id}`);

  try {
    const result = await remove(table, { id: Number(id) }, dbPath);
    res.status(204).send();
  } catch (error) {
    res.status(400).json({ error: error.message });
  }
});

// Count
app.get('/count/:table', async (req, res) => {
  const table = req.params.table;
  const conditions = req.query;

  try {
    const result = await count(table, conditions, dbPath);
    res.json({ count: result });
  } catch (error) {
    res.status(400).json({ error: error.message });
  }
});

// Group By
app.get('/groupby/:table', async (req, res) => {
  const table = req.params.table;
  const { groupColumn, aggregateFunction } = req.query;

  try {
    const result = await groupBy(table, groupColumn, aggregateFunction, dbPath);
    res.json(result);
  } catch (error) {
    res.status(400).json({ error: error.message });
  }
});

// Batch Insert
app.post('/batch-insert/:table', async (req, res) => {
  const table = req.params.table;
  const dataArray = req.body;

  try {
    await batchInsert(table, dataArray, dbPath);
    res.status(201).send();
  } catch (error) {
    res.status(400).json({ error: error.message });
  }
});

// Pagination
app.get('/paginate/:table', async (req, res) => {
  const table = req.params.table;
  const { page = 1, pageSize = 10 } = req.query;

  try {
    const data = await paginate(table, page, pageSize, {}, dbPath);
    res.json(data);
  } catch (error) {
    res.status(400).json({ error: error.message });
  }
});

// Search
app.get('/search/:table', async (req, res) => {
  const table = req.params.table;
  const { column, searchTerm } = req.query;

  try {
    const data = await search(table, column, searchTerm, dbPath);
    res.json(data);
  } catch (error) {
    res.status(400).json({ error: error.message });
  }
});

// Distinct
app.get('/distinct/:table/:column', async (req, res) => {
  const table = req.params.table;
  const column = req.params.column;

  try {
    const result = await distinct(table, column, dbPath);
    res.json(result);
  } catch (error) {
    res.status(400).json({ error: error.message });
  }
});

// Read Detail Join By ID
app.get('/detail-join/:table1/:table2', async (req, res) => {
  const table1 = req.params.table1;
  const table2 = req.params.table2;
  const { joinCondition, id } = req.query;

  try {
    const data = await readDetailJoin(table1, table2, joinCondition, id, dbPath);
    res.json(data);
  } catch (error) {
    res.status(400).json({ error: error.message });
  }
});

// Get Detail by ID
app.get('/detail/:table/:id', async (req, res) => {
  const table = req.params.table;
  const id = parseInt(req.params.id);

  try {
    const data = await readDetailById(table, id, dbPath);
    res.json(data);
  } catch (error) {
    console.error(error);
    res.status(400).json({ error: error.message });
  }
});

// Drop Table
app.delete('/drop/:table', async (req, res) => {
  const table = req.params.table;

  try {
    await dropTable(table, dbPath);
    res.status(204).send();
  } catch (error) {
    res.status(400).json({ error: error.message });
  }
});

app.listen(PORT, async () => {
  await initializeDatabase();
  console.log(`Server is running on http://localhost:${PORT}`);
});
```
