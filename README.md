<div align="center">
  
# **Neuralangelo**

#*Updated Install 2025*
## Easy install guide with included dependencies "updated to work as of 2025"
This repository provides an easy install for Neuralangelo, NVIDIA’s neural surface reconstruction framework. It updates dependencies for a seamless setup; Built for smooth deployment, ensuring high-fidelity 3D reconstruction with minimal hassle.
## *Original authors of Neuralangelo*
[Zhaoshuo Li](https://mli0603.github.io/), [Thomas Müller](https://tom94.net/), [Alex Evans](https://research.nvidia.com/person/alex-evans), [Russell H. Taylor](https://www.cs.jhu.edu/~rht/), [Mathias Unberath](https://mathiasunberath.github.io/), [Ming-Yu Liu](https://mingyuliu.net/), [Chen-Hsuan Lin](https://chenhsuanlin.bitbucket.io/)

**The code is built upon the Imaginaire library from the Deep Imagination Research Group at NVIDIA.
For business inquiries, please submit the [NVIDIA research licensing form.](https://www.nvidia.com/en-us/research/inquiries/)**
</div>

---

# **1.Installation**

## *Before installation, please read the requirements below*

*prior to reading or attempting to use Neuralangelo ensure you have an Nvidia Graphics Card & Use of Cuda based technology.*

## **Install the below** 

1. Install [Nvidia's 11.7 Cuda Drivers](https://developer.nvidia.com/cuda-11-7-0-download-archive) on the machine and reformat your image & system.
2. Install [WSL](https://learn.microsoft.com/en-us/windows/wsl/install) and set up a Debian based environment.
3. Install [Miniconda](https://docs.anaconda.com/miniconda/) and activate the dependencies on your WSL.
4. Install [VSC](https://code.visualstudio.com/download)
5. Update all your dependencies in WSL & Install git
6. Clone the following repository: [Neuralangelo](https://github.com/NVlabs/neuralangelo)

---

# **2.Setup**

Read the following [Install Guide](https://github.com/beasmith152/Easy-Install-Neuralangelo-includes-updated-dependency-list-/blob/main/INSTALLGUIDE.md)

# *Conda Setup* 

If you are confused by the below; please read the above setup guide.

The conda environment for Neuralangelo. Read the above guide and activate the environment neuralangelo with the below commands.

**IMPORTANT (ensure you edit standard neuralangelo.yaml file with [updated](https://github.com/beasmith152/Easy-Install-Neuralangelo-includes-updated-dependency-list-/blob/main/neuralangelo.yaml) file) before running the below command**

```
{
conda env create --file neuralangelo.yaml
conda activate neuralangelo
}
```
# **3.after you have ran neuralangelo.yaml file located above and activated the conda environment**

---

### *install the dependencies and modify the preferences below:*


### **install gcc/g++ through ubuntu toolchain**


sudo add-apt-repository ppa:ubuntu-toolchain-r/ppa -y


sudo apt install g++-11 gcc-11


### **Switch Ubuntu's preferences from standard for g++/gcc**


ls /usr/bin/gcc*


ls /usr/bin/g++*


### **then update the alternative**


sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-11 100


sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-11 100


### **finish manually selecting alt gcc/g++**


sudo update-alternatives --config gcc


sudo update-alternatives --config g++

---

# **Install the following Pytorch libraries; This will work for building tinywheels**


pip install torch==2.0.0+cu117 --extra-index-url https://download.pytorch.org/whl/cu117


pip install torchvision==0.15.0+cu117 --extra-index-url https://download.pytorch.org/whl/cu117



**4.After installing these dependencies you should be able to install the [requirements.txt](https://github.com/beasmith152/Easy-Install-Neuralangelo-includes-updated-dependency-list-/blob/main/requirements.txt)**

---

# **5.Dataset Creation & Training Instructions**

---

After your installation of all dependencies and following the above instructions you can now start training and creating custom datasets by following these instructions.


[dataset creation & training](https://github.com/beasmith152/Easy-Install-Neuralangelo-includes-updated-dependency-list-/blob/main/Dataset%20Creation%20%26%20Training%20Instructions.md)

---

# **6.Exporting the Mesh/Iso Surface Extraction**

---

[dataset creation & training](https://github.com/beasmith152/Easy-Install-Neuralangelo-includes-updated-dependency-list-/blob/main/Dataset%20Creation%20%26%20Training%20Instructions.md)

**After Iteration is complete continue to ISO surface extraction**

   
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
    

**check the checkpoint file; you will find the datasetname.ply file which is a 3d file exported to that location.**

---

# **7.Helpful Tips for creating footage for datasets**

---

The video capture sequence may contain significant motion blur or out-of-focus frames. Higher shutter speed (reducing motion blur) and smaller aperture (increasing focus range) are very helpful.

If you want the entire object you need to record from multiple angles without interuption, also try and control your lighting environment; it does not like bright white reflections and will attempt to render them as solid objects. 


