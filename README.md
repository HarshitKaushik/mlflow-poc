# mlflow-poc

POCs for MLflow.

- MLflow is an open source platform for managing the end-to-end machine learning lifecycle. It tackles four primary functions:
1. Tracking experiments to record and compare parameters and results (**MLflow Tracking**).
2. Packaging ML code in a reusable, reproducible form in order to share with other data scientists or transfer to production (**MLflow Projects**).
3. Managing and deploying models from a variety of ML libraries to a variety of model serving and inference platforms (**MLflow Models**).
4. Providing a central model store to collaboratively manage the full lifecycle of an MLflow Model, including model versioning, stage transitions, and annotations (**MLflow Model Registry**).
- MLflow is library-agnostic. You can use it with any machine learning library, and in any programming language, since all functions are accessible through a REST API and CLI. For convenience, the project also includes a Python API, R API, and Java API.
- **Using the Tracking API** - The MLflow Tracking API lets you log metrics and artifacts/files from your data science code and see a history of your runs.
- **Viewing the Tracking UI** - By default, wherever you run your program, the tracking API writes data into files into a local ./mlruns directory. You can then run mlflow's tracking UI by the command  ```mlflow ui``` and view it at http://localhost:5000.
- **Running MLflow Projects** - MLflow allows you to package code and its dependencies as a project that can be run in a reproducible fashion on other data. Each project includes its code and a MLproject file that defines its dependencies (for example, Python environment) as well as what commands can be run into the project and what arguments they take.
- What is Conda? Conda is an open-source package management system and environment management system that runs on Windows, macOS, and Linux. Conda quickly installs, runs, and updates packages and their dependencies. Conda easily creates, saves, loads, and switches between environments on your local computer.
- Conda as a package manager helps you find and install packages. If you need a package that requires a different version of Python, you do not need to switch to a different environment manager because conda is also an environment manager.
- MLflow currently supports the following project environments: Conda environment, Docker container environment, and system environment.
- You can also run MLflow Projects directly in your current system environment. All of the project’s dependencies must be installed on your system prior to project execution.
- By default, MLflow Projects are run in the environment specified by the project directory or the MLproject file (see Specifying Project Environments). You can ignore a project’s specified environment and run the project in the current system environment by supplying the ```--no-conda``` flag.
- If you haven’t configured a tracking server, projects log their Tracking API data in the local mlruns directory so you can see these runs using ```mlflow ui```.
- By default mlflow run installs all dependencies using conda. To run a project without using conda, you can provide the ```--no-conda``` option to mlflow run. In this case, you must ensure that the necessary dependencies are already installed in your Python environment.
- **Saving and Serving Models** - MLflow includes a generic MLmodel format for saving models from a variety of tools in diverse flavors. For example, many models can be served as Python functions, so an MLmodel file can declare how each model should be interpreted as a Python function in order to let various tools serve it. 
- MLflow also includes tools for running such models locally and exporting them to Docker containers or commercial serving platforms.
- When you run the example, it outputs an MLflow run ID for that experiment. If you look at mlflow ui, you will also see that the run saved a model folder containing an MLmodel description file and a pickled model. You can pass the run ID and the path of the model within the artifacts directory (here “model”) to various tools.  
```mlflow models serve -m runs:/<RUN_ID>/model```  
- By default the server runs on port 5000. If that port is already in use, use the –port option to specify a different port. For example, see the below command.  
```mlflow models serve -m runs:/<RUN_ID>/model --port ${port}```
- **Logging to a Remote Tracking Server** - In the examples above, MLflow logs data to the local filesystem of the machine it’s running on. To manage results centrally or share them across a team, you can configure MLflow to log to a remote tracking server.
