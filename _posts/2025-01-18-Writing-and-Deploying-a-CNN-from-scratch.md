---
layout: default
title: "Deploying a prod-ready AI/ML system Digit Classifier CNN to prod with TensorFlow + metrics using TensorBoard"
date: 2025-01-18 15:00:00 -0000
---

---

# {{ page.title }}

**Published:** {{ page.date | date: "%B %d, %Y" }}

---

How it works

Core:

Python/Flask web app serving our Convolutional Neural Network + Model for MNIST digit classification.

Model: TensorFlow/Keras CNN architecture trained on MNIST dataset.

Monitoring: Prometheus for real-time metrics, Tensorboard for model training metrics and architecture visualization.

## Primary application/model training services

We start with our primary application serving our trained model. I wrote the model and defined the training logic.

I initially began deploying this locally on a 4090 for testing directly, but I quickly realized as I began deploying to Fly that containerization was the way to go. It was pretty clunky to train the model AS I was deploying to production infrastructure. You’d run into all sorts of issues and constraints, hit timeout thresholds, etc. (especially with 10 epochs).

Instead, I utilized Docker for testing each individual service as well as training the model prior to deployment. This way, as part of our deployment pipeline, we train the model and quickly deploy the already trained model straight to prod.

I also implemented different epochs for each environment. Dev only implements 5 epochs for training, whereas pre-training for prod deployment utilizes the full 10 epochs. I also implemented model versioning via saved weights in our model services directory.

## Tensorboard service

Yeah…the Tensorboard service. In dev, this worked fine. Deploying to prod on Fly? Not so much. I struggled with proxying different services from the same ‘machine’ (Fly lingo), and ended up deploying the Tensorboard as a microservice architecture in another app classification (xx-tensorboard). I also went with the lightweight implementation of Tensorboard in order to conserve resources, but it can go pretty deep into histograms and historical data if you’re monitoring real-time (experimented with this in dev a lot). Super insightful to see model performance with Tensorboard.

## Deployment

Deployment starts with pre-downloaded MNIST datasets in a container to optimize dependency management. Our build phase uses a multi-stage build process to minimize the final image size. I avoided runtime downloads by the aforementioned pre-downloading of the MNIST datasets. The multi-stage build process also compiles all Python dependencies with specific version pinning so we can repro later (important for containerization).

### runtime

Started implementing graceful startup with an 180s init period for model loading.

Use Unicorn with the gthread worker class for concurrent request handling.

Configure TensorFlow optimizations for CPU-only inference (didn’t really need or want GPU instances in the cloud..$$)

### health/monitoring

The build implements progressive health checks that account for the model init time.

Includes a Prometheus metrics endpoint for real-time performance monitoring (API)

Separate logging for application events and model predictions.

### Tensorboard service deployment

As mentioned, I pivoted to running a lightweight version of Tensorboard over increasing instance size (minimal memory allocation, 512MB, since primarily serving static files) and implemented efficient log handling through volume mounts (more containerization best practices IMO)

Managed logs by maintaining persistent storage for training logs, automatic log rotation to prevent storage issues, and uses a separate build process entirely to handle log file permissions correctly.

## Orchestration

Well, we’re using Fly so..yknow. Super convenient to implement zero-downtime deployments, maintain service availability during updates (model training, etc.), and health checks to validate new deployments before switching traffic.

Environment separation was also important here. I had to set up an environment with rapid model training abilities, a prod env using pre-trained models, and staging environments for validation (particularly for those tricky model updates).

Service discovery includes, of course, automatic DNS configuration (dynamic, through Fly), internal service comms for metric aggregation, and load balancing across multiple regions when scaled.

## Challenges

### Model init

Had to implement asynchronous model loading to prevent blocking the web server.

Created the staged startup process to handle dependencies correctly.

Added robust error handling for those pesky failed model initializations.

### Resource Management

Optimized memory usage for all services (many, many app/server crashes later).

Implemented proper cleanup of temp files (oops).

Configured scaling policies (pls don’t force me to scale, $$).

### Monitoring integrations

Monitoring is a pain, but it’s simply fascinating.

Had to separate concerns between app metrics and model performance.

Implemented custom health checks for model validity.

Create detailed logging for debugging and performance analysis.

### Infrastructure

Scaling strategies, horizontal scaling for the main application.

Vertical scaling for the TensorBoard service.

Resource limits to prevent runaway processes (did I mention those app/server crashes?).

Security (very important), implemented network isolation between services, proper handling of environment variables, access control for monitoring endpoints.

### Maintenance

Automated backup procedures for model artifacts.

Log rotation, cleanup.

Version control for both the application code (what I’m used to) AND model weights (new to me).