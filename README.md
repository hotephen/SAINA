# SAINA
Straggler-Aware In-Network Aggregation (SAINA) mitigates the straggler problem in distributed deep learning while preventing accuracy degradation. It aggregates local gradients of the fastest $k$ workers to exclude stragglers
and changes $k$ adaptively to balance the tradeoff between training speed and accuracy.

We proposed Switch-Friendly Convergence Detection (SFCD) algorithm which detects a convergence point and increases $k$ incrementally whenever a training error reaches the convergence point. The SFCD algorithm employs a simple bitwise operation on the signs of global gradients, which is supported by commodity programmable switches and thus it can be implemented practically. Simulation results demonstrate that SAINA can reduce the waiting time for stragglers and shows higher time-to-accuracy (TTA) performance than the existing in-network aggregation scheme and the straggler mitigation schemes.

The source code of SAINA was implemented on top of [SwitchML](https://github.com/p4lang/p4app-switchML).


# Performance Results
### Time-to-Accuracy Performance (VGG-16)

| Batch Size = 16 | Batch Size = 32 | Batch Size = 64 |
|------------------|------------------|------------------|
| ![VGG-16 (batch size = 16)](graphs/vgg16_batch16_tta.png) | ![VGG-16 (batch size = 32)](graphs/vgg16_batch32_tta.png) | ![VGG-16 (batch size = 64)](graphs/vgg16_batch64_tta.png) |

### Time-to-Accuracy Performance (SqueezeNet)

| Batch Size = 16 | Batch Size = 32 | Batch Size = 64 |
|------------------|------------------|------------------|
| ![SqueezeNet (batch size = 16)](graphs/squeezenet_batch16_tta.png) | ![SqueezeNet (batch size = 32)](graphs/squeezenet_batch32_tta.png) | ![SqueezeNet (batch size = 64)](graphs/squeezenet_batch64_tta.png) |

### Time-to-Accuracy Performance (ResNet50)

| Batch Size = 16 | Batch Size = 32 | Batch Size = 64 |
|------------------|------------------|------------------|
| ![ResNet50 (batch size = 16)](graphs/resnet50_lr0.001_batch16_tta.png) | ![ResNet50 (batch size = 32)](graphs/resnet50_lr0.001_batch32_tta.png) | ![ResNet50 (batch size = 64)](graphs/resnet50_lr0.001_batch64_tta.png) |

For ResNet50 model, when the batch size is set to 64, all schemes could not reach the target accuracy. Although we need to investigate better hyperparameters for improving the performance of all schemes, SAINA can reach the target accuracy faster than other schemes for other batch sizes. 

### Effect of $s_{th}$
| $s_{th}$ |    VGG-16     |  SqueezeNet   |
|----------|---------------|---------------|
|   30%    |     472.4s    |     771.2s    |
|   40%    |     448.8s    |     543.1s    |
|   50%    |     426.0s    |     401.0s    |
|   60%    |     555.6s    |     508.4s    |
|   70%    |     564.0s    |     530.3s    |

### Effect of $c_{th}$
| $c_{th}$ |    VGG-16     |  SqueezeNet   |
|----------|---------------|---------------|
|   1    |     451.0s    |     475.5s    |
|   3    |     426.3s    |     464.1s    |
|   5    |     426.0s    |     401.0s    |
|   7    |     451.1s    |     417.3s    |
|   9    |     463.0s    |     434.1s    |
|   $k$* |     423.8s    |     396.5s    |


### Effect of SFCD Algorithm

| Scheme              | VGG-16 Final Accuracy | VGG-16 TTA Reduction     |
|---------------------|----------------------|----------------------|
| SAINA ($k$=1)       | 73.9%                | -                    |
| SAINA ($k$=8)       | 76.3%                | 1.51x                |
| SAINA ($k^*$)       | 78.0%                | 1.70x                |


| Scheme              | VGG-16 Final Accuracy | SqueezeNet TTA Reduction |
|---------------------|--------------------------|--------------------------|
| SAINA ($k$=1)       | 70.1%                    | 2.28x                    |
| SAINA ($k$=8)       | 72.2%                    | 2.15x                    |
| SAINA ($k^*$)       | 72.5%                    | 2.84x                    |

The above tables are results when the batch size is set to 32. In these tables, Final Accuracy means the accuracy when all training is completed, and TTA Reduction means how much TTA is reduced compared to when $k$ is fixed to 16.
