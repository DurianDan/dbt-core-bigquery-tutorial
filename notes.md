## Project Init
- `dbt` will try to find the `profile.yaml` file first in the working directory, if not existing, it will try to find the file in the `~/.dbt` folder
- In ther `profile.yaml`, parsing the credential file:
    - If you want to parse the whole credential json file, the field `method` should be `service-account-json`
    - Or only the path to the credential file, the field `method` should be `service-account`