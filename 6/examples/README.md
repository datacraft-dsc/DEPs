## Examples

These are the planned implementations of services that implement the invoke API interface

1. Addition 

A service that adds 2 numbers. Purely for demo and templating

2. Consent service

Takes a dataset (typically a CSV file) and returns a dataset (list of instances) filtered against a pre-approved whitelist. The whitelist itself is not managed by this service.

### Inputs

- input dataset

### Output

- filtered dataset

3. Model training with a private model 

This service takes a training and validation dataset, and returns a trained model. The details of the model are internal to the service. An example of a service: an image classification model which accepts mages of a certain size/type. It can then be retrained on a new image classification dataset, and returns a trained model.

### Inputs

- training dataset
- (optional) validation dataset

### Output

- trained model

4. Model training with private data.

Use case: a company has a private dataset, the contents of which are not to be revealed. The format of the dataset is made public. This service accepts a training algorithm (e.g. a notebook), which is given the private dataset to train on. The trained model is the output of this service.

### Inputs

- training algorithm

### Output

- trained model

5. Model prediction on a private model

A company has a trained model and it wants to deploy a prediction service. The service can accept a test dataset, and returns predictions from the model on each of those test data instances.

### Inputs

- test dataset

### Outputs

- predictions

5. Kaggle Kernel

Accepts a Kaggle kernel as input and returns multiple outputs (e.g the rendered notebook, and/or models)

Steps:
- Install docker-machine if not installed already.
- Run `docker-machine create -d virtualbox --virtualbox-disk-size "50000" --virtualbox-cpu-count "4" --virtualbox-memory "8092" docker2`
- Run `docker-machine start docker2`
- Run `docker pull kaggle/python`

### Inputs

- kaggle Docker instance
- Kaggle Notebook

### Outputs

- optional 

