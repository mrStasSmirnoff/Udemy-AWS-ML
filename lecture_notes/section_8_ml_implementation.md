## SageMaker and Docker Containers

### SageMaker + Docker
  * All models in SageMaker are hosted in Docker containers
  * Pre-build images or use your own training and inference code
  * Containers are isolated, and contain all dependencies and resources needed to run

### Using docker
  1. Input data (S3/ORACLE) -> Model Training (Docker container)
  2. Training jobs generate artifacts (models) and save to another S3
  3. The models from step 2 is being deployed (also a docker container) along with the inference code
     * Images from 1 and 3 are living in Amazon ECR
  4. Model(s) will have endpoints to expose them to outside applications

### Structure of a training container
/opt/ml
 * input
   * config
     * hyperparameters.json
     * resourceConfig.json
   * data
     * input data (or channels)
 * model
 * code
   * inference.py
 * output
   * failure

### Structure of a deployment container
/opt/ml
  * model
    * *model files*


### Assembling it all in a Dockerfile
  * FROM tensorflow/tensorflow:2.0.0a0  # image
  * RUN pip install sagemaker-containers
  * COPY train.py /opt/ml/code/train.py # copies the training code inside the container
  * ENV SAGEMAKER_PROGRAMM train.py # defines train.py as script entrypoint
  
### Production Variants
  * You can test out multiple models on live traffic using Production Variants
  * Variant Weights tell SageMaker how to distribute traffic among them
  * So, you could roll out a new iteration of your model at say 10% variant weight
  * Once you’re confident in its performance, ramp it up to 100%
  * This lets you do A/B tests, and to validate performance in real-world settings
  * Offline validation isn’t always useful

### Managing SageMaker Resources
  * In general, algorithms that rely on deep learning will benefit from GPU instances 
    (P2 or P3) for training
  * Inference is usually less demanding and you can often get away with compute instances 
    there (C4, C5)

### Managed Spot Training
  * Can use EC2 Spot instances for training
    * Save up to 90% over on-demand instances
  * Spot instances can be interrupted!
    * Use checkpoints to S3 so training can resume
  * Can increase training time as you need to wait for spot instances to become available

### Elastic Inference (only works for DL)
  * Accelerates deep learning inference
    * At fraction of cost of using a GPU instance for inference
  * EI accelerators may be added alongside a CPU instance
    * ml.eia1.medium / large / xlarge
  * EI accelerators may also be applied to notebooks

### Automatic Scaling
  * You set up a scaling policy to define target metrics, min/max capacity, cooldown periods
  * Works with CloudWatch
  * Dynamically adjusts number of instances for a production variant
  * Load test your configuration before ,using it!

### Serverless Inference
  * Specify your container, memory requirement, concurrency requirements
  * Underlying capacity is automatically provisioned and scaled
  * Good for infrequent or unpredictable traffic; will scale down to zero when there are no requests
  * Charged based on usage
  * Monitor via CloudWatch
    * ModelSetupTime, Invocations, MemoryUtilization

### Amazon SageMaker Inference Recommender
  * Recommends best instance type & configuration for your models
  * Automates load testing  model tuning
  * Deploys to optimal inference endpoint
  * How it works:
    * Register your model to the model registry
    * Benchmark different endpoint configurations
    * Collect & visualize metrics to decide on instance types
    * Existing models from zoos may have benchmarks already
  * Instance Recommendations
    * Runs load tests on recommended instance types
    * Takes about 45 minutes
  * Endpoint Recommendations
    * Custom load test
    * You specify instances, traffic patterns, latency requirements, throughput requirements
    * Takes about 2 hours

### Inference Pipelines
  * Linear sequence of 2-15 containers
  * Any combination of pre-trained built-in algorithms or your own algorithms in Docker containers
  * Combine pre-processing, predictions, post-processing
  * Can handle both real-time inference and batch transforms


