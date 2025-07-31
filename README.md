# Craft Language Reference

## Overview

Craft is a specialized programming language for machine learning and data visualization 
that compiles to native TaliaOS .sey executables. It's designed to be lightweight yet powerful, 
with strong abstractions for common ML tasks.

## Basic Syntax

### Variables and Types

```craft
// Type inference
let x = 5                // Integer
let y = 3.14             // Float
let name = "Craft"       // String

// Explicit typing
let count: int = 10
let features: tensor<float, [28, 28]> = load_image("digit.png")
```

### Tensor Operations

```craft
// Create tensors
let a = tensor([1, 2, 3, 4])
let b = tensor([[1, 2], [3, 4]])

// Operations are concise and expressive
let c = a * b                     // Element-wise multiplication
let d = a @ b                     // Matrix multiplication
let e = a.reshape([2, 2])         // Reshaping
```

### Functions

```craft
// Function declaration
fn add(a: float, b: float) -> float {
    return a + b
}

// Lambda functions
let multiply = |a, b| a * b

// Higher-order functions
let data = [1, 2, 3, 4, 5]
let squared = data.map(|x| x * x)
```

## Neural Network Definition

Neural networks can be defined in an intuitive, declarative style:

```craft
model SimpleNN {
    // Sequential style
    input(784) -> 
    dense(128, activation: relu) -> 
    dense(10, activation: softmax) ->
    output
    
    // Or graph style for complex architectures
    let input = Input(784)
    let hidden = Dense(128, activation: relu)(input)
    let output = Dense(10, activation: softmax)(hidden)
    
    // Connect layers
    Graph(input, output)
}
```

## Data Handling

```craft
// Built-in data loading
let dataset = Dataset("mnist")

// Or create custom pipelines
pipeline ImagePipeline {
    source: "images/*.jpg"
    preprocess: [
        resize(224, 224),
        normalize,
        augment(flip: horizontal, rotation: 15)
    ]
    batch: 32
    shuffle: true
}
```

## Visualization

Visualization is a first-class citizen:

```craft
// Quick plots
plot(data)

// Customized visualizations
viz.create(type: "scatter") {
    data: results
    x: "epoch"
    y: ["training_loss", "validation_loss"]
    title: "Training Progress"
    interactive: true
}

// Live dashboards
dashboard {
    layout: grid(2, 2)
    plots: [
        viz.loss_curve(),
        viz.confusion_matrix(model, test_data),
        viz.feature_importance(model),
        viz.embedding_projector(model.layer("embedding"))
    ]
    update: 500ms  // Update every 500 milliseconds
    save: "training_dashboard"
}
```

## Hardware Acceleration

```craft
// Explicitly control hardware usage
with gpu {
    // Code to run on GPU
}

with accelerator("TPU") {
    // Code to run on TPU
}

// Power-aware computing
with power.efficient {
    // Run with power-saving optimizations
}
```

## Deployment

```craft
// Deploy models easily
deploy(MyModel) {
    format: "sey"        // TaliaOS native format
    optimize: "speed"    // Optimize for inference speed
    quantize: 8          // 8-bit quantization
    target: "mobile"     // Target mobile hardware
}
```

## Integration with TaliaOS

Craft integrates deeply with TaliaOS:

```craft
// Access system resources
let battery = system.battery.level
if battery < 0.2 {
    // Switch to power saving mode
    model.config.precision = "fp16"
}

// Native notifications
system.notify("Training complete!")
```
