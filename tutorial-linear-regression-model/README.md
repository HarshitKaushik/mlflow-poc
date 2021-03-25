This tutorial uses a dataset to predict the quality of wine based on quantitative features like the wine’s “fixed acidity”, “pH”, “residual sugar”, and so on.

This tutorial showcases how you can use MLflow end-to-end to:  
- Train a linear regression model.
- Package the code that trains the model in a reusable and reproducible model format.
- Deploy the model into a simple HTTP server that will enable you to score predictions.

## Training the Model

First, train a linear regression model that takes two hyperparameters: ```alpha``` and ```l1_ratio```.

This example uses the familiar pandas, numpy, and sklearn APIs to create a simple machine learning model. The MLflow tracking APIs log information about each training run, like the hyperparameters ```alpha``` and ```l1_ratio```, used to train the model and metrics, like the root mean square error, used to evaluate the model. The example also serializes the model in a format that MLflow knows how to deploy.

You can run the example with default hyperparameters as follows:

```bash
python sklearn_elasticnet_wine/train.py
```

Try out some other values for ```alpha``` and ```l1_ratio``` by passing them as arguments to train.py.

```bash
python sklearn_elasticnet_wine/train.py <alpha> <l1_ratio>
```

Each time you run the example, MLflow logs information about your experiment runs in the directory ```mlruns```.

## Comparing the Models

Next, use the MLflow UI to compare the models that you have produced.

```bash
mlflow ui
```

## Packaging Training Code in a Conda Environment

Now that you have your training code, you can package it so that other data scientists can easily reuse the model.

You do this by using MLflow Projects conventions to specify the dependencies and entry points to your code. The ```sklearn_elasticnet_wine/MLproject``` file specifies that the project has the dependencies located in a Conda environment file called ```conda.yaml``` and has one entry point that takes two parameters: ```alpha``` and ```l1_ratio```.

## Serving the Model

Now that you have packaged your model using the MLproject convention and have identified the best model, it is time to deploy the model using MLflow Models. An **MLflow Model** is a standard format for packaging machine learning models that can be used in a variety of downstream tools — for example, real-time serving through a REST API or batch inference on Apache Spark.

At the bottom, you can see that the call to ```mlflow.sklearn.log_model``` produced two files. The first file, ```MLmodel```, is a metadata file that tells MLflow how to load the model. The second file, ```model.pkl```, is a serialized version of the linear regression model that you trained.

The first file, MLmodel, is a metadata file that tells MLflow how to load the model. The second file, model.pkl, is a serialized version of the linear regression model that you trained.

To deploy the server, run (replace the path with your model’s actual path):

```bash
mlflow models serve -m mlruns/0/${modelHash}/artifacts/model -p ${port}
```

The version of Python used to create the model must be the same as the one running ```mlflow models serve```. 

Once you have deployed the server, you can pass it some sample data and see the predictions.

The following example uses curl to send a JSON-serialized pandas DataFrame with the split orientation to the model server. For more information about the input data formats accepted by the model server, see [the MLflow deployment tools documentation](https://mlflow.org/docs/latest/models.html#local-model-deployment).

```bash
# On Linux and macOS
curl -X POST -H "Content-Type:application/json; format=pandas-split" --data '{"columns":["alcohol", "chlorides", "citric acid", "density", "fixed acidity", "free sulfur dioxide", "pH", "residual sugar", "sulphates", "total sulfur dioxide", "volatile acidity"],"data":[[12.8, 0.029, 0.48, 0.98, 6.2, 29, 3.33, 1.2, 0.39, 75, 0.66]]}' http://127.0.0.1:1234/invocations

# On Windows
curl -X POST -H "Content-Type:application/json; format=pandas-split" --data "{\"columns\":[\"alcohol\", \"chlorides\", \"citric acid\", \"density\", \"fixed acidity\", \"free sulfur dioxide\", \"pH\", \"residual sugar\", \"sulphates\", \"total sulfur dioxide\", \"volatile acidity\"],\"data\":[[12.8, 0.029, 0.48, 0.98, 6.2, 29, 3.33, 1.2, 0.39, 75, 0.66]]}" http://127.0.0.1:1234/invocations
```

The server should respond with output similar to below output.

```bash
[6.379428821398614]
```