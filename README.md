# Data preprocessing

To structure [Turku Paraphrase Corpus data](https://github.com/TurkuNLP/Turku-paraphrase-corpus/tree/main/data-fi) files (train.json, dev.json, test.json and texts.json.gz) the same way as SQUAD run the `make_paraphrase_data.py`

```bash
python3 make_paraphrase_data.py 
--file dev.json 
--context texts.json.gz 
--output qa_data/dev.json
```

# Fine-tuning BERT

All of the original code for fine tuning can be found in [this HuggingFace repository](https://github.com/huggingface/transformers/tree/master/examples/pytorch/question-answering). The code for this project was fetched from the mentioned repository on 7th of June 2021.

To run the code in CSC Mahti:

Clear the loaded modules and load pytorch

```bash
module purge 
module load pytorch/1.8
```

Important! To run HuggingFace example code install transformers from the source:

```bash
git clone https://github.com/huggingface/transformers
cd transformers
python -m pip install --user . 
```
To install the requirements:
```bash
python -m pip install --user -r requirements.txt
```

**Notes:** 
- This script only works with models that have a fast tokenizer 
- If your dataset contains samples with no possible answers (like SQUAD version 2), you need to pass along the flag `--version_2_with_negative`.

Example code for fine-tuning BERT on the preprocessed Turku Paraphrase Corpus dataset. 

```bash
python3 run_qa.py \
  --model_name_or_path TurkuNLP/bert-base-finnish-cased-v1 \
  --train_file train.json \
  --validation_file dev.json \
  --test_file test.json \
  --do_train \
  --do_eval \
  --do_predict \
  --per_device_train_batch_size 16 \
  --per_device_eval_batch_size 16 \
  --learning_rate 2e-5 \
  --num_train_epochs 2 \
  --max_seq_length 512 \
  --doc_stride 128 \
  --version_2_with_negative \
  --output_dir /output/ \
  --cache_dir /caches/ \
```
