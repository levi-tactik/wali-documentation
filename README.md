## **Sample Input Output for Large Language Models**
### Sample Input
```python

``` 
### Sample Prompt to LLM
```python

```
### Sample Output
```python

```
******

## **Sample Input Output for Embeddings Models**
### Sample Input
```python
"Can you make me a quote for 10 Starter Licences?"
``` 
### Sample Output
```python
[
  0.0123, -0.0456, 0.0789, 0.1023, -0.0567, ..., 0.0345
]
```
******

## **Sample Step-by-Step Process for Vector Database Training**
### Sample Products Table Data
| id  | name            | category | description                | price | tags                |
| --- | --------------- | -------- | -------------------------- | ----- | ------------------- |
| 1   | Starter License | Software | Basic license for one user | 100.0 | starter, basic      |
| 2   | Pro License     | Software | Advanced license for teams | 250.0 | pro, advanced, team |
| 3   | Enterprise Plan | Software | Full suite for enterprises | 500.0 | enterprise, full    |

### Consolidated Columns Result
Before generating embeddings, relevant text-based columns are combined into a single field:
| id  | consolidated_text                                                                                            |
| --- | ------------------------------------------------------------------------------------------------------------ |
| 1   | "Name: Starter License, Category: Software, Description: Basic license for one user, Tags: starter, basic"   |
| 2   | "Name: Pro License, Category: Software, Description: Advanced license for teams, Tags: pro, advanced, team"  |
| 3   | "Name: Enterprise Plan, Category: Software, Description: Full suite for enterprises, Tags: enterprise, full" |

### Sample Embeddings Table Data
After generating embeddings, the results are stored in a **PostgreSQL** table with a `vector` column (assuming **pgvector** is used):
| id  | product_id | embedding (vector representation) |
| --- | ---------- | --------------------------------- |
| 1   | 1          | `[0.0123, -0.0456, 0.0789, ...]`  |
| 2   | 2          | `[0.0987, -0.0123, 0.0567, ...]`  |
| 3   | 3          | `[0.0234, 0.0781, -0.0452, ...]`  |