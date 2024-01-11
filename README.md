# SunBlock router
The image of the software can be downloaded [here](https://www.dropbox.com/scl/fi/p5lvr8khvkqnqycwbuu2v/netml_cli_armv7l_demo.tar.gz?rlkey=q50gq1wgcfextx3r6mlrxz59y&dl=0)

This module is a Docker-packaged Debian image for performing anomaly detection on Internet of Things (IoT) network traffic. The anomaly detection algorithms are based on NetML library ([https://github.com/noise-lab/netml](https://github.com/noise-lab/netml))

### Prerequisites
Before proceeding with the deployment, ensure that you have Docker installed and running on your armv7l platform. If not, you can install it using the following commands:

```bash
sudo apt-get update
sudo apt-get install docker.io
```

Once you have Docker installed, follow these steps to run the Docker image for NetML-based IoT traffic classification:
1. Load the Docker image from the .tar.gz file:
```bash
docker load < netml_cli_armv7l_demo.tar.gz
```
2. The `/root` directory contains all .deb files of the necessary libraries for running netml on armv7l. These libraries are already installed on the current image. If you are running another docker image, you can copy those files and use `dpkg` to install .deb files:
```bash
sudo dpkg -i /path/to/library.deb
```
Replace `/path/to/library.deb` with the actual paths of your .deb files.

### Deploying the demo
1. Capture the required amount of IoT traffic for training and save it as a .pcap file (e.g., `train.pcap`). Use the `tcpdump` tool for this:
```bash
tcpdump -i wlan0 -w train.pcap
```
2. Use the `pcap2features.py` script to extract features from the `train.pcap` file. The extracted features are saved to the `normal-features.dat` file:
```bash
python3 pcap2features.py train.pcap
```
3. Run the `feature2model.py` script to create the OCSVM model. The model will be saved as `ocsvm.dat`
```bash
python3 feature2model.py
```
4. To run the anomaly detector in live mode, run the `capture.py` script:
```bash
python3 capture.py ocsvm.dat X
```
Replace `X` with a number of packets after which anomaly detection should be performed. E.g. X=1000 means anomaly classification is performed every 1000 packets. 
5. To run the anomaly detector on already captured traffic, use the `classify.py` script with the .pcap file as input:
```bash
python3 classify.py traffic.pcap
```
Replace `traffic.pcap` with your actual .pcap file name.
