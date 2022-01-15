# MLFlow Machine Learning Pipeline Template






<hr>

## Creating base conda environment
- Firstly we need to create our base conda environment for development.
- Add any dependencies you know you will need in the `environment.yml`

```powershell
conda env create -f environment.yml
conda activate base_project_env
```


## Creating a New Component

- To create a component, run the following command:

```
cookiecutter cookie-mlflow-step -o src
```

- You will then be prompted to provide some input about the component.
For example:

```powershell
step_name [step_name]: basic_cleaning
script_name [run.py]: run.py
job_type [my_step]: basic_cleaning
short_description [My step]: This steps cleans the data
long_description [An example of a step using MLflow and Weights & Biases]: Performs basic cleaning on the data and save the results in Weights & Biases
parameters [parameter1,parameter2]: parameter1,parameter2,parameter3
```

- This will autogenerate a folder within `src` with the step name you provided.
- Within this folder you will see a `run.py`. This is some boilerplate code to help you build out your component
- You will notice that it already has the ability to accept the parameters you specified earlier, and calls the `go()` function.
- Edit this function to perform what the component aims to achieve. You may import miscallaneous utilities from the `utils` folder.


## Dependencies:

- see `conda.yml` for individual component dependencies.
- This differs from the `environment.yml`, as the `conda.yml` us whats used by MLFlow at runtime.



## Usage:

### Run entire pipeline

- `cd` to the root diractor
```
mlflow run .
```

## Running the entire pipeline or just a selection of steps:
- In order to run the pipeline when you are developing, you need to be in the root of the starter kit, then you can execute as usual:

```powershell
mlflow run .
```

- This will run the entire pipeline.

- When developing it is useful to be able to run one step at the time. Say you want to run only the download step. The main.py is written so that the steps are defined at the top of the file, in the _steps list, and can be selected by using the steps parameter on the command line:

```powershell
mlflow run . -P steps=download
```

- If you want to run the download and the basic_cleaning steps, you can similarly do:

```powershell
mlflow run . -P steps=download,basic_cleaning
```

- You can override any other parameter in the `config.yaml` using the Hydra syntax, by providing it as a `hydra_options` parameter.

```powershell
mlflow run . \
  -P steps=train_model \
  -P hydra_options="modeling.random_forest.n_estimators=10"
```

## Adding a new component to main.py

- In the `config.yaml` file, under `main.execute_steps` add your new component to the list (*make sure your pipeline is in sequential order*)
- Go to `main.py` and add this step to the code
- NOTE: you will need to use different parameters:

```python
if "data_extraction" in steps_to_execute:

    _ = mlflow.run(
        os.path.join(root_path, "src", "data_extraction"),
        "main",
        parameters={
            "file_url": config["data"]["file_url"],
            "artifact_name": "raw_data.parquet",
            "artifact_type": "raw_data",
            "artifact_description": "Data as downloaded"
        },
    )
```


## Removing conda environments created by MLFlow:
```
> conda info --envs | grep mlflow | cut -f1 -d" "

> for e in $(conda info --envs | grep mlflow | cut -f1 -d" "); do conda uninstall --name $e --all -y;done
```

<hr>


## Known Issues:

**VSCode terminal:**

- If you are using VSCode sometimes the interpreter looks at the wrong instance of python
- make sure you deactivate the default (base) environment before activating the `base_project_env`, or whatever you called your conda env within `environment.yml`

**Python version**
- Sometimes, especially when using VSCode, you make be accidentally using the wrong version of python.
- If a component fails to run due to a `ModuleNotFoundError` or `AttributeError` try adding the python version to the `conda.yaml`

```yaml
dependencies:
  - python=3.8
```
