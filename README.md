# Congestion Control Evaluation using Pantheon

This repository contains experimental results and custom trace files for evaluating multiple congestion control protocols using the [Pantheon](https://github.com/StanfordSNR/pantheon) framework.

>  Includes results for: **BBR**, **Cubic**, and **Sprout**  
>  Evaluated on: **1 Mbps, 200 ms RTT** and **50 Mbps, 10 ms RTT** profiles

---

## 📁 Repository Structure

```
Congestion-Control-Pantheon/
├── results_1mbps_200ms/      # Results for low bandwidth, high RTT
├── results_50mbps_10ms/      # Results for high bandwidth, low RTT
├── traces/                   # Mahimahi custom trace files
├── part_a_data/              # Raw logs and plots from initial test
└── README.md                 # Detailed setup & explanation
```

---

## 📊 Results Included

- `results_1mbps_200ms/`: Experiment outputs and plots for 1 Mbps bandwidth, 200 ms RTT
- `results_50mbps_10ms/`: Experiment outputs and plots for 50 Mbps bandwidth, 10 ms RTT
- `traces/`: Mahimahi trace files used to emulate the network profiles
- `part_a_data/`: Part A verification test logs and plots

---

## Full Setup Instructions (Ubuntu 18.04)

Follow these steps exactly to recreate the Pantheon environment and run experiments.

### Step 1: System Preparation

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install -y build-essential zlib1g-dev libncurses5-dev \\
libgdbm-dev libnss3-dev libssl-dev libreadline-dev libffi-dev \\
wget curl git net-tools unzip cmake pkg-config
```

---

### Step 2: Install Python 2.7.18 (Required for Pantheon)

```bash
cd /tmp
wget https://www.python.org/ftp/python/2.7.18/Python-2.7.18.tgz
tar -xf Python-2.7.18.tgz
cd Python-2.7.18
./configure --prefix=/opt/python2.7
make -j\$(nproc)
sudo make install
sudo ln -s /opt/python2.7/bin/python2.7 /usr/bin/python
python --version  # should print Python 2.7.18
```

---

### Step 3: Install pip2 and Required Packages

```bash
curl https://bootstrap.pypa.io/pip/2.7/get-pip.py -o get-pip.py
python get-pip.py
pip2 install --user pyyaml==5.1.2
echo 'export PATH=\$PATH:~/.local/bin' >> ~/.bashrc
source ~/.bashrc
```

---

### Step 4: Install Mahimahi Emulator

```bash
sudo apt install -y mahimahi
which mm-delay  # should return the path if installed correctly
```

---

### Step 5: Clone & Setup Pantheon

```bash
cd ~
git clone https://github.com/StanfordSNR/pantheon.git
cd pantheon
git submodule update --init --recursive
sudo tools/install_deps.sh
python src/experiments/setup.py --setup --all
```

---

## Running Experiments

Use the following command (example):

```bash
python src/experiments/test.py local \\
  --schemes "cubic bbr sprout" \\
  --uplink-trace traces/1mbps.trace \\
  --downlink-trace traces/1mbps.trace \\
  --runtime 60 \\
  --data-dir results_1mbps_200ms
```

Replace traces and folder name depending on test:
- `traces/50mbps.trace`
- `results_50mbps_10ms`

---

## Generate Plots and Reports

```bash
python src/analysis/analyze.py --data-dir results_1mbps_200ms
python src/analysis/plot.py --data-dir results_1mbps_200ms
python src/analysis/report.py --data-dir results_1mbps_200ms
```

Repeat for the second setup as needed.

---

## Part A (Initial Run Check)

To verify installation, a simple test was done using:

```bash
python src/experiments/test.py local --cubic bbr
```

Output plots and logs are available under `part_a_data`.

---

## Clone this Repo to Reproduce

```bash
git clone https://github.com/PravarshaErodula6/Congestion-Control-Pantheon.git
cd Congestion-Control-Pantheon
```

Then follow the installation steps above to rerun experiments or replot results.

---

## Notes

- This repo includes **only experiment results and trace files**.
- The full Pantheon framework should be cloned and set up separately using the guide above.
- Ensure Python version is 2.7.18 as required by Pantheon.
