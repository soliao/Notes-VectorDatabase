# Notes-VectorDatabase


### Define table schema

```python
from lancedb.pydantic import vector, LanceModel

class TABLE_SCHEMA(LanceModel):
    field_1: vector(2)
    field_2: str
    field_3: str
    field_4: float
```

### Create table

```python
import lancedb

db = lancedb.connect(DB_URI)
db.drop_table(TABLE_NAME, ignore_missing=True)

# empty table
tbl = db.create_table(TABLE_NAME, schema=TABLE_SCHEMA)

# convert nparray to table
tbl = db.create_table(TABLE_NAME, vec_to_table(NP_ARRAY))
```

### Add data

```python
data = [TABLE_SCHEMA(...), TABLE_SCHEMA(...), TABLE_SCHEMA(...)]

tbl.add([dict(x) for x in data])

tbl.head().to_pandas()
```

### Query data

```python
# Euclidean distance
tbl.search(VECTOR).limit(NUM_LIMIT).to_df()

# Cosine distance
tbl.search(VECTOR).metric("cosine").limit(NUM_LIMIT).to_df()

# Filtering
tbl.search(VECTOR).limit(NUM_LIMIT).where("SQL_FILTER").to_df()
```

### Create ANN indices

```python
tbl.create_index(num_partitions=NUM_PARTITIONS, num_sub_vectors=NUM_SUB_VECTORS)

tbl.search(VECTOR).limit(10).to_df()
```

### Version control

```python
tbl.list_versions()
```


