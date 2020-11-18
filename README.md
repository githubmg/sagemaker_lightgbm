## Bring your own LightGBM model to SageMaker

SageMaker is a data science managed service that gives you the flexbility to bring your own container following the service´s specification´s described in the <a href='https://docs.aws.amazon.com/sagemaker/latest/dg/adapt-training-container.html'> documentation </a>.

In the lightgbmscript notebook you can find a simple lightgbm pipeline trained on the open source Titanic dataset.

In the lightbyo notebook you´ll find a step by step solution to build a container with the training and inference code and deploy it to your account´s ECR and use it with SageMaker´s training and inference service. 