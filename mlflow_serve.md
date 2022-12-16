---
layout: default
title: "Models serving"
nav_order: 6
description: "Yummy MLflow models serving."
permalink: /yummy-mlflow-models-serving
---

# Yummy mlflow models serving

Performant rust implementation of the models server allows to deploy
mlflow models as Rest API.
The server expose the same [/invocations](https://www.mlflow.org/docs/latest/models.html#deploy-mlflow-models)
endpoint.

See: [benchmark](https://github.com/yummyml/yummy-benchmark-mlflow) to analyse response 
times and resource consumption in comparison to the mlflow server.

## Yummy mlflow

The MLflow rust wrapper currently supports models:
[x] lightgbm
[x] catboost (only binary classification)

The implementation currently supports MLflow models kept on local path.
To run the model run:

`yummy_mlflow.model_serve(MODEL_PATH, HOST, POST, LOG_LEVEL)`

example:
```python
import yummy_mlflow

yummy_mlflow.model_serve(model_path, '0.0.0.0', 8080, 'error')
```

The `yummy-mlflow` will expose HTTP server. 
The request response is compatible with MLflow model serving API.

Example:

Request:
```bash
curl -X POST "http://localhost:8080/invocations" \
-H "Content-Type: application/json" \
-d '{
    "columns": ["0","1","2","3","4","5","6","7","8","9","10",
               "11","12"],
    "data": [
     [ 0.913333, -0.598156, -0.425909, -0.929365,  1.281985,
       0.488531,  0.874184, -1.223610,  0.050988,  0.342557,
      -0.164303,  0.830961,  0.997086,
    ]]
}'
```

Response:
```json
[[0.9849612333276241, 0.008531186707393178, 0.006507579964982725]]
```


