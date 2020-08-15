<p float="left" align = "center">
  <img src="readme-assets/ML_Bridge_Logo.png" width="500"/>
</p>

# Overview

This organisation is the result of my work on a project during Google Summer of 
Code 2020, as a part of the Cloud Native Computing Foundation.

The ML Bridge organisation provides machine learning capabilities to languages 
and platforms that generally have a dearth of such capabilities. 

Currently, it is being used to integrate machine learning capabilities with 
CoreDNS, to protect people against malicious websites and applications. It helps
in identifying websites that could be potentially used by malicious hackers and 
cybercriminals and prevents the user from accessing such websites. 

# Installation Guide

The repositories in the ML Bridge organisation require the Elasticsearch Server.
To install the Elasticsearch Server please follow the instructions that can be 
found in this [link](https://phoenixnap.com/kb/install-elasticsearch-ubuntu).

For installing each component in the ML Bridge organisation please find the 
individual installation instructions in each individual repository. The links 
to the individual repositories can be found below:

- [The ML Bridge Plugin (A CoreDNS Plugin)](https://github.com/mlbridge/coredns-mlbridge)
- [The ML Bridge Middleware](https://github.com/mlbridge/mlbridge-middleware)
- [The ML Bridge User Interface](https://github.com/mlbridge/mlbridge-ui)
- [The ML Bridge Machine Learning Module](https://github.com/mlbridge/mlbridge-machine-learning)

To install and start CoreDNS please take a look at the CoreDNS 
[repository](https://github.com/coredns/coredns). Add the `mlbridge` plugin to
CoreDNS. To add external plugins, please take a look at the 
[example plugin](https://github.com/coredns/example).

Create a new directory `mlbridge` by using the following the command:

```
mkdir mlbridge
```

The recommended file structure while cloning the repositories can be found 
below:

```
mlbridge
   |__ mlbridge-middleware
   |__ mlbridge-ui
   |__ mlbridge-machine-learning
```

To start the ML Bridge software suite, first start the Elasticsearch Server. The
instructions to start the same can be found in this
[link](https://phoenixnap.com/kb/install-elasticsearch-ubuntu).

Next, start the CoreDNS Server. The CoreDNS can be started by following the 
instructions found in this [link](https://github.com/coredns/coredns).

Then, go to the mlbridge directory by using the following command:

```
cd mlbridge
``` 

Finally, execute the following script to start the ML Bridge software suite:

```
python mlbridge-middleware/mlbridge_middleware/src/middleware.py &
python mlbridge-ui/mlbridge_ui/src/ui.py &
python mlbridge-machine-learning/python-code/training/py
```

# Approach

## General Overview

CoreDNS is a DNS Server written in Go. However, Go currently does not have 
native libraries for the interaction with the CUDA platform, which is essential 
for machine learning applications. At the same time, the Python ecosystem has 
tools like TensorFlow, PyTorch, MXNet and various others that not only interact 
with the CUDA platform but also allows for the easy prototyping and evaluation 
of deep learning models.

<p float="left" align = "center">
  <img src="readme-assets/mlbridge_overview.png"/>
</p> 

This project combines the deep learning capabilities that the Python ecosystem 
provides, with CoreDNS, by creating:

- **An ML Bridge Plugin (a CoreDNS Plugin):** The plugin intercepts requests and 
forwards them to the Application Middleware for further processing. The 
repository for the ML Bridge Plugin can be found 
[here](https://github.com/mlbridge/coredns-mlbridge). 

- **An ML Bridge Middleware:** The middleware receives the request from the ML 
Bridge Plugin along with other metadata. The middleware infers whether the 
request is from a malicious or a benign website, from a vetted list. However, if 
the domain is not in the vetted list, a machine learning model infers whether 
the request is malicious or benign, and then sends back a response to the plugin
whether to allow the request or block it. It stores the result as well as other 
metadata to a database. The repository for the ML Bridge Middleware can be 
found [here](https://github.com/mlbridge/mlbridge-middleware).

- **An ML Bridge User Interface:** The user interface is used to analyse 
historical trends as to how many people are accessing malicious websites and at 
what frequency. It is also used to manually vet websites and classify them as 
malicious or benign. The user interface is also used to train new machine 
learning models or retrain existing models by communicating with the ML Bridge 
Machine Learning module. The repository for the ML Bridge User Interface can be 
found [here](https://github.com/mlbridge/mlbridge-ui).

- **An ML Bridge Machine Learning Module:** The machine learning module is used 
to train new models or retrain existing ones. The module receives model 
training information from the details entered by the user in the Training section 
of the ML Bridge User Interface. The model is then trained according to the 
information entered, and then the results of the training are communicated back 
to the ML Bridge User Interface. The ML Bridge User Interface then displays the 
result in the form of accuracy and loss graphs as well as confusion matrices and 
metrics. The repository got the ML Bridge Machine Learning Module can be found
[here](https://github.com/mlbridge/mlbridge-machine-learning). 

## Machine Learning

### Learning Dataset

The deep-learning model is trained on a COVID-19 Cyber Threat Coalition 
Blacklist for malicious domains that can be found 
[here](https://blacklist.cyberthreatcoalition.org/vetted/domain.txt) and on a 
list of benign domains from DomCop that can be found 
[here](https://www.domcop.com/top-10-million-domains).

Currently, the pre-trained model has been trained on the top 500 domain names 
from both these datasets.

### Learning Process

**Data Preprocessing:** Each domain name is converted into a unicode code point 
representation and then extended to a numpy array of a length 256. The dataset 
was created by combining the malicious domains as well as the non-malicious. 
The dataset was split as follows:
- Train Set: 80% of the dataset.
- Validation Set: 10 % of the dataset
- Test Set: 10% of the dataset

**Training:** The deep-learning model is a Convolutional Neural Net that is 
trained using batch gradient descent with the Adam optimizer.











