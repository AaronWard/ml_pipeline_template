# MLFlow Machine Learning Pipeline Template



```powershell
conda env create -f environment.yml
conda activate base_project_env
```



**Creating a New Component**

To create a component, run the following command:

```cookiecutter cookie-mlflow-step -o src```

You will then be prompted to provide some input about the component
example:

```powershell
step_name [step_name]: basic_cleaning
script_name [run.py]: run.py
job_type [my_step]: basic_cleaning
short_description [My step]: This steps cleans the data
long_description [An example of a step using MLflow and Weights & Biases]: Performs basic cleaning on the data and save the results in Weights & Biases
parameters [parameter1,parameter2]: parameter1,parameter2,parameter3
```

- This will autogenerate a folder within `src` with the step name you provided. Within this folder you will see a `run.py`. This is some boilerplate code to help you build out your component
- You will notice that it already has the ability to accept the parameters you specified earlier, and calls the `go()` function.
- Edit this function to perform what the component aims to achieve. You may import miscallaneous utilities from the `utils` folder.


**Dependencies**:

- see `conda.yml`


**Running the pipeline**

- `cd` to the root diractor
- ```mlflow run .```



**Removing conda environments created by MLFlow**:
```
> conda info --envs | grep mlflow | cut -f1 -d" "
> for e in $(conda info --envs | grep mlflow | cut -f1 -d" "); do conda uninstall --name $e --all -y;done
```


<hr>


**Issues with VSCode:**

- If you are using VSCode sometimes the interpreter looks at the wrong instance of python
- make sure you deactivate the default (base) environment before activating the `base_project_env`, or whatever you called your conda env within `environment.yml`