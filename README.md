# Deploying a Simple Tekton Pipeline

This README provides instructions on deploying a simple Tekton pipeline that consists of a single Task. The pipeline uses an Ubuntu container to execute a simple 'echo' command.

## Prerequisites

Before you begin, ensure you have the following prerequisites:

- A running Kubernetes cluster.
- 'kubectl' installed and configured to connect to your cluster.
- Internet access to fetch Tekton Pipelines components.

## Step 1: Install Tekton Pipelines

To deploy Tekton Pipelines, run the following command:

```
kubPectl apply --filename https://storage.googleapis.com/tekton-releases/pipeline/latest/release.yaml
```

This command installs the Tekton Pipelines components into your Kubernetes cluster.

## Step 2: Deploy the Simple Task

Now, let's deploy a simple Tekton Task that uses an Ubuntu container to execute an 'echo' command. Create a file named 'simple-task.yaml' with the following content:

```
apiVersion: tekton.dev/v1beta1
kind: Task
metadata:
  name: simple-task
spec:
  steps:
    - name: echo
      image: ubuntu
      command:
        - echo
      args:
        - 'Hello, Tekton!'
```

Apply the Task configuration to your cluster:

```
kubectl apply -f simple-task.yaml
```

This creates the Task named 'simple-task'.

## Step 3: Create the Tekton Pipeline

Next, create a Tekton Pipeline that references the Task. Create a file named 'simple-pipeline.yaml' with the following content:

```
apiVersion: tekton.dev/v1beta1
kind: Pipeline
metadata:
  name: simple-pipeline
spec:
  tasks:
    - name: simple-task
      taskRef:
        name: simple-task
```

Apply the Pipeline configuration to your cluster:

```
kubectl apply -f simple-pipeline.yaml
```

This defines the Pipeline named 'simple-pipeline' that uses the 'simple-task' we created earlier.

## Step 4: Run the Pipeline

To execute the pipeline, create a Tekton PipelineRun. Create a file named 'simple-pipeline-run.yaml' with the following content:

```
apiVersion: tekton.dev/v1beta1
kind: PipelineRun
metadata:
  name: simple-pipeline-run
spec:
  pipelineRef:
    name: simple-pipeline
```

Apply the PipelineRun configuration to your cluster:

```
kubectl apply -f simple-pipeline-run.yaml
```

This creates the PipelineRun named 'simple-pipeline-run' and starts the pipeline execution.

## Step 5: View the Pipeline Output

To view the output of the pipeline and the 'echo' command, check the logs of the TaskRun's pod:

```
kubectl logs -n <namespace> <pod-name>
```

Replace '<namespace>' with the appropriate namespace, and '<pod-name>' with the pod name associated with your TaskRun.

You should see the 'Hello, Tekton!' message in the logs.
