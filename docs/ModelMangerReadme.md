<!-- START_HIDDEN_SECTION -->

# Model Manager

A `dovedb.core.module.ModelManager` organizes your PyTorch code into 6 sections:

- Initialization (`__init__`).
- Train Loop (`dovedb.core.module.ModelManager.training_step`)
- Validation Loop (`dovedb.core.module.ModelManager.validation_step`)
- Test Loop (`dovedb.core.module.ModelManager.test_step`)
- Prediction Loop (`dovedb.core.module.ModelManager.predict_step`)
- Optimizers and LR Schedulers (`dovedb.core.module.ModelManager.configure_optimizers`)

## Initialization 

In DoveDB, you only need one entity `ModelManager` to manage all the model entities. You just need to run the following line of code to complete the initialization.

```
mmanager = ModelManager()
```

| Method | Description |
| --- | --- |
| `__init__` | Define initialization of Model Manager |
| `dovedb.core.module.ModelManager.create_model` | To create a model entity in Model Manager |
| `dovedb.core.module.ModelManager.delete_model` | To delete a model entity in Model Manager |
| `dovedb.core.module.ModelManager.deploy_model` | To deploy a model entity in Model Manager |
| `dovedb.core.module.ModelManager.train_model` | To train a model |
| `dovedb.core.module.ModelManager.valid_model` | To valid a model, do not run the all |
| `dovedb.core.module.ModelManager.inference` | Complete the inference of a certain model on a certain data. |

## Create Model

All you need to do is pass in a string that matches the rule, and `ModelManager` automatically creates a model entity for easy subsequent management. And returns a unique entity id that corresponds to the model entity that was created.

```
# create detector model 
detect_model_info = "DEC_YOLOV5"
detect_model_id = ModelManager.create_model(detect_model_info)

# create tracker model
track_model_info = "MOT_SORT"
track_model_id = ModelManager.create_model(track_model_info)

# create detector model with your weight
track_model_info = "MOT_SORT_./weights/pts/yolov5.pt"
track_model_id = ModelManager.create_model(track_model_info)
```

**Parameters:**

- `model_info` (str): The model information that you want to create.

**Return type:** Union[int]

**Returns:**

The `model_id` in the `ModelManager` that representates the model your created in Model Manager.

## Deploy Model

Due to limited resources, DoveDB supports limited computing resource allocation for models.

```
device_id = 0
deploy_flag = ModelManager.deploy_model(model_id, device_id)
```

**Parameters:**

- `model_id` (str): The model that you want to deploy.
- `device_id` (int): The device id.

**Return type:** Union[bool] 

**Returns:**

True if success, otherwise False.

## Delete Model

This function will help you how to delete a model by `model_id` you created.

```
delete_flag = ModelManager.delete_model(model_id)
```

**Parameters:**

- `model_id` (str): The model that you want to delete.

**Return type:** Union[bool] 

**Returns:**

True if success, otherwise False.

## Train
This function will help you train a model with a specified `model_id`, and the model manager will automatically read the data and complete the training steps.

```
# Train the specified detection model through the dataset path.
data_dir = "/your/path/root/"
train_flag, error_info = ModelManager.train_model(model_id, data_dir)

# Train the specified detection model through the dataset.
dataset = [images, labels]
train_flag, error_info = ModelManager.train_model(model_id, dataset)
```

**Parameters:**

- `model_id` (str): The model that you want to train.
- `data_info` (str/list): The dataset that the model was trained on.

**Return type:** Union[bool, str] 

**Returns:**

True if success, otherwise False. If train_flag is False, we will print the error.

## Valid
This function will help you train a model with a specified `model_id`, and the model manager will automatically read the data and complete the training steps.

```
# Valid the specified detection model through the dataset path.
data_dir = "/your/path/root/"
valid_flag, error_info = ModelManager.valid_model(model_id, data_dir)

# Valid the specified detection model through the dataset.
dataset = [images, labels]
valid_flag, error_info = ModelManager.valid_model(model_id, dataset)
```

**Parameters:**

- `model_id` (str): The model that you want to train.
- `data_info` (str/list): The dataset that the model was trained on.

**Return type:** Union[bool, str] 

**Returns:**

True if success, otherwise False. If valid_flag is False, we will print the error.

## Inference
This function will help you valid a model with a specified `model_id`, and the model manager will automatically read the data and complete the Inference step.

```
# Inference the specified detector model.
inputs = [image...]
inference_result = ModelManager.inference(model_id, inputs)

# Inference the specified tracker model through the dataset path.
inputs = [[image, detection]...]
inference_result = ModelManager.inference(model_id, inputs)
```

**Parameters:**

- `model_id` (str): The model that you want to inference.
- `inputs` (str/list): The input format must conform to the deployed model.

**Return type:** Union[list] 

**Returns:**

The model will return inference results. If it is a deployed detection model, the inference result will be a list, and the format of each list element is as follows: [*x*,*y*,*w*,*h*,*cls*,*conf*,None]; if it is a deployed tracking model, the inference result will be will be a list with each list element in the following format: [*x*,*y*,*w*,*h*,*cls*,*conf*,*tid*]. *x*, *y*, *w*, *h* is the bounding box of the object in the frame. *cls* is the class name of the object. *conf* is the confidence in the detection/tracking of the object. *tid* is the unique object id of the object. 