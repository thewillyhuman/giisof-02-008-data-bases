# Session 01 - Introduction to SQL I

### 1. Get all the data about the cars stored in the entity (table) CARS.
```sql
SELECT *
FROM cars;
```

### 2. Get all the data about the cars in the CARS table with 'gtd' model.
```sql
SELECT *
FROM cars
WHERE model='gtd';
```

### 3. Insert a new car into the CARS entity.
```sql
INSERT INTO cars (codecar,namec,model) VALUES ('21','Clio','1.5');
```

### 4. Delete an existing car from the CARS entity.
```sql
DELETE FROM cars WHERE codecar='21';
```

### 5. Update some data about a given car in the CARS entity.
```sql
UPDATE cars SET model='gtd' WHERE codecar='16';
```