# Image Classification using AWS SageMaker

Use AWS Sagemaker to train a pretrained model that can perform image classification by using the Sagemaker profiling, debugger, hyperparameter tuning and other good ML engineering practices. This can be done on either the provided dog breed classication data set or one of your choice.

## Project Set Up and Installation
Enter AWS through the gateway in the course and open SageMaker Studio. 
Download the starter files.
Download/Make the dataset available. 

## Dataset
The provided dataset is the dogbreed classification dataset which can be found in the classroom.
The project is designed to be dataset independent so if there is a dataset that is more interesting or relevant to your work, you are welcome to use it to complete the project.

### Access
Upload the data to an S3 bucket through the AWS Gateway so that SageMaker has access to the data. 

## Hyperparameter Tuning
What kind of model did you choose for this experiment and why? Give an overview of the types of parameters and their ranges used for the hyperparameter search

I decided on the ResNet18, I decided to use this architecture because it is known to give good performance in the area of computer vision tasks, image classification and object detection. I also selected because of its computationally efficiency and its reputation as an easy network to train.

The hyperparameters selected and thier ranges are as follows:-

 - batch_size: This determines how many samples used in each mini-batch during training used [ 64, 128]
 - Learning rate(lr): This controls the step size in the gradient desent algorithm used a range of 0.0001 and  0.1
 - epsilon value(eps): This is used in batch normalization the range of  0.00000001 and 0.000000001 was used.  
 - weight_decay: This adds a penalty term to loss function and used a range of 0.001 and 0.1

Remember that your README should:
- Include a screenshot of completed training jobs
<img width="1414" alt="completed training" src="https://user-images.githubusercontent.com/5279800/221414600-6952fefe-54b1-4d4f-a090-f76e6cf8d2c0.png">
- Logs metrics during the training process
<img width="1638" alt="trainingJob" src="https://user-images.githubusercontent.com/5279800/221414611-084c7122-e5a0-4fc1-b5f1-6839e1b36d21.png">

- Tune at least two hyperparameters
<img width="1448" alt="hyperparametertuning" src="https://user-images.githubusercontent.com/5279800/221414642-c4cc3c02-a6b0-4522-a762-f43d275b6bdb.png">

- Retrieve the best best hyperparameters from all your training jobs
<img width="1474" alt="hyperparameterbest" src="https://user-images.githubusercontent.com/5279800/221414659-9c3422d6-3af6-459c-94cd-0226f095a294.png">

## Debugging and Profiling
**TODO**: Give an overview of how you performed model debugging and profiling in Sagemaker

The model debugging and profiling in Sagemaker was performed by setting the debugger hook to record and keep track of the loss criterion
metrics of the process . This occured in the training and validation/testing phases.

### Results
**TODO**: What are the results/insights did you get by profiling/debugging your model?

I was expecting a smooth crossEntropyloss output but it was somewhat an iregullar sinusodial line.This is because of fully connected layer used.
Making some changes while extensively experimenting will help find parameters that will give a straight line. Unfortunately i used up 
most of  my AWS credit in tuning and training.


**TODO** Remember to provide the profiler html/pdf file in your submission.


## Model Deployment
**TODO**: Give an overview of the deployed model and instructions on how to query the endpoint with a sample input.
I at first deployed my endpoint using the line below but kept getting errors after searching several threads in slack i came across
`predictor=estimator.deploy(initial_instance_count=1, instance_type='ml.m5.xlarge')`
https://sagemaker-examples.readthedocs.io/en/latest/frameworks/pytorch/get_started_mnist_deploy.html
This suggested using an inference script containing a `model_fn`.

After defining the inference script, calling with the line below and deploying i was able to make predictions.

`pytorch_model = PyTorchModel( model_data = model_location,
                            role = role,
                             entry_point= "inference.py",
                             py_version = "py36",
                             framework_version = "1.6",
                            predictor_cls = ImgPredictor
                            )`
`predictor = pytorch_model.deploy( initial_instance_count = 1, instance_type = "ml.t2.medium")` 

The predictions were made using the line below

` response = predictor.predict(payload, initial_args={"ContentType": "image/jpeg"})`
**TODO** Remember to provide a screenshot of the deployed active endpoint in Sagemaker.
<img width="1450" alt="activeendpoint" src="https://user-images.githubusercontent.com/5279800/221417102-0d411df7-c5af-495a-8657-e81d9206c25e.png">

## Standout Suggestions
**TODO (Optional):** This is where you can provide information about any standout suggestions that you have attempted.
