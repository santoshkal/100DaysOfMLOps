# Task:
The xFusionCorp Industries data science team needs a standardised Python environment for their new ML project. Set up a virtual environment with the required ML libraries on the *controlplane* host.


Create a Python virtual environment named `ml-env` under `/root/code/` using `python3 -m venv`.

Activate the environment and install the following packages: `numpy`, `pandas`, `scikit-learn`, and `matplotlib`.

Generate a `requirements.txt` file using pip freeze and save it at `/root/code/requirements.txt`.

---
# Solution:

- cd into the `code` dir:
`cd /root/code`

- Create the virtual enviornment
`python3 -m venv ml-env`

- Activate the virtual env:
`source ml-env/bin/activate`

- Install the requiremd packages within the new venv
`pip install numpy pandas scikit-learn metplotlib`

- Generate a `requirements.txt`
`pip freeze > /root/code/requirements.txt`

