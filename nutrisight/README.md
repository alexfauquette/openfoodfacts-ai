# NutriSight

Nutritional information is crucial in assessing the quality of food products, but often, this data is presented to consumers in the form of nutritional tables, which can be challenging to comprehend.

This project aims to address this issue by developing a tool that can automatically extract nutritional information from a photograph of food product’s packaging.

After 6 months of annotation, more than 3400 samples were annotated and proof-read. Some images were too blurry or did not contain nutrition images, so ~400 samples were discarded. The dataset is available on [Hugging Face](https://huggingface.co/datasets/openfoodfacts/nutrient-detection-layout).

We trained a Document AI model (LayoutLMv3-large) on this dataset. Different training hyperparameters and different model versions were tested to obtain a first model.
Then, we analyzed where the model predictions differed from the annotations, to spot possible annotation errors. Using this method, we fixed more than 100 annotation errors in the original dataset. A final model (v2) was trained on the cleaned dataset.

The model is also available on [Hugging Face](https://huggingface.co/openfoodfacts/nutrition-extractor).

## Dataset management

Every script related to dataset management and generation is located in the `dataset` directory:

- `1_create_dataset.py`: Create a JSON dataset directly from Open Food Facts database, in [Label Studio JSON format](https://labelstud.io/guide/tasks#Basic-Label-Studio-JSON-format).
- `2_create_project.py`: Create a Label Studio project, using the XML label config.
- `3_upload_dataset.py`: Upload the JSON dataset to the Label Studio project.
- `4_add_batch.py`: Segment tasks into batches of 100 in the Label Studio project.
- `5_update_checked_field.py`: Debug script to add missing "checked" field in the project.
- `6_push_dataset.py`: Download annotations from Label Studio, create a local dataset and push it on Hugging Face.
- `7_check_errors.py`: Check tasks and display possible annotation errors.
- `8_get_prepared_as_samples.py`: Display all tasks that were labeled as having nutrition values for prepared product.
- `9_detect_errors.py`: Compare annotations with model predictions and display possible errors.


## Model training

The model training script and the requirements are located in the `train` directory.
The `launch.sh` script is used to launch the training.


## ONNX Export

To export the model to ONNX format, create a virtualenv with the following dependencies:

```bash
pip install onnx==1.16.2 onnxruntime==1.19.2 torch==2.4.1+cpu transformers==4.44.2 optimum==1.22.0
```

Then run the following command:

```bash
optimum-cli export onnx -m openfoodfacts/nutrition-extractor --opset 19 --task token-classification nutrition-extractor-onnx-19
```

## Thanks to our sponsors!

The NutriSight project has indirectly received funding from the European Union’s Horizon Europe research and innovation action programme, via the DRG4FOOD – Open Call #1 issued and executed under the DRG4FOOD project (Grant Agreement no. 101086523).

<img src="./assets/DRG4FOOD_Logo_Icon+Type-Black.png" alt="Funded by DRG4Food" title="Funded by DRG4Food" height="100" />  
<img src="./assets/EN_FundedbytheEU_RGB_POS.png" alt="Funded by the EU" title="Funded by the EU" height="100" />