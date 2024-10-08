## MYSQL Version

### Import Modules:

```
const { 
  initPool, 
  create, 
  read, 
  join, 
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
} = require('opcrud-mysql');
```

### Connection:

```
initPool({
  host: 'localhost',
  user: 'your_username',
  password: 'your_password',
  database: 'your_database',
});
```

### Created

```
app.post('/records', async (req, res) => {
  try {
    const data = req.body;
    const result = await create('your_table_name', data);
    res.status(201).json(result);
  } catch (err) {
    res.status(500).json({ error: err.message });
  }
});
```

### Reading

```
app.get('/records', async (req, res) => {
  try {
    const records = await read('your_table_name');
    res.status(200).json(records);
  } catch (err) {
    res.status(500).json({ error: err.message });
  }
});
```

### Count

```
app.get('/records/count', async (req, res) => {
  try {
    const total = await count('your_table_name');
    res.status(200).json({ total });
  } catch (err) {
    res.status(500).json({ error: err.message });
  }
});
```

### Group By

```
app.get('/records/group', async (req, res) => {
  const { groupColumn, aggregateFunction } = req.query;

  try {
    const groupedRecords = await groupBy('your_table_name', groupColumn, aggregateFunction);
    res.status(200).json(groupedRecords);
  } catch (err) {
    res.status(500).json({ error: err.message });
  }
});
```

### Batch Insert

```
app.post('/records/batch', async (req, res) => {
  try {
    const dataArray = req.body;
    const result = await batchInsert('your_table_name', dataArray);
    res.status(201).json(result);
  } catch (err) {
    res.status(500).json({ error: err.message });
  }
});
```

### Pagination

```
app.get('/records/paginate', async (req, res) => {
  const { page = 1, pageSize = 10 } = req.query;
  const conditions = req.query.conditions ? JSON.parse(req.query.conditions) : {};

  try {
    const paginatedRecords = await paginate('your_table_name', page, pageSize, conditions);
    res.status(200).json(paginatedRecords);
  } catch (err) {
    res.status(500).json({ error: err.message });
  }
});
```

### Search

```
app.get('/records/search', async (req, res) => {
  const { column, searchTerm } = req.query;

  try {
    const searchResults = await search('your_table_name', column, searchTerm);
    res.status(200).json(searchResults);
  } catch (err) {
    res.status(500).json({ error: err.message });
  }
});
```

### Distinct

```
app.get('/records/distinct', async (req, res) => {
  const { column } = req.query;

  try {
    const distinctValues = await distinct('your_table_name', column);
    res.status(200).json(distinctValues);
  } catch (err) {
    res.status(500).json({ error: err.message });
  }
});
```

### Get Detail Join By ID

```
app.get('/records/detail/:id', async (req, res) => {
  const { id } = req.params;
  const table1 = 'your_first_table';
  const table2 = 'your_second_table';
  const joinCondition = 'your_first_table.foreignKey = your_second_table.id';

  try {
    const recordDetail = await readDetailJoin(table1, table2, joinCondition, id);
    if (recordDetail) {
      res.status(200).json(recordDetail);
    } else {
      res.status(404).json({ message: 'Record not found' });
    }
  } catch (err) {
    res.status(500).json({ error: err.message });
  }
});
```

### Get Detail by ID

```
app.get('/records/:id', async (req, res) => {
  const { id } = req.params;

  try {
    const record = await readDetailById('your_table_name', id);
    res.status(200).json(record);
  } catch (err) {
    res.status(500).json({ error: err.message });
  }
});
```

### Join Table

```
app.get('/join/:id', async (req, res) => {
  const { id } = req.params;

  try {
    const table1 = 'table1';
    const table2 = 'table2';
    const joinCondition = 'table1.foreignKey = table2.id';
    const conditions = { id: id };

    const joinedRecords = await join(table1, table2, joinCondition, conditions);
    res.status(200).json(joinedRecords);
  } catch (err) {
    res.status(500).json({ error: err.message });
  }
});
```

### Update

```
app.put('/records/:id', async (req, res) => {
  const { id } = req.params;
  const data = req.body;

  try {
    const result = await update('your_table_name', data, { id });
    res.status(200).json(result);
  } catch (err) {
    res.status(500).json({ error: err.message });
  }
});
```

### Remove

```
app.delete('/records/:id', async (req, res) => {
  const { id } = req.params;

  try {
    const result = await remove('your_table_name', { id });
    res.status(200).json(result);
  } catch (err) {
    res.status(500).json({ error: err.message });
  }
});
```

### Drop Table

```
app.delete('/tables/:tableName', async (req, res) => {
  const { tableName } = req.params;

  try {
    await dropTable(tableName);
    res.status(200).json({ message: `Table ${tableName} dropped successfully.` });
  } catch (err) {
    res.status(500).json({ error: err.message });
  }
});
```

## Implementation Example

```
const express = require('express');
const bodyParser = require('body-parser');

const { 
  initPool, 
  create, 
  read, 
  join, 
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
} = require('opcrud-mysql');

const app = express();
const PORT = process.env.PORT || 3000;
app.use(bodyParser.json());

// Connection
initPool({
  host: 'localhost',
  user: 'your_username',
  password: 'your_password',
  database: 'your_database',
});

// Created
app.post('/records', async (req, res) => {
  try {
    const data = req.body;
    const result = await create('your_table_name', data);
    res.status(201).json(result);
  } catch (err) {
    res.status(500).json({ error: err.message });
  }
});

// Reading
app.get('/records', async (req, res) => {
  try {
    const records = await read('your_table_name');
    res.status(200).json(records);
  } catch (err) {
    res.status(500).json({ error: err.message });
  }
});

// Count
app.get('/records/count', async (req, res) => {
  try {
    const total = await count('your_table_name');
    res.status(200).json({ total });
  } catch (err) {
    res.status(500).json({ error: err.message });
  }
});

// Group By
app.get('/records/group', async (req, res) => {
  const { groupColumn, aggregateFunction } = req.query;

  try {
    const groupedRecords = await groupBy('your_table_name', groupColumn, aggregateFunction);
    res.status(200).json(groupedRecords);
  } catch (err) {
    res.status(500).json({ error: err.message });
  }
});

// Batch Insert
app.post('/records/batch', async (req, res) => {
  try {
    const dataArray = req.body;
    const result = await batchInsert('your_table_name', dataArray);
    res.status(201).json(result);
  } catch (err) {
    res.status(500).json({ error: err.message });
  }
});

// Pagination
app.get('/records/paginate', async (req, res) => {
  const { page = 1, pageSize = 10 } = req.query;
  const conditions = req.query.conditions ? JSON.parse(req.query.conditions) : {};

  try {
    const paginatedRecords = await paginate('your_table_name', page, pageSize, conditions);
    res.status(200).json(paginatedRecords);
  } catch (err) {
    res.status(500).json({ error: err.message });
  }
});

// Search
app.get('/records/search', async (req, res) => {
  const { column, searchTerm } = req.query;

  try {
    const searchResults = await search('your_table_name', column, searchTerm);
    res.status(200).json(searchResults);
  } catch (err) {
    res.status(500).json({ error: err.message });
  }
});

// Distinct
app.get('/records/distinct', async (req, res) => {
  const { column } = req.query;

  try {
    const distinctValues = await distinct('your_table_name', column);
    res.status(200).json(distinctValues);
  } catch (err) {
    res.status(500).json({ error: err.message });
  }
});

// Get Detail Join By ID
app.get('/records/detail/:id', async (req, res) => {
  const { id } = req.params;
  const table1 = 'your_first_table';
  const table2 = 'your_second_table';
  const joinCondition = 'your_first_table.foreignKey = your_second_table.id';

  try {
    const recordDetail = await readDetailJoin(table1, table2, joinCondition, id);
    if (recordDetail) {
      res.status(200).json(recordDetail);
    } else {
      res.status(404).json({ message: 'Record not found' });
    }
  } catch (err) {
    res.status(500).json({ error: err.message });
  }
});

// Get Detail by ID
app.get('/records/:id', async (req, res) => {
  const { id } = req.params;

  try {
    const record = await readDetailById('your_table_name', id);
    res.status(200).json(record);
  } catch (err) {
    res.status(500).json({ error: err.message });
  }
});

// Join Table
app.get('/join/:id', async (req, res) => {
  const { id } = req.params;

  try {
    const table1 = 'table1';
    const table2 = 'table2';
    const joinCondition = 'table1.foreignKey = table2.id';
    const conditions = { 'table1.id': id };

    const joinedRecords = await join(table1, table2, joinCondition, conditions);
    res.status(200).json(joinedRecords);
  } catch (err) {
    res.status(500).json({ error: err.message });
  }
});

// Update
app.put('/records/:id', async (req, res) => {
  const { id } = req.params;
  const data = req.body;

  try {
    const result = await update('your_table_name', data, { id });
    res.status(200).json(result);
  } catch (err) {
    res.status(500).json({ error: err.message });
  }
});

// Remove
app.delete('/records/:id', async (req, res) => {
  const { id } = req.params;

  try {
    const result = await remove('your_table_name', { id });
    res.status(200).json(result);
  } catch (err) {
    res.status(500).json({ error: err.message });
  }
});

// Drop Table
app.delete('/tables/:tableName', async (req, res) => {
  const { tableName } = req.params;

  try {
    await dropTable(tableName);
    res.status(200).json({ message: `Table ${tableName} dropped successfully.` });
  } catch (err) {
    res.status(500).json({ error: err.message });
  }
});


app.listen(PORT, () => {
  console.log(`Server is running on port ${PORT}`);
});
```