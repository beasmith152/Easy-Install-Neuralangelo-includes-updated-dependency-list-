# *Start from HERE (after installing 11.7 on the machine and reformatting the image)*

---

## **Install the below software:**

1. Install [WSL](https://learn.microsoft.com/en-us/windows/wsl/install)

2. open command prompt: input wsl --install

3. restart the computer

4. type in wsl

5. follow instructions in CMD to install Ubuntu

6. set up linux username & password

7. Install [Miniconda](https://docs.anaconda.com/miniconda/)

8. go to install tab; scroll to the bottom

9. choose linux tab & copy commands one by one into wsl.

10. enter sudo apt update && sudo apt upgrade

11. enter sudo apt-get install build-essential git g++

12. go to your documents/files and type in cmd in top bar

13. once cmd is open type in wsl to enter Ubuntu

14. type in git (check if github is installed)

15. type in: git clone https://github.com/NVlabs/neuralangelo (wait for the folder to clone)

16. cd (change directory) to neuralangelo

17. open neuralangelo.yaml (The original YAML will not work, visit the custom YAML file on this repository and paste into the neuralangelo.yaml)

18. type in: conda env create --neuralangelo.yaml

19. copy and paste conda activate neurangelo (should change directory and machine from base to display neuralangelo)

20. type export LIBRARY_PATH="/usr/lib/wsl/lib:$LIBRARY_PATH"

21. type in pip install -r requirements.txt 

22. go to data preparation page in (github) in order to prepare your datasets.

23. perform submodule updates
