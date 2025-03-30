# AI-enabled-Framework-for-Automated-and-Precise-Mandible-Reconstruction-in-Surgical-Planning
Hao Wang, Chenfan Xu, Wen Du, Wenbo Zhang, Jiamin Wu, Yao Yu, Shuo Liu, Zhiming Cui, and Xin Peng

- [ ] Inference code and model checkpoint
- [ ] Training code
- [ ] Preprocessing code

### Example
- [ ] Synthetic defective mandible (Input)
- [ ] Corresponding complete mandible (Ground truth)
- [ ] Corresponding reconstructed mandible (Prediction)

### Getting started
1. Install packages in `requirements.txt`. We train and test our model on a 40G A100 GPU with 11.8 CUDA, 2.0.0 pytorch, 3.9 python and 10.1.0 GNU.
```angular2html
conda create -n Completion python=3.9 -y
conda activate Completion
pip install -r requirements.txt
cd data_processing/libmesh
pip install -e .
cd ../libvoxelize
pip install -e .
```
### Inference
2.  Prepare your defective mandible mask in the format of NIFIT file. IF you need the reconstructed mandible mask aligned with your defective mandible mask, the defective mandible mask must be resample to the resolution $256 \times 256 \times 256$ (Optional).

3.  Preprocess the defective mandible mask file.
```angular2html
python process.py --pre \
                  --inp /directory/of/your/defective/mandible/mask/files \
                  --out /path/to/store/preprocessed/data
```
4.  Reconstruct the complete mandible with our checkpoint.
```angular2html
python run.py -b config.yaml \
              -l ./exp \
              -n Completion \
              --test ./last.ckpt \
              data.params.infer_dir=/path/to/store/preprocessed/data
```
Tips: If your want to specify your customized output directory, you can set the parameter of "model.params.out_dir" with your own path.
5.  Postprocess the output to obtain the complete mandible mesh and mask.
```angular2html
python process.py --inp /directory/of/your/defective/mandible/mask/files \
                  --out /path/to/store/postprocessed/mesh/and/mask \
                  --occ ./exp/Completion/test/step-000000
```
