# Dataset Creation & Training Process

---


# **Colmapping Process:**

---

**1. First find/take a video & download it (ensure you save it as an .mp4 and keep it in the main neuralangelo folder)**


**2. Then (prepare these commands):**



SEQUENCE=

PATH_TO_VIDEO=

DOWNSAMPLE_RATE=

SCENE_TYPE= {outdoor,indoor,object}



**(example)**

SEQUENCE=tdark

PATH_TO_VIDEO=tdark.mp4

DOWNSAMPLE_RATE=2

SCENE_TYPE=object


**notes**

SEQUENCE: your custom name for the video sequence.

PATH_TO_VIDEO: absolute/relative path to your video.

DOWNSAMPLE_RATE: temporal downsampling rate of video sequence (for extracting video frames).

SCENE_TYPE: can be one of  {outdoor,indoor,object}.



**3. Install these dependencies below. (only if first time using neuralangelo)**



sudo apt install ffmpeg colmap
pip install notebook


**4. Then use this command to begin ColMap:**



bash projects/neuralangelo/scripts/preprocess.sh ${SEQUENCE} ${PATH_TO_VIDEO} ${DOWNSAMPLE_RATE} ${SCENE_TYPE}



**5. After Colmapping is complete review the footage:**



input: **jupyter notebook**



This launches a webserver that only you can access through link provided in WSL/CMD
Run each cell, except second to last. Verifying info in cell 3 (change the name of the lego_dsl2 to filename_dsl2.
When you run the last one you should get a viewpoint and can readjust parameters under json_frame



**6. [YAML file changes](https://github.com/beasmith152/Easy-Install-Neuralangelo-includes-updated-dependency-list-/blob/main/base.yaml)**


After adjusting in the viewer; adjust the same parameters in neuralangelo/projects/configs then find the project **yourfilename.YAML** and input same changes there.


Also update **dict_size dependent on GPU parameters** (found below) before training in the base.yaml file located centrally in the neuralangelo folder.

**GPU VRAM**	   **Hyperparameter**
8GB	         dict_size=20, dim=4
12GB	       dict_size=21, dim=4
16GB	       dict_size=21, dim=8

batch size can be decreased with clarity loss;
set a good checkpoint. the full max iterations of 500,000 will take 30 hours to train.
1250 is too early to tell how the model will turn out. The results get far better past 200,000 iterations.


**7. enter CNTRL + C to stop the Jupyter notebook**

---

# **Training the Data**


**8. Enter each command seperately (change names to file name)**
EXPERIMENT=toy_example
GROUP=example_group
NAME=example_name
CONFIG=projects/neuralangelo/configs/custom/${EXPERIMENT}.yaml
GPUS=1  # use >1 for multi-GPU training!


**example**
In the case for the filename:
EXPERIMENT=filename
GROUP=example_group
NAME=example_name
CONFIG=projects/neuralangelo/configs/custom/${EXPERIMENT}.yaml
GPUS=1  


**Enter this command whole**

torchrun --nproc_per_node=${GPUS} train.py \
--logdir=logs/${GROUP}/${NAME} \
--config=${CONFIG} \
--show_pbar

*model should be training now & will save an iteration based on input of checkpoint in base.yaml file.*


**enter control +c to stop training once iteration is reached.**

*rename the iteration.pt to whatever you feel like.

---

# **Exporting the Mesh**

**9. After Iteration is complete continue to ISO surface extraction**

   
**Some useful notes:**

Add --textured to extract meshes with textures. (will take longer)
Add --keep_lcc to remove noises. May also remove thin structures.
Lower BLOCK_RES to reduce GPU memory usage.
Lower RESOLUTION to reduce mesh size.

**replace the names notated by xxx & input each command one by one**

CHECKPOINT=logs/${GROUP}/${NAME}/xxx.pt
OUTPUT_MESH=logs/${GROUP}/${NAME}/xxx.ply
CONFIG=logs/${GROUP}/${NAME}/config.yaml
RESOLUTION=2048
BLOCK_RES=128
GPUS=1  # use >1 for multi-GPU mesh extraction


**enter command below all at once:**


torchrun --nproc_per_node=${GPUS} projects/neuralangelo/scripts/extract_mesh.py \
    --config=${CONFIG} \
    --checkpoint=${CHECKPOINT} \
    --output_file=${OUTPUT_MESH} \
    --resolution=${RESOLUTION} \
    --block_res=${BLOCK_RES} 
    

**10. check the checkpoint file; you will find the datasetname.ply file which is a 3d file exported to that location.**


Congrats you did it! Now just take your .ply file and open it in your own 3D environment. 
