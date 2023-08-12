## Project Init
- `dbt` will try to find the `profile.yaml` file first in the working directory, if not existing, it will try to find the file in the `~/.dbt` folder
- In ther `profile.yaml`, parsing the credential file:
    - If you want to parse the whole credential json file, the field `method` should be `service-account-json`
    - Or only the path to the credential file, the field `method` should be `service-account`

## Create models
- Config the model (in dir `./jaffle_shop/models/*.sql`) to be a `view` or an actual `table` in 2 ways (in 2 levels):
    - At the **Project** level (can apply to all models): in the `dbt_project.yml` file:
        ```yaml
        models:
            jaffle_shop:
                +materialized: table # apply to all models in project. global config
                # Config indicated by + and applies to all files under models/example/
                example:
                +materialized: view # overide global config
        ```
    - At the **model** level (will **overide** the config in `dbt_project.yml` file), at the top of each `.sql` file (*definning a model*), add these line of code:
        ```sql
        {{
            config(
                meterialized='view'
            )
        }}
        ```
- After changing any **config** to the **targets**, use `dbt run --full-refresh` to takes affect in the data warehouse.

## references (`ref`)
- use `ref` in script to reference other models (in others files):
```sql
with customers as (
    select * from {{ ref('stg_customers')}}
)
```
- To run models in a folder, execute `dbt run --models`
    - E.g.: To run every model inside **folder2**, inside **folder1**: `dbt run --models folder1.folder2.*`

## UI
- For **first time running the docs**: `dbt docs generate`, new config will be stored in the folder `target/manifest.json` and `target/catalog.json`,
- For serving the docs into a server: `dbt docs serve`